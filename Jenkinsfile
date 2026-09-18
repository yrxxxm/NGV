// Jenkinsfile
//
// 이 프로젝트의 빌드/정적분석/테스트를 수행하는 Declarative Pipeline입니다.
// 특정 Docker 이미지나 에이전트 라벨을 가정하지 않고, Python 3가 설치된
// 임의의 에이전트에서 venv를 직접 구성해 실행합니다. GitHub Actions
// (.github/workflows/ci.yml)와 동일한 절차를 사내 Jenkins 환경에서도
// 수행하기 위한 것입니다.
//
// 저장소에 아직 Python 소스/테스트가 없을 수 있으므로(초기 단계), 없으면
// 해당 단계를 안전하게 건너뛰고 통과 처리합니다.

pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        VENV_DIR = '.venv'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Detect Sources') {
            steps {
                script {
                    env.HAS_PY = sh(
                        script: "git ls-files '*.py' | grep -q . && echo true || echo false",
                        returnStdout: true
                    ).trim()
                    env.HAS_TESTS = sh(
                        script: "git ls-files 'test_*.py' '*_test.py' 'tests/*.py' 'tests/**/*.py' 2>/dev/null | grep -q . && echo true || echo false",
                        returnStdout: true
                    ).trim()
                    echo "HAS_PY=${env.HAS_PY} HAS_TESTS=${env.HAS_TESTS}"
                }
            }
        }

        stage('Setup Python') {
            when { expression { env.HAS_PY == 'true' } }
            steps {
                sh '''
                    set -e
                    python3 -m venv "${VENV_DIR}"
                    . "${VENV_DIR}/bin/activate"
                    pip install --upgrade pip
                    if [ -f requirements.txt ]; then
                        pip install -r requirements.txt
                    fi
                    pip install flake8 pylint radon coverage unittest-xml-reporting
                '''
            }
        }

        stage('Build') {
            when { expression { env.HAS_PY == 'true' } }
            steps {
                sh '''
                    set -e
                    . "${VENV_DIR}/bin/activate"
                    python -m compileall -q .
                '''
            }
        }

        stage('Static Analysis') {
            when { expression { env.HAS_PY == 'true' } }
            steps {
                sh '''
                    set -e
                    . "${VENV_DIR}/bin/activate"
                    flake8 --max-line-length=120 .
                    pylint --disable=all --enable=duplicate-code \
                        --min-similarity-lines=7 $(git ls-files '*.py')

                    VIOLATIONS=$(radon cc --min C -s .)
                    if [ -n "$VIOLATIONS" ]; then
                        echo "$VIOLATIONS"
                        echo "Cyclomatic complexity exceeds the project limit of 10."
                        exit 1
                    fi
                '''
            }
        }

        // CLAUDE.md 기준: 단위테스트 분기 커버리지 100%, 테스트 성공률 100%.
        // src/tests 레이아웃이 정리되면 단위 테스트 디렉터리로 범위를 좁히는
        // 것을 권장한다 — 현재는 발견되는 전체 테스트 스위트에 적용한다.
        stage('Test') {
            when { expression { env.HAS_TESTS == 'true' } }
            steps {
                sh '''
                    set -e
                    . "${VENV_DIR}/bin/activate"
                    mkdir -p test-reports
                    coverage run --branch -m xmlrunner discover -o test-reports
                    coverage report -m
                    coverage xml
                    coverage report --fail-under=100
                '''
            }
        }

        stage('Skip Notice') {
            when { expression { env.HAS_PY == 'false' } }
            steps {
                echo 'No Python source files found yet - skipping build/static analysis/tests.'
            }
        }
    }

    post {
        always {
            script {
                if (env.HAS_TESTS == 'true') {
                    junit allowEmptyResults: true, testResults: 'test-reports/*.xml'
                    archiveArtifacts artifacts: 'coverage.xml', allowEmptyArchive: true
                }
            }
        }
    }
}

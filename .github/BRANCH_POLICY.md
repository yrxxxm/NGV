# Git 브랜치 정책

이 문서는 AI 기반 개발("바이브 코딩")과 GitHub를 함께 사용할 때 일반적으로
적용되는 브랜치 전략을 이 저장소에 맞게 정리한 것입니다. `main` 브랜치
보호 규칙은 GitHub 저장소 설정(웹 UI 또는 `gh` CLI)에서 별도로 적용해야
하며, 이 문서는 그 설정값의 근거와 절차를 남기기 위한 것입니다(**적용
상태**는 문서 하단 참고).

## 1. 브랜치 전략 — GitHub Flow 기반

- `main`은 항상 배포 가능한 상태를 유지하는 트렁크 브랜치입니다.
- 모든 변경은 **브랜치 생성 → Pull Request → CI 통과/리뷰 → 병합** 절차를
  따르며, `main`에 직접 커밋/푸시하지 않습니다.
- 브랜치 명명 규칙:
  - `feature/<설명>` — 신규 기능
  - `fix/<설명>` — 결함 수정
  - `docs/<설명>` — 문서/스킬·에이전트 정의 변경
  - `refactor/<설명>` — 동작 변경 없는 리팩터링
  - `chore/<설명>` — 빌드/CI/설정 등 기타 변경
  - 이슈 번호가 있으면 접두로 붙입니다: `feature/123-add-retry-logic`

## 2. `main` 브랜치 보호 규칙 (GitHub 저장소 설정에서 적용)

**GitHub → 저장소 → Settings → Branches → Branch protection rules → `main`**
에서 아래 항목을 설정합니다:

- **Require a pull request before merging** — `main` 직접 푸시 금지.
  - Require approvals: **1** (1인 개발 프로젝트라면 0으로 낮출 수 있으나,
    "Do not allow bypassing the above settings"는 관리자에게도 유지 권장)
- **Require status checks to pass before merging**
  - 필수 체크: `CI / Build, Test, Static Analysis` (`.github/workflows/ci.yml`의
    워크플로우 이름 / 잡 이름)
  - Require branches to be up to date before merging
- **Require conversation resolution before merging**
- **Do not allow force pushes** (`main` 대상)
- **Do not allow deletions** (`main` 대상)
- (선택, 권장) Require signed commits

기능/수정 브랜치에는 이 규칙을 적용하지 않습니다 — 작업 중 자유롭게
rebase/force-push할 수 있어야 합니다.

## 3. 커밋/PR 컨벤션

- 커밋 메시지는 [Conventional Commits](https://www.conventionalcommits.org/)
  형식을 권장합니다: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`.
  AI 세션이 생성하는 다수의 작은 커밋을 병합 이력에서 구분하기 쉬워집니다.
- 병합 방식은 **Squash and merge**를 기본으로 합니다 — 하나의 PR(논리적
  변경 단위)이 `main`에는 하나의 커밋으로 남습니다.
- 병합 후 브랜치는 자동 삭제합니다(저장소 Settings → General → "Automatically
  delete head branches" 활성화).

## 4. CI 연동

- PR을 생성/갱신하면 `.github/workflows/ci.yml`이 자동 실행되어 빌드
  (구문 검사)/정적분석/테스트를 수행합니다.
- 2절의 "Require status checks to pass before merging"로 이 CI 결과를
  병합 필수 조건으로 지정합니다 — CI가 실패한 PR은 병합할 수 없습니다.
- CI가 검증하는 프로젝트 정량 품질 기준(CLAUDE.md 기준)은 다음과 같습니다:
  함수 50라인 이하, 순환복잡도 10 이하, 중복 코드 7라인 이하, Doxygen 주석
  20% 이상, 단위테스트 분기 커버리지 100%, 테스트 성공률 100%.
- 저장소에 아직 Python 소스/테스트가 없는 동안에는 해당 단계를 안전하게
  건너뛰고 통과 처리합니다(코드가 추가되면 자동으로 활성화됩니다).

## 5. Jenkins 연동 (선택)

`Jenkinsfile`은 GitHub Actions와 동일한 빌드/정적분석/테스트 절차를
사내 Jenkins 환경에서도 수행할 수 있도록 별도로 제공합니다. 두 파이프라인
중 하나만 사용해도 되고, 이행 기간 동안 병행할 수도 있습니다.

## 적용 상태

- [x] `.github/workflows/ci.yml` — 작성 완료, 커밋되면 즉시 활성화됩니다.
- [x] `Jenkinsfile` — 작성 완료.
- [ ] **2절의 `main` 브랜치 보호 규칙은 아직 GitHub 저장소 설정에 실제로
      적용되지 않았습니다.** 이 머신에 `gh` CLI가 설치/인증되어 있지 않아
      자동 적용을 보류했습니다. GitHub 웹 UI에서 위 항목을 직접 설정하거나,
      `gh` CLI 설치·인증(`gh auth login`) 후 다시 요청하면 API로 적용할 수
      있습니다.

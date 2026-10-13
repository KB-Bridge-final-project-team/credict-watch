# GitHub 협업 가이드

처음 GitHub로 협업하는 팀원을 위한 안내입니다. 명령어는 프로젝트 폴더의 PowerShell 또는 VS Code 터미널에서 실행합니다.

**기본 흐름: Issue 등록 → 최신 main 받기 → 작업별 브랜치 → 수정·확인 → Commit·Push → PR → 팀원 검토 → 병합**

## 1. 담당 역할과 작업 원칙

| 역할 | 담당 업무 |
|---|---|
| 개발 담당 | 데이터 수집 및 자동화 파이프라인, 웹페이지 개발 |
| 재무제표 담당 | 금융 지식 공유, 재무지표 정의, 분석 결과 평가, 방향 조정 |
| 데이터 분석 담당 | 전처리, EDA, 머신러닝·딥러닝, LLM 적용 검토 및 구현 |
| 공통 | 문서 작성, 결과물 검토, Notion 및 포트폴리오 정리 |

- 역할은 주 담당 영역입니다. 다른 영역의 수정이 필요하면 해당 담당자와 먼저 공유합니다.
- **사람이나 역할마다 고정 브랜치를 두지 않고, 작업 하나마다 새 브랜치를 만듭니다.**
- 한 브랜치는 한 명이 주로 관리합니다. 같은 파일을 동시에 수정해야 한다면 범위와 순서를 먼저 합의합니다.
- main에는 검토가 끝난 변경을 모읍니다. main에 직접 Push하거나 GitHub에서 직접 편집해 Commit하지 않습니다.
- PR(Pull Request)은 내 브랜치의 변경을 main에 반영해 달라는 검토 요청입니다.

```text
main에서 시작 → feat/12-data-collection에서 작업 → PR → 검토 → main 병합
main에서 시작 → docs/15-financial-metrics에서 작업 → PR → 검토 → main 병합
```

## 2. 저장소와 폴더 구조

저장소: https://github.com/KB-Bridge-final-project-team/credict-watch

2026-10-13 확인한 최상위 구조입니다.

```text
credict-watch/
├── data_pipeline/   # 데이터 수집 및 자동화
├── frontend/        # 웹페이지
├── analysis/        # 데이터 전처리 및 EDA
├── docs/            # 프로젝트 문서
├── .gitignore       # Git에 올리지 않을 파일 규칙
└── CONTRIBUTING.md  # 협업 가이드
```

`models/`, `finance/`, `README.md`는 확인 시점에 최상위에 없습니다. 필요한 경우 Issue에서 용도를 합의하고 추가합니다. 실행 방법과 환경 설정은 README에 정리하는 것을 권장합니다.

## 3. 최초 1회 준비

1. Git 설치 후 터미널을 다시 열고 `git --version`으로 확인합니다.
2. 저장소 관리자에게 협업 초대를 받아 수락합니다. 이 가이드는 팀원이 같은 저장소에 브랜치를 Push할 권한이 있는 방식을 기준으로 합니다.
3. Commit에 표시할 이름과 이메일을 설정합니다. 아래 값은 본인 정보로 바꿉니다. 이메일 공개를 피하려면 GitHub Settings → Emails의 본인 noreply 주소를 사용합니다.

```powershell
git config --global user.name "본인 이름"
git config --global user.email "본인 GitHub 이메일"
```

위 설정은 이 컴퓨터의 다른 저장소에도 적용됩니다. 저장소별 설정이 필요하면 Clone 후 해당 폴더에서 `--global`을 빼고 실행합니다. 이름·이메일 설정은 GitHub 로그인과 별개입니다.

프로젝트를 둘 상위 폴더에서 다음 명령을 실행합니다.

```powershell
git clone https://github.com/KB-Bridge-final-project-team/credict-watch.git
cd credict-watch
git remote -v
git status
```

- `origin` 주소가 위 저장소인지 확인합니다. ZIP 다운로드는 Git 이력이 없으므로 협업용으로는 Clone을 사용합니다.
- Push할 때 인증이 필요하면 Git이 안내하는 브라우저 로그인 절차를 따릅니다. GitHub 계정 비밀번호를 토큰 대신 입력하지 않습니다.
- 가상환경, 패키지, API 설정은 프로젝트 실행 안내에 따라 준비합니다. Clone만으로 실행 환경까지 설치되지는 않습니다.

## 4. 작업 순서

### STEP 1. Issue 등록

GitHub → Issues → New issue에서 할 일을 적고 담당자를 지정합니다.

```text
제목: [DATA] 재무제표 데이터 수집
목적: 어떤 문제를 해결하는가?
작업 범위: 수정할 기능과 파일
완료 기준: 어떤 상태면 완료인가?
담당자 / 의존 작업: 누구와 맞춰야 하는가?
```

작업 전 기존 Issue를 확인해 중복 작업을 피합니다. 아래에서는 예시 Issue 번호를 12로 사용합니다. 본인 번호로 바꿉니다.

### STEP 2. 최신 main에서 브랜치 생성

```powershell
git status
git switch main
git pull --ff-only origin main
git switch -c feat/12-data-collection
git branch --show-current
```

**첫 `git status`에서 변경 파일이 있으면 먼저 처리하고 다음 명령으로 넘어갑니다.** 이전 작업이라면 그 작업 브랜치에서 필요한 파일만 Commit합니다. 저장하지 않은 변경을 새 브랜치에 섞거나, 오류를 없애려고 삭제하지 않습니다.

`--ff-only`는 로컬 main과 원격 main의 이력이 갈라지면 자동 병합하지 않고 멈춥니다. 오류가 나면 아래 문제 해결 절차를 확인합니다.

| 작업 | 브랜치 예시 |
|---|---|
| 데이터 수집 | `feat/12-data-collection` |
| 웹페이지 개발 | `feat/18-dashboard` |
| 오류 수정 | `fix/21-api-timeout` |
| 재무지표 문서 | `docs/15-financial-metrics` |
| 환경 설정 | `chore/23-python-env` |

브랜치명은 영문 소문자·숫자·하이픈을 권장합니다. 이미 진행 중인 작업은 새 브랜치를 만들지 않고 기존 브랜치로 이동합니다.

### STEP 3. 수정하고 로컬에서 확인

- 변경한 기능을 실행해 확인합니다. 문서라면 링크·명령어·오탈자를 확인합니다.
- 여러 기능을 한 PR에 섞지 않습니다. 불필요한 파일 전체 포맷 변경도 피합니다.
- 실행 방법이나 필요한 패키지가 달라지면 관련 안내·의존성 파일을 함께 수정합니다.

### STEP 4. 변경 확인 후 Commit·Push

다음은 `docs/financial-metrics.md`를 수정한 경우의 예시입니다. 파일 경로와 메시지를 본인 작업에 맞게 바꿉니다.

```powershell
git status
git diff
git add docs/financial-metrics.md
git diff --cached
git commit -m "docs: 재무지표 정의 추가"
git push -u origin HEAD
```

- `git diff`는 아직 추가하지 않은 추적 파일 변경을 보여줍니다. 새 파일은 편집기에서 내용도 확인합니다.
- `git add`는 이번 Commit에 넣을 파일을 선택합니다. `git diff --cached`로 선택한 변경을 최종 확인합니다.
- `git add .`는 현재 폴더 아래의 변경과 삭제를 함께 선택하므로, 초반에는 파일을 지정하는 방식을 권장합니다.
- Commit은 내 컴퓨터에 이력을 저장하고, Push는 GitHub에 올립니다. Commit만 하면 팀원은 볼 수 없습니다.
- `HEAD`는 현재 브랜치를 뜻합니다. Push 전 `git branch --show-current`로 main이 아닌지 확인합니다.
- 최초 Push 이후 같은 브랜치에서는 `git push`를 사용합니다.

| 구분 | 용도 | 예시 |
|---|---|---|
| `feat` | 기능 추가 | `feat: 재무제표 수집 구현` |
| `fix` | 오류 수정 | `fix: API 응답 누락 처리` |
| `docs` | 문서 수정 | `docs: 재무지표 정의 추가` |
| `refactor` | 동작 유지하며 구조 개선 | `refactor: 전처리 함수 분리` |
| `test` | 테스트 추가·수정 | `test: 결측값 처리 검증 추가` |
| `chore` | 환경·관리 설정 | `chore: gitignore 수정` |

### STEP 5. PR 생성과 팀원 검토

1. 저장소의 **Compare & pull request**를 클릭합니다. 버튼이 없으면 Pull requests → New pull request를 사용합니다.
2. **base: main**, **compare: 내 작업 브랜치**인지 확인합니다.
3. Files changed에서 의도한 파일만 포함됐는지 확인합니다.
4. 아래 형식으로 설명을 작성하고 다른 팀원에게 리뷰를 요청합니다. 진행 중이면 Draft PR을 사용합니다.

```markdown
## 목적
- 이 변경이 필요한 이유

## 변경 내용
- 무엇을 바꿨는지

## 확인 방법과 결과
- 실행 명령 또는 확인 절차:
- 실제 결과:
- 확인하지 못한 항목과 이유:

## 팀원에게 알릴 내용
- 필요한 환경 설정, 데이터/API 형식 변경, 후속 작업

Closes #12
```

`Closes #12`는 해당 Issue를 해결하는 PR이 기본 브랜치에 병합되면 Issue를 자동 종료하는 연결 표현입니다. 일부만 처리했다면 `Refs #12`로 참조하고 Issue를 유지합니다.

**리뷰어는 다음을 확인합니다.**

- 목적과 변경 범위가 맞는가?
- 설명한 확인 결과가 있고, 필요한 기능을 실행할 수 있는가?
- 다른 팀원의 코드·데이터 형식과 연결되는가?
- 비밀정보, 개인정보, 불필요한 데이터·산출물이 포함되지 않았는가?
- 분석 결과라면 데이터 출처·기준일·평가 방법·한계가 설명되어 있는가?

수정 요청을 받으면 **같은 브랜치에서 수정 → Commit → Push**합니다. 기존 PR이 자동으로 갱신되므로 새 PR을 만들 필요가 없습니다. 반영한 내용을 PR 댓글로 알려줍니다.

### STEP 6. 병합과 다음 작업

아래를 팀 운영 규칙으로 권장합니다.

- 작성자 외 팀원 최소 1명이 확인하고 Approve합니다.
- 수정 요청·충돌을 해결하고, 자동 검사가 있다면 통과 여부를 확인합니다.
- 병합은 PR 작성자 또는 지정한 담당자가 맡습니다. PR 댓글로 병합 여부를 공유합니다.
- 초반에는 **Merge pull request 방식**으로 통일합니다. 서로 다른 병합 방식을 섞으면 이력과 브랜치 정리가 헷갈릴 수 있습니다.

병합 완료 후 로컬 변경이 없는지 확인하고 main을 갱신합니다.

```powershell
git status
git switch main
git pull --ff-only origin main
git branch -d feat/12-data-collection
```

GitHub의 **Delete branch**는 원격 작업 브랜치를, 위 `-d`는 로컬 작업 브랜치를 정리합니다. 병합 완료와 팀원이 사용 중이지 않음을 확인한 뒤 정리합니다.

`-d`가 거부되면 강제 삭제하지 않습니다. Squash/Rebase 병합을 쓴 경우에도 거부될 수 있으므로 PR 병합 여부와 남은 변경을 확인합니다. 다음 작업은 최신 main에서 새 브랜치를 만듭니다.

## 5. 작업 중 main이 바뀌었을 때와 충돌 해결

다른 팀원의 PR이 먼저 병합되면 내 작업 브랜치에도 최신 main을 반영해야 할 수 있습니다. 아래는 **내 작업을 Commit하고 `git status`에 미저장 변경이 없는 상태**에서 진행합니다.

```powershell
git switch feat/12-data-collection
git fetch origin
git merge origin/main
```

충돌 없이 완료되면 실행을 다시 확인하고 `git push`합니다. 편집기가 열리면 병합 메시지를 저장하고 닫아 완료합니다.

충돌은 같은 부분에 서로 다른 변경이 있어서 Git이 선택하지 못하는 상황입니다.

1. `git status`에서 충돌 파일을 확인합니다.
2. 담당자와 두 변경의 의도를 확인합니다. VS Code의 Current/Incoming 중 하나를 무조건 선택하지 않습니다.
3. 아래 표시를 제거하고 필요한 내용을 합친 최종 코드로 저장합니다.

```text
<<<<<<< HEAD
내 브랜치의 내용
=======
main에서 가져온 내용
>>>>>>> origin/main
```

4. 충돌 파일을 지정해 추가하고 병합을 완료합니다. 아래 경로는 실제 충돌 파일로 바꿉니다.

```powershell
git add docs/financial-metrics.md
git diff --cached
git commit -m "chore: 최신 main 반영 및 충돌 해결"
```

5. 기능 실행·데이터 연결을 다시 확인한 뒤 `git push`합니다. 기존 PR도 갱신됩니다.

해결 방향이 불확실하면 **진행 중인 병합 상태에서** `git merge --abort`로 병합을 취소하고 담당자와 확인합니다. 이 절차는 병합 전 작업이 Commit되어 있는 상태를 전제로 합니다.

## 6. 비밀정보·데이터·노트북 주의사항

- `.env`, API Key, 비밀번호, 인증서, DB 접속 문자열을 올리지 않습니다. 코드뿐 아니라 노트북 출력·로그·스크린샷·PR 설명에도 남기지 않습니다.
- `.env.example`에는 필요한 변수 이름과 가짜 예시만 작성합니다. 실제 값은 별도로 안전하게 공유합니다.
- **이미 Git이 추적하는 파일은 `.gitignore`에 추가해도 추적이 해제되지 않습니다.** 올라간 파일을 지우는 것만으로 과거 Commit에서도 사라지지 않습니다.
- 키가 올라갔다면 즉시 해당 키를 폐기·재발급하고 팀에 알립니다. 이력 정리는 모든 팀원의 작업에 영향을 주므로 혼자 강제 Push하지 않습니다.
- 개인정보, 사용 권한이 없는 데이터, DB 전체 덤프를 업로드하지 않습니다. 공개 가능한 작은 샘플과 데이터 설명을 공유합니다.
- 대용량 원본 데이터와 학습 모델은 별도 저장 위치를 합의합니다. Git에는 다운로드·생성 방법과 데이터 버전·출처를 기록합니다.
- 현재 `.gitignore`는 `data/raw/`, `*.pkl`, `*.joblib`, `*.h5`, `*.pt` 등을 제외합니다. **CSV·Excel·다른 경로의 원본 데이터까지 모두 제외하는 규칙은 아닙니다.** 파일을 올리기 전에 경로와 내용을 확인합니다.
- `.venv/`, `node_modules/`, 캐시를 올리지 않습니다. 필요한 패키지는 의존성 파일로 공유합니다.
- 노트북은 Commit 전 민감하거나 불필요하게 큰 출력을 정리합니다. 같은 `.ipynb`의 동시 편집은 피하고, 재사용 함수는 `.py`로 분리하는 것을 권장합니다.

## 7. 이 프로젝트에서 먼저 합의할 내용

Git 충돌이 없어도 데이터·코드가 서로 맞지 않으면 협업 결과를 실행할 수 없습니다.

| 항목 | 합의할 내용 |
|---|---|
| 데이터 연결 | 컬럼명, 자료형, 기업 식별자, 날짜 형식, 금액 단위, 결측값 표현 |
| 재무지표 | 계산식, 연결/별도 기준, 기간, 해석과 예외 처리 |
| API 연결 | 요청·응답 형식, 오류 응답, 담당자 |
| 분석 재현 | 데이터 버전·출처, 전처리 순서, 학습/평가 분리, 난수 시드, 평가 지표 |
| 환경 | Python/Node 버전, 설치 방법, 필요한 환경변수, 실행 명령 |
| 공용 DB | 테스트 범위, 변경 담당자, 테이블·데이터 수정 전 공유 절차 |

합의 내용은 `docs/`에 남기고, 설치·실행의 시작점은 README에 정리합니다. 컬럼·함수·API 형식을 바꿀 때는 연결된 담당자에게 먼저 알리고 함께 확인합니다. Git 브랜치를 나눠도 공용 DB와 외부 API의 실제 상태는 분리되지 않습니다.

## 8. 자주 막히는 상황

| 상황 | 대응 |
|---|---|
| 브랜치 변경 시 덮어쓰기 오류 | `git status`로 변경 확인. 현재 작업 브랜치에서 필요한 변경을 Commit한 후 이동 |
| `git pull --ff-only` 실패 | 로컬/원격 이력이 갈라졌을 수 있음. `git status`, `git log --oneline --graph -10` 결과를 팀에 공유하고 확인 |
| Push가 `non-fast-forward`로 거부됨 | 같은 원격 브랜치가 갱신됐을 수 있음. 작업을 Commit한 뒤 `git fetch origin`, 이력·담당자 확인 후 해당 원격 브랜치를 병합. 강제 Push 금지 |
| `Permission denied` / 403 | 로그인 계정, 초대 수락 여부, 저장소 쓰기 권한, origin 주소 확인 |
| `nothing to commit` | 파일 저장 여부와 `git status` 확인. 제외 여부는 `git check-ignore -v 파일경로`로 확인 |
| main에서 실수로 파일 수정 | 아직 Commit하지 않았다면 `git switch -c fix/새작업이름`으로 변경을 새 브랜치에 옮김 |
| main에서 실수로 Commit | Push하지 말고 팀에 공유. 브랜치 보존·이력 확인 후 복구하며 임의로 초기화하지 않음 |
| 아직 진행 중인 작업 공유 | 작업 브랜치에 Commit·Push하고 Draft PR로 진행 상황 공유 |

오류를 질문할 때는 **실행한 명령, 오류 전문, 현재 브랜치, `git status` 결과**를 함께 전달합니다. 비밀정보는 가립니다.

`git push --force`, `git reset --hard`, `git clean -fd`, `git branch -D`는 이력을 덮거나 작업 파일·브랜치를 없앨 수 있습니다. 오류 해결용으로 복사해 실행하지 말고, 무엇이 사라지는지 확인한 뒤 팀과 합의합니다.

## 9. 저장소 관리자 설정 권장 사항

아래는 **설정 완료를 뜻하지 않는 권장안**입니다. 관리자가 저장소 Settings의 Rulesets 또는 Branch protection에서 적용 가능한 기능을 확인합니다.

- main 변경에 PR을 요구합니다.
- 작성자 외 승인 1명 이상을 요구하고, 리뷰 후 추가 변경 시 다시 확인하도록 설정합니다.
- main의 강제 Push와 삭제를 제한합니다.
- 자동 검사를 도입했다면 실제 동작하는 검사만 필수로 지정합니다.
- 팀원의 접근 권한을 확인하고, 설정 가능한 경우 관리자 우회도 제한합니다.

GitHub 요금제와 저장소 공개 범위에 따라 설정 가능 여부가 달라질 수 있습니다. 설정 전까지도 팀 규칙으로 PR·리뷰 절차를 지킵니다.

## 10. Commit·PR 전 체크리스트

- [ ] 내 작업 브랜치에서 작업했다.
- [ ] 변경 파일과 내용을 확인했고, 비밀정보·불필요한 데이터가 없다.
- [ ] 실제 실행 또는 문서 확인 결과를 PR에 적었다.
- [ ] 데이터·API·환경 변경을 관련 담당자에게 알렸다.
- [ ] PR 대상이 main이고 다른 팀원에게 리뷰를 요청했다.
- [ ] 병합 후 최신 main을 받고 다음 작업은 새 브랜치에서 시작한다.

## 참고 문서

- [GitHub의 PR과 협업 방식](https://docs.github.com/en/pull-requests/reference/pull-requests)
- [민감정보가 저장소에 올라갔을 때의 대응](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)

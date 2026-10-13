# 🤝 GitHub 협업 가이드

## 1. 담당 역할

| 역할 | 담당 업무 |
|---|---|
| 💻 개발 담당 | 데이터 수집 및 자동화 파이프라인, 웹페이지 개발 |
| 📊 재무제표 담당 | 금융 전문지식 공유, 분석 결과 평가, 프로젝트 방향 조정 |
| 🤖 데이터 분석 담당 | 데이터 전처리, 머신러닝·딥러닝, LLM 적용 검토 및 구현 |
| 👥 공통 | 문서 작성, 결과물 검토, Notion 및 포트폴리오 정리 |

## 2. 협업 방식

**각자 브랜치에서 작업 → GitHub 업로드 → Pull Request → 검토 후 병합**

```text
              main (최종 코드)
                    │
        ┌───────────┼───────────┐
        │           │           │
   feat/dev    feat/finance  feat/analysis
     개발         재무제표       데이터 분석
        │           │           │
        └───────────┼───────────┘
                    ▼
               Pull Request
                    ▼
                 코드 검토
                    ▼
                main 병합
```

> ⚠️ `main` 브랜치에 직접 수정하지 않고 PR을 통해 병합합니다.

## 3. 프로젝트 폴더 구조

```text
finance-ai-project/
│
├── data_pipeline/   # 데이터 수집 및 자동화
├── frontend/        # 웹페이지
├── analysis/        # 데이터 전처리 및 EDA
├── models/          # AI 모델 및 LLM
├── finance/         # 재무지표, 금융 지식, 평가 기준
├── docs/            # 회의록 및 프로젝트 문서
│
├── .gitignore
└── README.md
```

## 4. GitHub 작업 순서

### STEP 1. 최초 1회: 프로젝트 다운로드

```bash
git clone <저장소_URL>
cd finance-ai-project
```

### STEP 2. 작업 시작: 최신 코드 가져오기

```bash
git switch main
git pull origin main
```

### STEP 3. 작업용 브랜치 생성

```bash
git switch -c feat/작업이름
```

**브랜치 이름 예시**

| 작업 | 브랜치 |
|---|---|
| 데이터 수집 | `feat/data-collection` |
| 웹페이지 개발 | `feat/frontend` |
| 재무지표 정리 | `docs/financial-metrics` |
| AI 모델 개발 | `feat/ai-model` |

> 하나의 기능을 작업할 때마다 새 브랜치를 생성합니다.

### STEP 4. 작업 완료 후 GitHub 업로드

```bash
git add .
git commit -m "feat: 작업 내용"
git push -u origin 브랜치이름
```

**Commit 메시지 규칙**

| 구분 | 용도 | 예시 |
|---|---|---|
| `feat` | 기능 추가 | `feat: 데이터 수집 구현` |
| `fix` | 오류 수정 | `fix: API 호출 오류 해결` |
| `docs` | 문서 수정 | `docs: 재무지표 정의 추가` |
| `refactor` | 코드 개선 | `refactor: 전처리 함수 분리` |
| `chore` | 환경 설정 | `chore: gitignore 수정` |

### STEP 5. Pull Request(PR) 생성

1. GitHub 저장소 접속
2. **Compare & pull request** 클릭
3. 변경 내용 작성 후 PR 생성
4. 다른 팀원 검토
5. 문제없으면 **Merge pull request** 클릭

```text
개인 브랜치 → PR 생성 → 팀원 확인 → main 병합
```

### STEP 6. 다음 작업 시작

```bash
git switch main
git pull origin main
git switch -c feat/새작업이름
```

위 과정을 반복합니다.

---

## 5. 협업 규칙

- ✅ 작업 시작 전 `git pull`로 최신 코드 받기
- ✅ 기능별 브랜치 생성하기
- ✅ 작업 완료 후 Commit 및 Push
- ✅ PR 생성 후 다른 팀원 검토받기
- ✅ 작업 내용은 GitHub Issues로 관리하기
- ✅ API 및 데이터 형식 변경 시 팀원에게 공유하기
- ❌ `main`에 직접 Push 금지
- ❌ `.env`, API Key, 비밀번호 업로드 금지
- ❌ 다른 팀원의 작업 파일을 합의 없이 수정하지 않기

## 6. GitHub Issues 사용법

작업할 내용을 Issue로 등록하고 담당자를 지정합니다.

```text
[Issue 생성]
     ↓
담당자 지정
     ↓
브랜치 생성 및 작업
     ↓
Pull Request 생성
     ↓
병합 후 Issue 완료
```

**Issue 제목 예시**

- `[DATA] 재무제표 데이터 수집`
- `[FINANCE] 재무비율 분석 기준 정리`
- `[AI] 재무 위험도 예측 모델`
- `[WEB] 분석 결과 대시보드`

## 7. 자주 사용하는 명령어

| 명령어 | 설명 |
|---|---|
| `git status` | 현재 변경 사항 확인 |
| `git branch` | 현재 브랜치 확인 |
| `git switch main` | main 브랜치로 이동 |
| `git pull origin main` | 최신 main 코드 가져오기 |
| `git switch -c feat/name` | 새 브랜치 생성 |
| `git add .` | 변경 파일 추가 |
| `git commit -m "내용"` | 변경 내역 저장 |
| `git push -u origin 브랜치명` | 브랜치 GitHub 업로드 |

---

### 📌 핵심 요약

**`최신 코드 받기 → 브랜치 생성 → 작업 → Commit → Push → PR → Merge`**

이 순서만 지키면 됩니다.

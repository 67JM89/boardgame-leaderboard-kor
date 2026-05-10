# 🎲 보드게임 리더보드 / Boardgame Leaderboard

**[🔗 Live](https://67jm89.github.io/boardgame-leaderboard-kor/)** &nbsp;·&nbsp; 🔒 **Private Pages** (GitHub Enterprise, collaborator + 로그인 필요)

소규모 보드게임 모임의 결과를 기록·분석하는 단일 페이지 대시보드. 로컬 Excel(`data/board.xlsx`) → GitHub Pages 자동 배포 워크플로우로 운영됩니다.

A single-page dashboard for tracking & analyzing small-group boardgame results. Powered by a local Excel file (`data/board.xlsx`) auto-deployed via GitHub Pages.

---

## 📦 구성 / Contents

```
boardgame-leaderboard-kor/
├── index.html              # 메인 대시보드 (현재 활성 버전)
├── index.v2~v8.html        # 이전 버전 스냅샷 (참고용)
├── data/
│   └── board.xlsx          # 게임 플레이 기록 (master data)
├── .github/
│   ├── workflows/
│   │   └── validate-xlsx.yml  # xlsx 무결성 자동 검증 CI
│   ├── CODEOWNERS              # 파일 오너
│   └── dependabot.yml          # GitHub Actions 버전 자동 업데이트
├── docs/
│   └── DEVOPS.md           # Branch Protection / Environments / PR Preview 가이드
└── README.md               # 이 문서
```

---

## ✨ 주요 기능 / Features

### 🏆 리더보드 & 통계
- **전체 / 게임별 / 월별** 리더보드 (정렬 가능 컬럼)
- **Tier Points vs Reward Points** 이원 점수 체계 (게임 실력 vs 교환 가능 지갑)
- **출석 보너스** (출석 체크 / HSM Running Club)
- **메달 시스템** (금/은/동 + 꼴찌/꼴찌 직전 빨강 마커)
- **반응형 정렬**: 컬럼 클릭 시 desc → asc → default 토글

### 🆕 V6 — 분석/탐색
- 🥊 **H2H 대결**: 두 플레이어 함께 참가한 세션의 승/무/패 통계
- 🔥 **히트맵**: 플레이어 × 게임 격자, 승률/평균순위/평균점수 3-mode 색상 그라디언트
- 🆕 **최근 모임 카드**: 가장 최근 세션의 게임별 우승자 요약
- ▲▼ **순위 변동 화살표**: 직전 세션 대비 ±N 표시
- ⏱ **카운트업 애니메이션**: 첫 로드 시 통계가 0→실제값으로

### 🛠 V7 — 실용/폴리시
- 🆕 **자동 업데이트 감지**: ETag 폴링 90초마다 → 배너로 "새 버전 있음" 알림 (hard refresh 불필요)
- 📷 **PNG 공유**: 헤더 공유 버튼으로 전체 페이지를 PNG로 다운로드
- 🖨 **Print Stylesheet**: `Ctrl+P` 시 리더보드만 깔끔한 출력
- 🏷️ **헤더 버전 칩** (`v8.6` 등) — 캐시 디버깅용

### 🏅 V8 — 챔피언십 & 인사이트
- 🏆 **챔피언십 탭**: 시즌(분기별) 챔피언 → 명예의 전당(공동 1위 지원) → ELO Top 10
- 💡 **자동 인사이트 캐러셀**: 데이터에서 발견한 1줄 사실 회전 (6.5초 간격)
  - 예: "Julia님이 이번 달 Tier +12.5 상승", "Catan은 최근 30일 최다 플레이"
- ⚡ **Personal Records**: 플레이어 모달에 게임별 단일 세션 최고 기록
- 📊 **모바일 분석 탭 압축**: 4개 분석 탭 → `📊 분석` 드롭다운으로 압축
- 🎨 **컬럼 군별 음영**: Tier=보라 wash / Reward=파랑 wash로 시각 그룹 강조

### 🔒 보안 / Security
- **CSP** (Content Security Policy): jsDelivr CDN만 허용
- **SRI** (Subresource Integrity): CDN 파일 변조 자동 차단
- **SHA-256 해시 비밀번호** (XLSX 업로드 보호, 5회 실패 시 5분 잠금)
- **Private Pages** (GitHub Enterprise): 로그인한 collaborator만 접근

---

## 🚀 데이터 갱신 워크플로우 / Update Workflow

### Claude Code 사용자
```
1. data/board.xlsx 편집 + 저장 (Excel 닫기)
2. Claude Code에서  /private-repo_hsm_boardgame
3. 자동: git add + commit + push → Pages 1~2분 후 자동 배포
```

### 수동 (terminal)
```bash
# 1. xlsx 편집/저장 + Excel 닫기
# 2. 아래 명령 실행
git add data/board.xlsx
git commit -m "Update data: $(date '+%Y-%m-%d %H:%M')"
git push origin main
```

### End User (친구들)
- GitHub 로그인 + 저장소 collaborator 권한 필요 (Private repo)
- 페이지 첫 접속 시 GitHub 인증 → `refactored-giggle-XXXX.pages.github.io` 로 자동 리다이렉트 (정상)
- 새 버전 배포되면 90초 내 우측 하단 배너 자동 표시 → "새로고침" 클릭

---

## 📋 점수 체계 / Scoring System

### Tier Points (게임 실력 점수)
**순위를 결정하는 핵심 점수.** 음수 가능.

```
Tier Points = 누적 승점 + 누적 감점
```

- 게임 행에서만 계산 (출석 보너스 무관)
- 사용/교환은 영향 없음 → **교환해도 순위 안 떨어짐**

### Reward Points (교환 가능 포인트, 지갑)
**상품 교환용.** 최소 0 (음수 floor).

```
Reward Points = Tier + 출석 보너스 − 사용한 포인트 − 교환
```

### ELO Rating (V8)
체스 ELO와 같은 페어와이즈 정통 실력 점수. 모두 1000부터 시작.
- 같은 세션 내 모든 플레이어 쌍에 대해 업데이트
- K-factor = 32 / (N-1) 로 정규화 (N=세션 참가자 수, 큰 세션 스윙 폭주 방지)

---

## 🏆 리더보드 정렬 기준 / Ranking Criteria

| 우선 | 기준 | 효과 |
|:----:|------|------|
| 1 | **Tier Points** ↓ | 누적 승점 + 감점, 실력의 핵심 점수 |
| 2 | **총 승점** ↓ | 상위 순위 자주 → 가산점 |
| 3 | **승리 횟수** ↓ | 승점 > 0 받은 게임 수 |
| 4 | **게임 수** ↓ | 참여도 우대 ("꼴찌해도 참여만 하라") |
| 5 | **총 감점** ↑ (적을수록) | 안정적 플레이 강조 |

---

## 🏷 컬럼 순서 (V8.2) / Column Order

의미 그룹 3덩어리:

```
순위 │ 플레이어 ║ Tier │ 총승점 │ 총감점 ║ 게임수 │ 승리 │ 승률 ║ Reward │ 🏃RUN │ 사용
                ╰─────── Tier 군 ───────╯ ╰── 활동 군 ──╯ ╰─── Reward 군 ───╯
                (보라 wash)              (기본)         (파랑 wash)
```

모바일에선 디테일(총승점/총감점/RUN/사용) 4컬럼 자동 hide → 7컬럼만 노출.

---

## 🛠 인프라 / CI · DevOps

### GitHub Actions
- **`.github/workflows/validate-xlsx.yml`**: `data/board.xlsx` push 시 자동 무결성 검증
  - 필수 컬럼 확인 (`플레이 날짜`, `게임 이름`, `플레이어 이름`, `승점`, `감점`)
  - 10MB 초과 차단

### Dependabot
- `.github/dependabot.yml`: 매주 월요일 09:00 KST 액션 버전 자동 업데이트 PR

### CODEOWNERS
- `.github/CODEOWNERS`: 파일별 오너 지정 (Branch Protection 결합 시 효력)

### 추가 셋업 가이드
👉 [`docs/DEVOPS.md`](docs/DEVOPS.md) — Branch Protection, Environments + Secrets, PR Preview Deploys 등.

---

## ⚠️ 주의 사항 / Cautions

- **Private repo**: collaborator 권한 없으면 페이지 접근 불가. 코드/데이터도 외부 비공개.
- **GitHub Enterprise 구독 필수**: Private Pages 기능은 Enterprise plan에서만 활성. Team plan은 Pages를 Public로만 노출.
- **하드 리프레시 불필요**: V7부터 자동 업데이트 감지 배너가 새 버전을 알림. (그래도 안 보이면 `Ctrl+Shift+R`)
- **데이터는 Excel 마스터**: 모든 통계는 `data/board.xlsx`에서 파싱. 다른 곳에서 직접 수정 X.

---

## 🧠 학습 가치 (Personal Project Goals)

이 프로젝트는 **회사 업무에서 자주 보는 GitHub 패턴**을 1인 환경에서 학습하기 위한 케이스 스터디입니다:

- Organization 운영 (`67JM89`)
- Team → Enterprise plan 업그레이드 (Private Pages 활성)
- Repo 이전 (`sung-jungmin` → `67JM89`) + 자동 redirect
- Private Pages + auth 서브도메인 (`refactored-giggle-*`)
- GitHub Actions / CODEOWNERS / Dependabot
- Branch Protection (계획)
- Environments + Secrets (계획)

---

## 📜 라이선스 / License

자유롭게 사용, 수정, 배포 가능. Free to use, modify, distribute.

## 💬 문의 / Contact

Issues 탭으로 연락 / Reach out via GitHub Issues.

**Repository:** [67JM89/boardgame-leaderboard-kor](https://github.com/67JM89/boardgame-leaderboard-kor) (Private, GitHub Enterprise)

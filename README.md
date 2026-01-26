# 보드게임 리더보드 대시보드 / Boardgame Leaderboard Dashboard

**[🎲 Live Demo](https://sung-jungmin.github.io/boardgame-leaderboard-kor/)**

---

## 개요 / Overview
개인 또는 소규모 모임의 보드게임 결과를 기록하고, **리더보드**, **월별 성적**, **게임별 통계**를 한눈에 확인할 수 있는 대시보드입니다. 엑셀 파일(`board.xlsx`)에 게임 결과를 입력하면, HTML 대시보드에서 자동으로 순위와 통계를 계산합니다.

이 리포지토리는 다음 두 가지를 포함합니다:

- **`index.html`** : 브라우저에서 실행하는 보드게임 리더보드 대시보드
- **`data/board.xlsx`** : 게임 플레이 기록 (플레이 날짜, 게임 이름, 플레이어, 승점, 감점)

A dashboard for recording board game results in personal or small group gatherings, and checking **leaderboards**, **monthly standings**, and **game-specific statistics** at a glance. Simply enter game results in an Excel file (`board.xlsx`), and the HTML dashboard automatically calculates rankings and statistics.

This repository includes:

- **`index.html`** : A browser-based board game leaderboard dashboard
- **`data/board.xlsx`** : Game play records (play date, game name, player, wins, losses)

---

## 사용 방법 / How to Use
#### 1. 대시보드 열기

1. 이 리포지토리를 로컬 PC에 **클론** 또는 **ZIP 다운로드** 합니다.
2. 로컬 폴더에서 `index.html` 파일을 더블클릭하여 브라우저(Chrome, Firefox 등)로 엽니다.
3. 대시보드는 항상 서버의 최신 데이터를 불러오며, 화면 상단의 **파일 업로드(XLSX)** 버튼으로 다른 파일을 업로드하여 즉시 확인할 수도 있습니다.

> **참고:** 이 HTML 파일은 로컬에서만 실행되며, 별도의 웹 서버 없이도 브라우저에서 바로 동작합니다.

#### 2. 엑셀 데이터 업데이트

1. `data/board.xlsx` 파일을 열고 **새로운 게임 결과를 입력**합니다:
   - 플레이 날짜 (예: 2026-01-20)
   - 게임 이름 (예: 콩심기, Take 5!)
   - 플레이어 이름
   - 승점 (예: 3)
   - 감점 (예: -1.0)

2. 파일을 저장한 후, 터미널/명령줄에서 다음 명령을 실행하여 변경사항을 GitHub에 푸시합니다:

```bash
git add data/board.xlsx
git commit -m "Update board results"
git push origin main
```

#### 3. 주요 기능

- **전체 리더보드** (Overall Leaderboard)
  - 현재 포인트, 총 승점, 총 감점, 게임 수, 승리 수, 평균 포인트를 표시합니다.
  - 정렬 순서: 현재 포인트 → 총 승점 → 게임 수 → 총 감점 → 승리 수

- **게임별 리더보드** (Game-Specific Leaderboard)
  - 특정 게임에서의 순위와 통계를 따로 확인할 수 있습니다.

- **월별 리더보드** (Monthly Leaderboard)
  - 연·월 범위를 선택하면, 해당 기간의 게임 결과만을 기반으로 순위를 계산합니다.

- **플레이 히스토리 & 플레이어 상세 정보**
  - 각 플레이어의 날짜별 기록, 포인트 변화, 게임별/월별 누적 통계를 조회할 수 있습니다.

#### 1. Open the Dashboard

1. **Clone** or **download as ZIP** this repository to your local PC.
2. Double-click the `index.html` file in the local folder to open it in a web browser (Chrome, Firefox, etc.).
3. The dashboard always loads the latest data from the server. You can also use the **Upload File (XLSX)** button at the top of the screen to upload a different file and view results immediately.

> **Note:** This HTML file runs locally and works directly in your browser without requiring a separate web server.

#### 2. Update Excel Data

1. Open the `data/board.xlsx` file and **enter new game results**:
   - Play Date (e.g., 2026-01-20)
   - Game Name (e.g., Bean Farming, Take 5!)
   - Player Name
   - Win Score (e.g., 3)
   - Loss Score (e.g., -1.0)

2. Save the file, then run the following commands in your terminal to push changes to GitHub:

```bash
git add data/board.xlsx
git commit -m "Update board results"
git push origin main
```

#### 3. Key Features

- **Overall Leaderboard**
  - Displays current points, total wins, total losses, game count, win count, and average points.
  - Sorted by: Current Points → Total Wins → Game Count → Total Losses → Win Count

- **Game-Specific Leaderboard**
  - View rankings and statistics for a particular game separately.

- **Monthly Leaderboard**
  - Select a year and month range to calculate rankings based only on results from that period.

- **Play History & Player Details**
  - View each player's date-by-date records, point changes, and accumulated statistics by game and month.

---

## 승점 / 감점 규칙 / Scoring Rules
이 리더보드는 각 게임 결과에서 **승점(+)과 감점(-)** 을 합산하여 포인트를 계산합니다.

- 각 기록은 다음 컬럼을 가집니다:
  - **승점** : 해당 판에서 얻은 점수 (보통 상위 순위 보상)
  - **감점** : 해당 판에서 잃은 점수 (페널티, 보통 음수)

- 한 판에서의 포인트 변화는 다음과 같이 계산됩니다:
  ```
  포인트 변화 = 승점 + 감점
  ```

- 플레이어의 **현재 포인트**는 선택된 기간(필터)에 포함된 모든 게임의 포인트 변화를 순서대로 더한 값입니다.
  포인트가 음수로 내려가지 않도록, 최소값은 **0**으로 처리됩니다.

The leaderboard calculates points by summing **win scores (+) and loss scores (-)** from each game result.

- Each record contains:
  - **Win Score** : Points earned in that game (usually rewards for higher placements)
  - **Loss Score** : Points lost in that game (penalty, usually negative)

- A player's point change in one game is calculated as:
  ```
  Point Change = Win Score + Loss Score
  ```

- A player's **Current Points** is the cumulative sum of all point changes from games within the selected period.
  The minimum value is set to **0** to prevent negative totals.

---

## 랭킹 정렬 기준 / Ranking Criteria
모든 리더보드는 다음 순서로 플레이어 순위를 결정합니다:

| 순위 | 기준 | 설명 |
|------|------|------|
| **1순위** | **현재 포인트** (높을수록 우위) | 지금까지의 누적 결과를 보여주는 최종 점수입니다. |
| **2순위** | **총 승점** (높을수록 우위) | 선택된 기간 동안 획득한 승점의 합입니다. 상위 순위를 자주 달성한 플레이어가 유리합니다. |
| **3순위** | **게임 수** (높을수록 우위) | 해당 기간 동안 참여한 판 수입니다. 많이 참여한 사람을 우대하여 "꼴찌를 해도 괜찮으니 많이 참여하자"는 메시지를 반영합니다. |
| **4순위** | **총 감점** (낮을수록 우위) | 선택된 기간 동안의 감점 절댓값 합입니다. 손실을 적게 낸, 안정적인 플레이를 강조합니다. |
| **5순위** | **승리** (높을수록 우위) | 1등을 한 횟수입니다. 동일 조건에서 1등을 더 자주 한 플레이어가 상위에 옵니다. |

**정렬 기준의 철학:**

- **총 승점** (2순위)은 게임 실력과 성과를 강조합니다.
- **게임 수** (3순위)를 상위에 배치한 것은 **참여도**를 실력만큼 중요하게 본다는 의미입니다.
- 이를 통해 "꼴찌를 해도 괜찮으니, 게임에 자주 참여해 주세요!"라는 커뮤니티의 메시지를 전달합니다.

All leaderboards rank players in the following order:

| Rank | Criteria | Description |
|------|----------|-------------|
| **1st** | **Current Points** (higher is better) | The final accumulated score reflecting overall performance. |
| **2nd** | **Total Win Score** (higher is better) | Sum of all win scores earned during the selected period. Players with frequent top finishes have an advantage. |
| **3rd** | **Game Count** (higher is better) | Number of games played during the selected period. This prioritizes participation, sending the message "It's okay to finish last—just participate often!" |
| **4th** | **Total Loss Score** (lower is better) | Absolute value sum of loss scores during the selected period. Emphasizes stable, consistent play with minimal losses. |
| **5th** | **Wins** (higher is better) | Number of 1st place finishes. In case of a tie, the player with more 1st place finishes ranks higher. |

**Philosophy Behind the Ranking:**

- **Total Win Score** (2nd) emphasizes gameplay skill and performance.
- Placing **Game Count** (3rd) high reflects that **participation** is valued as much as skill.
- This conveys the community's message: "It's okay if you finish last—please join us often!"

---

## 주의 사항 / Cautions

- 이 리포지토리는 **공개(Public)** 로 설정되어 있으므로, `data/board.xlsx`의 플레이어 이름과 점수는 누구나 열람 가능합니다. 이를 감수하고 사용하시기 바랍니다.
- 리더보드 로직(정렬 기준, 포인트 계산 방식)을 수정하려면, `index.html`의 JavaScript 코드도 함께 업데이트해야 합니다.

- This repository is set to **Public**, so player names and scores in `data/board.xlsx` are viewable by anyone. Please use this tool with that in mind.
- If you modify the leaderboard logic (sorting criteria, point calculation), you must also update the JavaScript code in `index.html` accordingly.

---

## 파일 구조 / File Structure

```
boardgame-leaderboard-kor/
├── index.html          # 리더보드 대시보드 (로컬에서 실행)
│                       # Leaderboard dashboard (runs locally)
├── data/
│   └── board.xlsx      # 게임 기록 데이터 (정기적으로 업데이트)
│                       # Game records data (updated periodically)
└── README.md           # 이 문서 / This document
```

---

## 라이선스 / License
자유롭게 사용, 수정, 배포 가능합니다.
Free to use, modify, and distribute.

---

## 문의 및 피드백 / Contact & Feedback

문제를 발견하거나 기능을 제안하고 싶다면, GitHub Issues를 통해 연락주세요.  
If you find any issues or have feature suggestions, please reach out via GitHub Issues.

**Repository:** [sung-jungmin/boardgame-leaderboard-kor](https://github.com/sung-jungmin/boardgame-leaderboard-kor)

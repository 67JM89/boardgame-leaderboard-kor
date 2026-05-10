# DevOps Setup Guide

회사 업무에서 자주 보는 GitHub 패턴들을 이 1인 프로젝트에서 익히기 위한 가이드.

---

## 1. GitHub Actions — `.github/workflows/`

### `validate-xlsx.yml` (이미 활성화됨)
- `data/board.xlsx` push/PR 시 자동 실행
- 필수 컬럼(`플레이 날짜`, `게임 이름`, `플레이어 이름`, `승점`, `감점`) 검증
- 파일 크기 10MB 초과 차단
- 실패 시 PR 머지 / push 후 빨간 X 표시

### 동작 확인
```bash
# 의도적으로 잘못된 xlsx 만들어 push → Actions 탭에서 실패 로그 확인
```

또는 GitHub 웹: `Actions` 탭 → `Validate xlsx data` → 최근 실행 결과 확인.

### 워크플로우 추가 권장 (다음 단계)
- `html-lint.yml` — HTMLHint 등으로 index.html 검증
- `link-check.yml` — 외부 CDN 링크 살아있는지 주기 체크
- `lighthouse.yml` — Pages 성능 점수 추적

---

## 2. CODEOWNERS — `.github/CODEOWNERS`

이미 추가됨. 모든 파일 오너를 본인으로 지정.

### Branch Protection 과 결합 시 강력해짐
GitHub 웹 → `Settings` → `Branches` → `Add rule`:
- **Branch name pattern**: `main`
- **Require a pull request before merging** ← 켜면 직접 push 차단 (1인은 굳이 X)
- **Require review from Code Owners** ← CODEOWNERS 매칭 파일 변경 시 본인 승인 필요
- **Require status checks to pass** ← `validate-xlsx` 통과해야 머지 가능 ⭐ 추천
- **Do not allow bypassing the above settings** ← 본인도 룰을 따라야 함

⚠️ 1인 dev로 운영 시: `Require status checks` 만 켜면 push는 자유롭되 CI 실패 데이터는 안 들어가도록 강제할 수 있음.

---

## 3. Dependabot — `.github/dependabot.yml`

이미 추가됨. 매주 월요일 오전 9시(KST) 액션 업데이트 PR 자동 생성.

### 동작 확인
- `Insights` 탭 → `Dependency graph` → `Dependabot`
- 첫 스캔까지 ~30분 걸릴 수 있음
- PR이 자동 생성되면 머지 (CI 통과 후)

### npm 사용 시 (참고)
```yaml
- package-ecosystem: npm
  directory: "/"
  schedule:
    interval: weekly
```

---

## 4. Environments + Secrets (수동 셋업 필요)

### 왜 필요?
- 같은 코드를 dev/stage/prod 등 환경별로 다른 설정으로 배포
- 환경별 Secret 분리 (DB 비밀번호, API 키 등)
- 환경별 deployment 승인 워크플로우

### 설정 절차
1. `Settings` → `Environments` → `New environment`: `production`
2. **Required reviewers**: 본인 추가 (production 배포 전 본인 승인 필요)
3. **Environment secrets**: 예) `XLSX_PASSWORD_HASH` 추가
4. 워크플로우에서 사용:
   ```yaml
   jobs:
     deploy:
       environment: production
       runs-on: ubuntu-latest
       steps:
         - run: echo "Hash: ${{ secrets.XLSX_PASSWORD_HASH }}"
   ```

### 이 프로젝트 적용 사례
현재 `index.html`에 비밀번호 SHA-256 해시가 하드코딩됨.
Environment Secret으로 이동 → 빌드 타임에 주입하는 워크플로우로 개선 가능 (학습용).

---

## 5. PR Preview Deploys (고급)

### 개념
- 각 PR 마다 별도 Pages URL 생성 → 머지 전 미리보기 가능
- 회사 환경에선 거의 표준

### 구현 옵션
- **Cloudflare Pages** + Wrangler — GitHub Pages 보다 PR preview 지원이 깔끔
- **Netlify Deploy Previews** — 자동, 무료 티어로 충분
- **GitHub Pages + Actions** — `peaceiris/actions-gh-pages` 등으로 PR 별 폴더 deploy

### 한계 (현 셋업)
- GitHub Pages 자체는 PR preview 미지원 (단일 환경만 배포)
- Private Pages 라 외부 호스팅으로 옮길 경우 인증 처리 별도 필요

→ 회사 업무 학습 목적이라면 **Netlify** 가 가장 깔끔하게 PR preview 익히기 좋음.

---

## 6. 추가 학습 권장 (이후 단계)

| 항목 | 학습 가치 | 도입 난이도 |
|------|----------|------------|
| Branch Protection rule | ⭐⭐⭐⭐⭐ | ⭐ |
| Required status checks (validate-xlsx) | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Environment Secrets | ⭐⭐⭐⭐ | ⭐⭐ |
| Renovate (Dependabot 보강) | ⭐⭐⭐ | ⭐⭐ |
| Lighthouse CI | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| Conventional Commits + commitlint | ⭐⭐⭐⭐ | ⭐⭐ |
| Semantic Release | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Renovate group rules | ⭐⭐⭐ | ⭐⭐⭐ |

---

## 체크리스트 (회사 업무에 적용 시 따라가는 순서)

- [x] `.github/workflows/<name>.yml` 으로 CI 추가
- [x] `.github/CODEOWNERS` 로 오너 정의
- [x] `.github/dependabot.yml` 로 의존성 자동화
- [ ] **Branch Protection**: status check 필수화 (수동, GitHub UI)
- [ ] **Environments**: production 환경 + secret 분리 (수동)
- [ ] **Required Reviewers**: production 배포 시 사람 승인 (수동)
- [ ] **Audit log** 모니터링 — Org Settings → Logs
- [ ] **Insights → Dependency graph** 정기 확인

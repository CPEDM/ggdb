# 견과다방 매장 안내판 (Google Sheets 제어 + GitHub Pages 전체화면 모니터링)

- **제어(수정):** Google Sheets `제어판` 탭 (스마트폰/PC에서 체크박스·드롭다운·영업시간 수정)
- **화면(모니터링):** GitHub Pages `index.html` (Google 배너 없는 100% 전체화면 디스플레이)
- **유지비:** 0원 (토큰 만료 없음, 약 5~15초 내 실시간 반영)

---

## 1. 파일 구성 (GitHub Pages 표준)
- [`index.html`](index.html): 매장 태블릿 디스플레이 화면 (ES5 호환, 전체화면 + PWA 지원, XHR/JSONP 이중 폴백)
- [`manifest.json`](manifest.json): 태블릿 "홈 화면에 추가" 전체화면 실행용 PWA 매니페스트
- [`.nojekyll`](.nojekyll): GitHub Pages 불필요 빌드 생략용 표준 마커 파일
- [`Code.gs`](Code.gs): Google Apps Script 백엔드 (`제어판` 시트 생성 및 `?api=status` JSON/JSONP API)
- [`specs/00_ARCHITECTURE_MAP.md`](specs/00_ARCHITECTURE_MAP.md): 아키텍처 및 동기화 명세

---

## 2. 설치 및 배포 순서

### Step 1. Google Sheets API 준비
1. [Google Sheets](https://sheets.google.com)에서 새 스프레드시트를 만듭니다 (예: `견과다방 안내판 제어`).
2. 상단 **[확장 프로그램] ➔ [Apps Script]** 를 열고 `Code.gs`에 이 폴더의 [`Code.gs`](Code.gs) 전체를 붙여넣은 뒤 저장합니다. *(HTML 파일은 추가할 필요 없습니다.)*
3. 구글 시트로 돌아와 **새로고침(F5)** 한 뒤 상단 **`견과다방 안내판` ➔ `1. 제어판 시트 초기 생성/복구`** 실행
4. **`견과다방 안내판` ➔ `3. 자동 반영 트리거 설치 (권장)`** 실행
5. Apps Script 편집기 우측 상단 **[배포] ➔ [새 배포] ➔ [웹 앱]**:
   - **다음 사용자 권한으로 실행:** `나 (본인 이메일)`
   - **액세스 권한이 있는 사용자:** `모든 사용자 (Anyone)`
6. 발급된 **웹 앱 URL**(`https://script.google.com/macros/s/.../exec`)을 복사해 둡니다.

### Step 2. GitHub Pages 배포
1. GitHub에 새 Public 저장소를 만들고 [`index.html`](index.html), [`manifest.json`](manifest.json), [`.nojekyll`](.nojekyll) 파일을 업로드합니다.
   - *(선택)* 업로드 전 `index.html` 안의 `DEFAULT_GAS_API_URL = '';` 에 1단계에서 복사한 웹 앱 URL을 미리 넣어두면 태블릿에서 별도 연결 입력 없이 바로 실행됩니다.
2. 저장소 **[Settings] ➔ [Pages]** 에서 `Deploy from a branch` (`main` / `/ (root)`)를 선택해 배포합니다.

### Step 3. 태블릿 연결 및 전체화면 실행
- **연결 방법 (2가지 중 편한 방법):**
  - URL에 직접 붙여 열기: `https://아이디.github.io/저장소명/?api=발급된웹앱URL` (최초 1회 접속 시 태블릿 브라우저에 자동 저장됨)
  - 또는 그냥 접속 후 상단 **`[시트 연결]`** 버튼(또는 브랜드명 2번 터치)을 눌러 웹 앱 URL 입력
- **전체화면 실행 방법:**
  - **Android / PC:** 우측 상단 **`[전체화면]`** 버튼 클릭 또는 화면 중앙 더블탭
  - **iPad (iPadOS Safari):** Safari 하단/상단 **공유 버튼 ➔ [홈 화면에 추가]** 로 아이콘을 만든 뒤 실행하면 주소창이 없는 **100% 전체화면**으로 고정됩니다.
- **배터리 보호:** 스마트 플러그로 태블릿 전원을 완전히 끄지 말고, 코드 내 `야간 절전(블랙아웃)` + 충전 어댑터 간헐 타이머(2시간 충전 / 3시간 차단)로 배터리를 보호하세요.

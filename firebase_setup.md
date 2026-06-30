# 🖥️ Firebase Realtime Database 연동 설정 가이드

본 웹 게임 시뮬레이터는 **Firebase Realtime Database (RTDB)**를 지원하여 30인 실시간 멀티플레이 위치 동기화, 대화 공유, 연산 및 타이머 제어를 지원합니다. 

아래 순서에 따라 Firebase 콘솔에서 프로젝트를 개설하고 필요한 설정 값들을 획득하여 게임 내 관리자 화면에 입력하십시오.

---

## 1. Firebase 프로젝트 생성하기

1. **[Firebase 콘솔](https://console.firebase.google.com/)**에 접속하여 구글 계정으로 로그인합니다.
2. **`[프로젝트 추가]`** 버튼을 클릭합니다.
3. 프로젝트 이름(예: `zombie-math-game`)을 입력하고 **`[계속]`**을 클릭합니다.
4. 구글 애널리틱스(Google Analytics) 설정 여부를 선택한 후(테스트용이라면 비활성화 권장), **`[프로젝트 만들기]`**를 클릭합니다.
5. 프로젝트 생성이 완료되면 **`[계속]`**을 눌러 대시보드로 진입합니다.

---

## 2. Realtime Database 활성화 및 보안 규칙 설정

1. 좌측 사이드바 메뉴에서 **`[빌드(Build)]`** -> **`[Realtime Database]`**를 선택합니다.
2. **`[데이터베이스 만들기]`** 버튼을 클릭합니다.
3. **실시간 데이터베이스 위치(Region)**를 선택합니다 (기본값 또는 `싱가포르` 권장) 후 **`[다음]`**을 클릭합니다.
4. 보안 규칙 시작 모드에서 **`[테스트 모드에서 시작]`**을 선택한 후 **`[사용 설정]`**을 클릭합니다.
5. 데이터베이스 인스턴스가 생성되면 상단의 **`[규칙(Rules)]`** 탭으로 이동합니다.
6. 실시간 동기화 시 권한 오류가 나지 않도록 보안 규칙을 다음과 같이 수정하고 **`[게시]`**를 클릭합니다:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> [!WARNING]
> `.read: true, .write: true` 규칙은 개발 및 테스트를 위한 규칙입니다. 서비스 배포 시에는 적절한 인증 보안 룰을 씌우는 것이 좋습니다.

---

## 3. 웹 앱 등록 및 Firebase 인증 정보(Credentials) 확인

1. Firebase 프로젝트 홈(대시보드 메인 화면)으로 이동합니다.
2. 프로젝트 이름 아래에 있는 **`[</> (Web)]`** 아이콘을 클릭하여 웹 앱 등록을 시작합니다.
3. 앱 닉네임(예: `web-client`)을 입력한 뒤 **`[앱 등록]`**을 클릭합니다.
4. 등록 완료 후 화면에 표시되는 `firebaseConfig` 객체에서 다음 세 가지 핵심 값을 복사해 둡니다:
   - **`apiKey`** (API 키)
   - **`databaseURL`** (데이터베이스 URL - Realtime Database 메인 화면 상단에서도 복사 가능)
   - **`projectId`** (프로젝트 ID)

---

## 4. 게임 내 설정 패널에 입력하여 연동하기

1. [index.html](file:///c:/Users/USER/Documents/안티그래비티/2026duru-zombie/index.html)을 웹 브라우저에서 실행합니다.
2. 우측 상단의 **`[🔑 관리자 로그인]`**을 누르고 패스워드 `2525`를 입력합니다.
3. 우측 관리자 콘솔 맨 아래에 있는 **`⚙️ 파이어베이스 실시간 멀티플레이 연동`** 접기(details) 패널을 클릭합니다.
4. 복사해 둔 정보를 각각 입력합니다:
   - **API Key** 입력란에 복사한 `apiKey` 붙여넣기
   - **Database URL** 입력란에 복사한 `databaseURL` 붙여넣기
   - **Project ID** 입력란에 복사한 `projectId` 붙여넣기
5. **`[Firebase 연결하기]`** 버튼을 클릭합니다.
6. 연결이 성공하면 네트워크 상태가 **`네트워크 상태: Firebase 실시간 연결됨 (Green)`**으로 전환되며, 브라우저 창을 여러 개 띄웠을 때 캐릭터들의 위치와 대화가 실시간으로 완벽하게 동기화되기 시작합니다!

---

## 5. (선택 사항) Firebase 정보 소스코드에 하드코딩하기

매번 관리자 패널에서 입력하는 번거로움을 줄이려면, [index.html](file:///c:/Users/USER/Documents/안티그래비티/2026duru-zombie/index.html) 파일의 소스 코드 내부 인풋 밸류에 기본값으로 삽입해 둘 수 있습니다.

* **수정할 파일**: `index.html`
* **수정할 위치**: `index.html` 소스코드 내 Firebase 인풋 필드 마크업 부분
```html
<!-- index.html 약 1251라인 -->
<input type="text" id="fb-apikey" class="firebase-input" value="YOUR_API_KEY_HERE" placeholder="API Key">
<input type="text" id="fb-dburl" class="firebase-input" value="https://YOUR_PROJECT_ID.firebaseio.com" placeholder="Database URL">
<input type="text" id="fb-projectid" class="firebase-input" value="YOUR_PROJECT_ID" placeholder="Project ID">
```

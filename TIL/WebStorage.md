# WebStorage // 2023년 6월 20일 (Tue)

### 특징 -> HTML5 부터 지원하는 브라우저에 데이터를 저장할 수 있는 API

- 5MB의 정보 저장 가능
- 자동으로 서버에 전송되지 않음 (쿠키 트래픽 문제 해결)
- 오리진 단위로 접근이 제한 (CSRF)로 부터 안전

### 웹스토리지의 종류와 활용 예시

#### 로컬 스토리지

- 로컬 스토리지의 저장 범위는 도메인 별이다.
- 직접 삭제를 해야만 삭제가 된다.
- 글 임시 저장 또는 사용자 설정 저장을 하는데 있어 활용을 할 수가 있다.
- 단점으로는, 만료 기간 설정이 불가하다. 미지원 하는 브라우저나 사파릿 시크릿 모드에는 할당량이 0으로 처리 되어 있어 에러 처리가 필요하다.

#### 세션 스토리지

- 세션 스토리지의 저장 범위는 도메인/탭 별로 할 수 있다.
- 브라우저나 탭을 닫을 시에 삭제가 되어 진다.
- 입력 폼을 저장하거나 일회서 로그인을 하는데 있어 활용을 할 수가 있다.

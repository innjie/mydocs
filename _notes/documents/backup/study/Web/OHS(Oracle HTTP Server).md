
## 기능
- 아파치 기반의 웹서버
- 동적 / 정적 콘텐츠 지원
- URL 재사용 및 프록시 서버 기능
- 별도 관리 프로세스
- GUI 기반 모니터링
- 통합 미들웨어

## 아키텍처
### 1. OHS Parent Process
- Child 프로세스들을 시작시키고 모니터링
- 구성 파일 읽음
- Client 요청 처리 X

### 2. OHS Chile Process
- Client 요청 처리
- Unix는 여러개, Windows는 1개

### 3. Pluggable Modules
- 다양한 모듈을 제공하고 있으며 확장 가능
- 다양한 플러그인 지원

### Oracle Process Manager / Notification Server
- 프로세스 상태 감지
- 프로세스 시작 / 중지 / 자동 재시작 담당

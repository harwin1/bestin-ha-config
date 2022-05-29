# bestin-ha-config
Home Assistant Config files for Bestin IOT

## 사전 요구사항
- IPARK 스마트홈 사용자

- curl 명령어가 수행가능한 환경 (home assistant가 구동중인 서버)

- 본인 거주 아파트의 원격 제어 Host IP 주소 : http://www.i-parklife.com/ 접속 후 아파트 이름을 클릭하면 원격제어 페이지로 접속된다. 거기서 나오는 IP 주소 획득

- 원격제어 페이지에서 알아내야 하는 정보
  1. 계정 정보 : 회원 가입하면 생성되는 id와 password 필요
  2. 쿠키 정보 : (크롬 브라우저 기준) F12키 누르고 개발자도구 진입 -> 응용프로그램 클릭 -> 쿠키 -> user_name의 값, PHPSESSID의 값
 
## 설정 가이드
1. 'environment_variables.json'의 파일 내용 수정 : Host IP 주소, 계정 id, 계정 password 및 쿠키 정보에서 알아낸 user_name의 값 입력
2. `automations.yaml` 참고하여 주기적으로 cookie_phpsessid command를 호출하여 인증토큰을 갱신하도록 자동화 구성
3. 본인 거주 아파트 환경에 맞게 방(이름) 갯수, 조명(이름) 갯수, 전원(이름) 소켓 갯수, 난방(이름) 갯수에 따라 sensors.yaml, lights.yaml, switches.yaml 파일 내용 수정 필요
   
   &거실(조명1, 조명2, 조명3), 부엌(조명2개, 방1번), 안방(조명2개, 방2번)은 기본값으로 설정 되어있음 혹시 다를 경우 수정필요

## 지원 기능
1. 조명
2. 난방 
3. 전원 소켓(대기전력 차단 지원)
4. 전열교환기
5. 가스벨브(단방향)
6. 외출설정
7. 도어락(상태조회 지원)

## 작성시 참조한 github
https://github.com/py-kim/bestin_config_HA
설정가이드가 상세히 나와있어서 참고하길 추천

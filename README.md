# bestin_config_HA

아이파크 스마트홈1 유저를 위한 Home Assistant용 Configuration File

## 사전 요구사항

- 아이파크 스마트홈1 사용자

  - 지원 아파트 목록
    - 서울 : '신공덕', '테헤란', '인왕산2차', '송파위례1차', '위례2차아이파크', '아현아이파크', '한남아이파크', '고덕숲아이파크', '삼성동센트럴아이파크'
    - 경기 : '인천검단2차', '수원1B/L', '수원3B/L', '수원4B/L', '수원2B/L', '남양주 별내', '용인성복', '고양삼송', '안산신길', '부천아이파크1단지', '부천아이파크2단지', 'C지역관사', 'A지역관사', '남양주별내2차', '수원5B/L', '수원6B/L', '고양삼송2차', 'F지역관사', '용인서천2차', '수원7B/L', '수원5차8블럭', '수원5차9블럭', '광주태전아이파크', '포천아이파크', '부천3차아이파크', '다산신도시아이파크', '포천2차아이파크', '김포한강아이파크', '평택비전아이파크', '일산센트럴아이파크'
    - 경상도 : '해운대', '부산명륜1단지', '부산명륜2단지', '부산명륜임대동', '김해1단지', '김해2단지', '울산문수로1단지', '울산문수로2단지', '대구월배1차', '울산약사', '부산명륜2차1단지',  '부산명륜2차2단지', '부산명륜2차3단지', '경북도청신도시', '대구월배2차아이파크', '대구수성아이파크', '거제아이파크', '울산복산아이파크', '창원용지아이파크', '거제2차아이파크1단지', '거제2차아이파크2단지'
    - 전라도 : '군산미장', '광주무등산아이파크', '군산미장2차아이파크'
    - 충청도 : '대전도안', '아산용화', '천안백석2차', '천안백석3차아이파크'
    - 강원도 : '춘천장학', '속초아이파크'
    - 제주도 : '제주아라', '제주노형'

- curl 명령어가 수행가능한 환경 (home assistant가 구동중인 서버)

- 본인 거주 아파트의 원격 제어 Host IP 주소 : http://www.i-parklife.com/ 접속 후 아파트 이름을 클릭하면 원격제어 페이지로 접속된다. 거기서 나오는 IP 주소 획득

- 원격제어 페이지에서 알아내야 하는 정보
  1. 계정 정보 : 회원 가입하면 생성되는 id와 password 필요
  2. 쿠키 정보 : (엣지 브라우저 기준) F12키 누르고 개발자도구 진입 -> 응용프로그램 클릭 -> 쿠키 -> user_name의 값, PHPSESSID의 값

## 설정 가이드

- 'environment_variables.json'의 파일 내용 수정 : Host IP 주소, 계정 id, 계정 password 및 쿠키 정보에서 알아낸 user_name의 값 입력

- 'cookie_phpsessid' 파일 내용 수정 : 쿠키 정보에서 알아낸 PHPSESSID의 값 입력, 초기에 한번만 입력하면 되며 automations.yaml을 통해 만료되기 전 한달에 한번 자동 업데이트 됨.

- 본인 거주 아파트 환경에 맞게 방 갯수, 조명 갯수, 전원 소켓 갯수에 따라 sensors.yaml, lights.yaml, switches.yaml 파일 내용 수정 필요, 직접 API 명령을 날려보며 작업하기를 권장함

  - Rest API 명령은 POSTMAN 프로그램을 사용하면 쉬움
  
  - LOGIN 예시 : (GET) http://<Host IP 주소>/webapp/data/getLoginWebApp.php?devce=WA&login_ide=<계정 id>&login_pwd=<계정 password>
  
    - LOGIN 하면 Response의 Cookies에 user_id, PHPSESSID, user_name이 생성된다. 이 3가지 쿠키를 이용하여 하단의 조명, 거실 조명, 전원 소켓, 난방 상태를 조회할 수 있다.
    
  - 조명 상태 예시 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice.php?req_name=remote_access_light&req_action=status&req_dev_num=1
  
    - req_dev_num : 방번호를 의미하며 위 예시에서는 1이 입력되었음
    
  - 조명 조작 예시 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice.php?req_name=remote_access_light&req_action=control&req_unit_num=1&req_ctrl_action=off&req_dev_num=1
  
    - req_unit_num : 조명번호를 의미하며 위 예시에서는 1이 입력되었음
    - req_ctrl_action : 조명제어명령을 의미, on 또는 off로 입력
    - req_dev_num : 방번호를 의미하며 위 예시에서는 1이 입력되었음
    
  - 거실 조명 상태 예시 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice.php?req_name=remote_access_livinglight&req_action=status
  
  - 거실 조명 조작 예시 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice.php?req_name=remote_access_livinglight&req_action=control&req_unit_num=1&req_ctrl_action=off
  
    - req_unit_num : 조명번호를 의미하며 위 예시에서는 1이 입력되었음
    - req_ctrl_action : 조명제어명령을 의미, on 또는 off로 입력
    
  - 전원 소켓 상태 예시 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice.php?req_name=remote_access_electric&req_action=status&req_dev_num=1
  
    - req_dev_num : 방번호를 의미하며 위 예시에서는 1이 입력되었음
    
  - 전원 소켓 조작 예시 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice.php?req_name=remote_access_electric&req_action=control&req_unit_num=1&req_ctrl_action=on&req_dev_num=1
  
    - req_unit_num : 전원 소켓 번호를 의미하며 위 예시에서는 1이 입력되었음
    - req_ctrl_action : 조명제어명령을 의미, on 또는 off로 입력
    - req_dev_num : 방번호를 의미하며 위 예시에서는 1이 입력되었음
    
  - 난방 상태 및 온도조회 : (GET) http://<Host IP 주소>/webapp/data/getHomeDevice_heat.php?req_name=remote_access_temper&req_action=status&req_unit_num=room1
  
    - req_unit_num : 방번호를 의미하며 위 예시에서는 room1이 입력되었음, 다른 방 조회하려면 room2, room3과 같이 입력
 

## 지원 기능
1. 조명
2. 난방 (온도, 가동상태)
3. 전원 소켓

## 작성시 참조한 github
https://github.com/gaussian8/bestin-ha-config

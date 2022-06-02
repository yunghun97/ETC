# 🍞 도커 컴포즈

> 시스템 구축과 관련된 명령어를 하나의 텍스트 파일(정의 파일)에 기재해 명령어 한번에 시스템 전체를 실행하고 종료와 폐기까지 한번에 하도록 도와주는 도구

## 도커 컴포즈 구조
시스템 구축에 필요한 설정을(YAML) 포맷으로 기재한 정의 파일을 이용해 전체 시스템을 일괄 실행(run) 또는 일괄 종료 및 삭제 할 수 있는 도구다.

### docker-compose up
정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행한다. 정의 파일에는 네트워크나 불륨에 대한 정의도 기재할 수 있어서 주변 환경을 한꺼번에 생성할 수 있다.  

### docker-compose down
컨테이너와 네트워크를 정지 및 상제한다. 불륨과 이미지는 삭제하지 않는다. 컨테이너와 네트워크 삭제 없이 종료만 하고 싶다면 stop 커맨드를 사용한다.

### 도커 컴포즈 vs Dockerfile
- 도커 컴포즈
    - docker run 명령어를 여러 개 모아 놓은 것
    - 컨테이너와 주변 환경을 생성한다.
    - 네트워크와 불륨까지 함께 만들 수 있다.
- Dockerfile
    - 이미지를 만들기 위한 것
    - 네트워크나 불륨은 만들 수 없다.

#### 도커 컴포즈 vs 쿠버네티스
- 도커 컴포즈
    - 컨테이너 생성 및 삭제 기능
    - 컨테이너 관리 기능 X
- 쿠버네티스 
    - 컨테이너 관리 도구
    - 컨테이너 관리 기능 O
    
## ❗ docker-compose 파일 작성하는 방법
### 1. 파일 이름(YAML 형식)
> docer-compose.yml  
### 2. 구조
컴포즈 파일은 맨 아퓨에 컴포즈 버전을 적고, 그 뒤로 services, networks, volumes를 차례로 기재한다.  
각 항목은 뒤에 콜론(:)을 붙인다.
- service
    - 컨테이너의 대한 내용(도커 컴포즈와 쿠버네티스에서는 컨테이너의 집합체를 서비스라고 부른다.)
> YAML 형식에서는 공백에 따라 의미가 달라지므로 탭은 의미가 없다
### 작성 예시
```bash
# 기본 형식
# '공백 두 개'로 맨 처음 들여쓱기를 했다면 그 뒤로도 '공백 두 개'가 한 단이 되도록 해야한다.
version: "3" # 공백 0
services:
  컨테이너이름: # 공백 2
networks:
  네트워크이름: # 공백 2
volumes:
  불륨이름: # 공백 2
```
```bash
# 설정을 기재
version: "3"
service:
  컨테이너이름:
    image: 이미지이름
    networks:
      - 네트워크이름
    ports:
      - 포트설정
  컨테이너이름2
    ...
networks:
  네트워크이름1:
volumes:
불륨_이름1:
```
```bash
# 예시
version: "3.5"
services:
  zk1:
    image: confluentinc/cp-zookeeper:5.5.1
    restart: always
    hostname: zk1
    container_name: zk1
    ports:
      - "2181:2181"
    environment:
      - ZOOKEEPER_SERVER_ID=1
      - ZOOKEEPER_CLIENT_PORT=2181
      - ZOOKEEPER_TICK_TIME=2000
      - ZOOKEEPER_INIT_LIMIT=5
      - ZOOKEEPER_SYNC_LIMIT=2
      - ZOOKEEPER_SERVERS=zk1:2888:3888;zk2:2888:3888;zk3:2888:3888
```
## 😀 작성 요령 정리
- 첫 줄에 docker-compose 버전 기재
- 주 항목 service, networks, volumes 아래에 설정 내용을 기재
- 항목 간의 상하 관계는 공백을 사용한 들여쓰기로 나타낸다.
- 들여쓰기는 같은 수의 배수만큼 공백을 사용한다.
- 이름은 주 항목 아래에 들여쓰기 한다음 기재한다.
- 컨테이너 설정 내용은 이름 아래에 들여쓰기 한 다음 기재한다.
- 여러 항목을 기재하려면 줄 앞에 '-'을 붙인다.
- 이름 뒤에는 콜론(:)을 붙인다.
- 클론 뒤에는 반드시 공백이 와야 한다.(바로 줄바꿈 하는 경우는 예외)
- #뒤의 내용은 주석으로 간주된다.
- 문자열은 작은따옴표(') 또는 큰따옴표(")로 감싸 작성한다.
  
## 🔧 docker-compose 파일의 항목
> 주 항목의 이름은 복수형이라는 데 주의

### 주 항목
|항목|내용|
|:---|:---|
|services|컨테이너를 정의한다.|
|networks|네트워크를 정의한다.|
|volumes|불륨을 정의한다.|
  
### 기타 정의 내용
> image~restart는 주요 정의 항목으로 변화가 없지만 restart 아래 항목들은 버전에 따라 명령어도 변화할 수 있으므로 공식 참조문서 참조  

|docker-compose 항목|docker run 커맨드의 해당 옵션 또는 인자|내용|
|:---|:---|:---|
|image|이미지 인자|사용할 이미지를 지정|
|networks|--net|접속할 네티워크를 지정|
|volumes|-v, --mount|스토리지 마운트를 설정|
|ports|-p|포트설정|
|environment|-e|환경변수 설정|
|depends_on|없음|다른 서비스에 대한 의존관계를 정의|
|restart|없음|컨테이너 종료 시 재시작 여부를 설정|
|command|커맨드 인자|컨테이너 시작 시 기존 커맨드를 오버라이드|
|container_name|--name|실행할 컨테이너의 이름을 명시적으로 지정|
|dns|--dns|DNS 서버를 명시적으로 지정|
|env_file|없음|환경설정 정보를 기재한 파일을 로드|
|entrypoint|--entrypoint|컨테이너 시작 시 ENTRYPOINT 설정을 오버라이드|
|external_links|--link|외부 링크를 설정|
|extra_hosts|--add-host|외부 호스트의 IP 주소를 명시적으로 지정|
|logging|--log-driver|로그 출력 대상을 설정|
|network_mode|--network|네트워크 모드를 설정|
- depends_on은 **다른 서비스에 대한 의존 관계**를 나타낸다. 예를 들어 abc 컨테이너 정의에 depends_on: '-xyz' 라는 내용이 있으면 xyz컨테이너를 생성한 다음에 abc 컨테이너를 만든다.

### restart의 설정 값
|설정값|내용|
|:---|:---|
|no|재시작하지 않는다.|
|always|항상 재시작한다.|
|on-failure|프로세스가 0 외의 상태로 종료됐다면 재시작한다.|
|unless-stopped|종료 시 재시작하지 않음, 그 외에는 재시작한다.|

## 🎃 docker-compose up 커맨드
```bash
docker-compose -f 정의_파일_경로 up 옵션
# 현재 위치에 docker-compose 파일이 있으면 docker-compose up 가능
# stop, down 도 마찬가지
```
### docker-compose 옵션 항목
|항목|내용|
|:---|:---|
|-d|백그라운드로 실행|
|--no-color|화면 출력 내용을 흑백으로|
|--no-deps|링크된 서비스를 실행하지 않음|
|--force-recreate|설정 또는 이미지가 변경되지 않더라도 컨테이너를 재생성|
|--no-cretae|컨테이너가 이미 존재할 경우 다시 생성하지 않음|
|--no-build|이미지가 없어도 이미지를 빌드하지 않음|
|--build|컨테이너를 실행하기 전에 이미지를 빌드|
|--abort-on-container-exit|컨테이너가 하나라도 종료되면 모든 컨테이너를 종료|
|-t, --timeout|컨테이너를 종료할 때의 타임아웃 설정, (기본 10초)|
|--remove-orphans|컴포즈 파일에 정의되지 않은 서비스 컨테이너를 삭제|
|--scale|컨테이너의 수를 변경|
|--rmi 종류|삭제 시에 이미지도 삭제한다. 종류를 all로 지정하면 사용헀던 모든 이미지를 삭제한다. local로 지정하면 커스텀 태그가 없은 이미지만 삭제|
|-v, --volumes|volumes 항목에 기재된 불륨을 삭제한다. 단 external로 지정된 불륨은 삭제되지 않는다.|
  
### 주요 커맨드 목록 (docker-compose & docker)
|항목|내용|
|:---|:---|
|**up**|컨테이너를 생성하고 실행한다.|  
|**down**|컨테이너와 네트워크를 종료하고 삭제한다.|  
|ps|컨테이너 목록 출력|  
|config|컴포즈 파일을 확인하고 내용을 출력한다.|  
|port|포트 설정 내용을 출력|  
|logs|컨테이너가 출력한 내용을 화면에 출력 -f로 지속적인 로그 확인 가능|  
|start|컨테이너를 시작한다.|  
|**stop**|컨테이너를 종료한다.|  
|kill|컨테이너 강제 종료|  
|exec|명령어를 실행한다.|
|run|컨테이너를 실핸한다.|
|create|컨테이너를 생성한다.|
|restart|컨테이너를 재실행한다.|
|pause|컨테이너를 일시 정지한다.|
|unpause|컨테이너의 일시 정지를 해제한다.|
|rm|종료된 컨테이너를 삭제한다.|
|build|컨테이너에 사용되는 이미지를 빌드 혹은 재빌드한다.|
|pull|컨테이너에 사용되는 이미지를 내려받는다.|
|scale|컨테이너의 수를 지정한다.|
|events|컨테이너로부터 실시간으로 이벤트를 수신한다.|
|help|도움말 화면을 출력한다.|

참고 도서
---
#### 그림과 실습으로 배우는 도커&쿠버네티스(위키북스)
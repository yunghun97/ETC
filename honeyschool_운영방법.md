# Honey School 운영방법


## 기본 개발 환경 💾 
1. ### JAVA 11 버전 사용
2. ### Front-end
    - Vue3
    - Vuex4
    - Typescript
    - Bootstrap
3. ### Back-end
    - SpringBoot
    - JPA
4. ### DB
    - MariaDB
5. ### 기타 기술
    - WebRTC(OpenVidu)
    - AWS
    - Docker


## 외부서비스 정리 문서 🧐
[학교정보 가져오기](https://open.neis.go.kr/portal/data/service/selectServicePage.do?page=1&rows=10&sortColumn=&sortDirection=&infId=OPEN17020190531110010104913&infSeq=1)  
[OpenVidu](https://docs.openvidu.io/en/stable/)

## 구동시키기 위해 필요한 것들 👩‍🏫
1. AWS DOCKER 설치
2. AWS Openvidu 설치
3. Certbot 설치
4. Java 11 설치
5. nginx 설치
6. MariaDB Docker로 설치

## 운영메뉴얼 🎉
### 개발 환경
도메인 주소 예시 : i6b201.p.ssafy.io  
AWS 환경 : Ubuntu 20.04

[1. Docker 설치](#Docker-설치)  
[2. Docker-Compose 설치](#Docker-Compose-설치)  
[3. DB 설치](#DB-설치)  
[4. OpenVidu 설치](#OpenVidu-설치)  
[5. Nginx 설치](#Nginx-설치)  
[6. FrontEnd 설정](#FrontEnd-설정)  
[7. BackEnd 설정](#BackEnd-설정)
## Docker 설치
```bash
# 패키지 업데이트
sudo apt-get update
# 도커 설치
sudo apt-get install docker.io -y
# docker 서비스 실행
sudo service docker start
# 권한 부여
shdo chmod 666 /var/run/docker.sock
```
  
## Docker-Compose-설치
1. docker-compose 최신 버전 설치
```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```  
  
2. docker-compose 권한 설정
```bash
sudo chmod +x /usr/local/bin/docker-compose
```  

3. docker version 확인
```bash
docker-compose version
```

## DB 설치
1.  
```bash
docker pull mariadb
```
2.  
```bash
# HoneyShool 에서는 DB port 3310 사용중입니다.
# 다른 포트 설정 시 HoneySchool 소스에서 Backend/src/main/resources/application.properties 에서 DB 포트 변경
docker run --name 컨테이너이름 -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=비번 mariadb
```
- -d : 컨테이너를 백그라운드로 실행
- -p : 3306:3306 호스트-컨테이너 간 포트 연결. 호스트에서 3306포트 접속 시 켄테이너 3306 프토로 포워딩 됨.
- -e : MYSQL_ROOT_PASSWORD=비번 : 컨테이너 내 환경변수 설정. root 계정의 패스워드를 mariadb로 지정
- mariadb : 다운로드 받은 이미지 이름  

3. bash 접속
```bash
docker exec -it 컨테이너ID bash
```
4. mariadb 접속
```bash
mysql -u root -p
```

### 설정 파일변경
character SET 및 시간 설정  
  
1. 설정 들어가기
```bash
vim etc/mysql/my.cnf
```
2. 원하는 포트로 변경
```bash
[client-server] 
port = 3306  # 이걸 원하는 포트로 변경
```
3. 제일 아래에 추가
```bash
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
collation-server = utf8mb4_unicode_ci
init-connect='SET NAMES utf8mb4'
character-set-client-handshake = FALSE
character-set-server = utf8mb4
default-time-zone='+9:00'
```
4. 재실행
```bash
# docker 컨테니어 bash 에서 나가려면 exit 입력
docker restart 컨테이너ID
```

### mysql접속
```bash
mysql -u root -p 
```
### mysql 상태확인
```bash
status
```
### mysql 외부접속 계정 생성
mysql -u root -p  접속 후
```bash
create user 'username'@'%' identified by 'password';
grant all privileges on *.* to 'username'@'%';
flush privileges;
```
  
## 4. OpenVidu 설치
## OpenVidu 설치
1. 서버 포트 구성
```bash
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 3478/tcp
sudo ufw allow 3478/udp
sudo ufw allow 40000:57000/tcp
sudo ufw allow 40000:57000/udp
sudo ufw allow 57001:65535/tcp
sudo ufw allow 57001:65535/udp
```
2. 설치 권장 폴더 생성 및 이동
```bash
mkdir opt
cd opt
```
3. 설치 스크립트 다운로드 및  실행
```bash
curl https://s3-eu-west-1.amazonaws.com/aws.openvidu.io/install_openvidu_latest.sh | bash

```
![image](https://user-images.githubusercontent.com/71022555/151701914-276b12bb-700f-4b31-96a3-388c18cf26c7.png)
4. 사진에 있는 순서대로 실행
```bash
cd openvidu
vim nano.env #
```
5. nano.env 설정
```bash
DOMAIN_OR_PUBLIC_IP=i6b201.p.ssafy.io
OPENVIDU_SECRET=MY_SECRET # 변경 가능
CERTIFICATE_TYPE=selfsigned
#HTTP_PORT=80
#HTTPS_PORT=443
LETSENCRYPT_EMAIL=your@email.co
```
![image](https://user-images.githubusercontent.com/71022555/151702012-5e077a48-456d-4bec-aa0c-ae6310df0cb7.png)

## OpenVidu-Server Custom Dockerizing
1. 디렉토리 생성 후 진입 : /home/ubuntu/ 에서 
```bash
mkdir OpenVidu
cd OpenVidu
```
2. OpenVidu Git clone
```bash
git clone https://github.com/OpenVidu/openvidu.git
```
3. 받아 진 openVidu 폴더 진입
```bash
cd openvidu
```
4. 패키지 클린
```bash
mvn clean install -U
```
5. openvidu/openvidu-server 폴더 지입
```bash
cd openvidu-server
```
6. 패키지 클린
```bash
mvn clean install -U
```
7. 관리자 권한 부여
```bash
sudo su
```
8. 루트 디렉토리 이동
```bash
cd /
```
9. SSL 인증서 경로 진입
```bash
cd /etc/letsencrypt/live/{SSL 인증 받은 도메인}/
# 이떄 도메인은 아까 설정했던 i6팀코드.p.ssafy.io
```
10. ssl
```bash
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out k5팀코드.p.ssafy.io.p12 --name ssafy -CAfile chain.pem -caname root

# 이때 설정하는 비번 보관해두기
# i6팀코드.p.ssafy.io.p12 생성되는지 확인
```
11. openvidu-server 파일에 ssl 인증서 복사, 붙여넣기
a. openvidu-server 의 application.properties가 있는 경로 까지 이동
```bash
cd openvidu/openvidu-server/src/main/resources/
```
b. ssl 인증서 복붙
```bash
cp /etc/letsencrypt/live/i6b201.p.ssafy.io/i6b201.p.ssafy.io.p12 .   # /etc~ 에 있는 파일을  .(현재 바라보고 있는 위치)에 복사
```
12. application.properties 수정
```bash
# 편집기 들어가기
vi application.properties

# 수정하는 내용
server.address=0.0.0.0
server.ssl.enabled=true
server.ssl.key-store=classpath:i6b201.p.ssafy.io.p12
server.ssl.key-store-password=your-password # 위 10번에서 설정한 비번
server.ssl.key-store-type=PKCS12
server.ssl.key-alias=ssafy
server.servlet.session.cookie.name=OVJSESSIONID
logging.level.root=info
spring.main.allow-bean-definition-overriding=true
SUPPORT_DEPRECATED_API=true
DOTENV_PATH=.
DOMAIN_OR_PUBLIC_IP=i6b201.p.ssafy.io
OPENVIDU_SECRET=MY_SECRET # openvidu .env 설정과 통일
CERTIFICATE_TYPE=selfsigned
HTTPS_PORT=5443 # 포트번호는 자유
KMS_URIS=["ws://localhost:8888/kurento"]
```
13. openvidu 최상위 파일로 이동
14. 메이븐 빌드
```bash
mvn package -DskipTests
```
15. openvidu-server 파일로 이동
```bash
cd openvidu-server
```
16. 파일 접근 권한 부여
```bash
chmod 777 create_image.sh
```
17. 도커 이미지 생성
```bash
./create_image.sh 2.20.1
# 이때 2.20.1은 기존의 openvidu server가 2.20.0 버전으로 도커 실행중이라 구분을 위해 버전을 달리 해줌
```
18. 
```bash
cd opt/openvidu
```
19. 도커 컴포즈 파일 수정
```bash
vi docker-compose.yml
# 내용
services:
openvidu-server:
image: openvidu/openvidu-server:2.20.1
restart: on-failure
network_mode: host
entrypoint: ['/usr/local/bin/entrypoint.sh']
volumes:
- /var/run/docker.sock:/var/run/docker.sock
- ${OPENVIDU_RECORDING_PATH}:${OPENVIDU_RECORDING_PATH}
- ${OPENVIDU_RECORDING_CUSTOM_LAYOUT}:${OPENVIDU_RECORDING_CUSTOM_LAYOUT}
- ${OPENVIDU_CDR_PATH}:${OPENVIDU_CDR_PATH}
env_file:
- .env
environment:
- SERVER_SSL_ENABLED=true
- SERVER_PORT=5443 # application.properties 설정과 일치
- KMS_URIS=["ws://localhost:8888/kurento"]
- COTURN_REDIS_IP=127.0.0.1
OpenVidu 설치부터 테스트까지 6
- COTURN_REDIS_PASSWORD=${OPENVIDU_SECRET}
- COTURN_IP=${COTURN_IP:-auto-ipv4}
```
  
20. openvidu .env 설정
```bash
# 설정 파일 접속
nano .env
# 수정할 내용
CERTIFICATE_TYPE=letsencrypt

LETSENCRYPT_EMAIL=yunghun97@naver.com

```

21. openvidu nginx certificate 설정
```bash
cd certificates/live

cp /etc/letsencrypt/live/i6b201.p.ssafy.io/i6팀코드.p.ssafy.io.p12 .    # etc~ 에 있는 파일을 .(현재 바라보고 있는 위치)에 복사
```
## 실행
1. /opt/openvidu 경로에서 
```bash
./openvidu start
# 이전에 실행 했다면 stop -> start  또는 retart

```
2. 도커 확인
```bash
docker ps
```
![docker 확인](https://user-images.githubusercontent.com/71022555/153356018-2454c187-897a-452b-b2a7-63b82e7525a5.png)

3. 접속
```
i6b201.p.ssafy.io:5443
```
![openvidu](https://user-images.githubusercontent.com/71022555/153356174-4e567d08-2e97-4f61-873c-39df469be6c1.png)

4. 테스트 openvidu tutorials 코드 다운 후 실행 시 작동하는지 확인
[OpenVidu tutorials](#https://docs.openvidu.io/en/stable/tutorials/)  
```bash
# 아까 설정한 도메인 + port
const OPENVIDU_SERVER_URL = "https://i6b201.p.ssafy.io:5443";
# .env 에 저장한 비밀번호
const OPENVIDU_SERVER_SECRET = "ssafy";
```
  
## Nginx 설치
1. Nginx 설치하기
```bash
apt get install nginx
```
2. Nginx 설정
```bash
cd /etc/nginx/sites-enabled
```
![Nginx 설정](https://user-images.githubusercontent.com/71022555/154523129-a66d048d-7e9b-4fae-b3f8-dffc5ece1222.png)
3. Nginx 구동
```bash
service nginx start
```
  
## FrontEnd 설정
1. 소스 다운 받기
```bash
git clone -b develop https://lab.ssafy.com/s06-webmobile1-sub2/S06P12B201.git
# 현재 /home/ubuntu/honeyschool 폴더에 받았다고 가정하고 진행
# 받으면 S06P12B201 폴더 생성 됨
```
2. 이동하기
```bash
# 이동
cd /var/www/html
# 기존의 dist 폴더 지워주기
rm -rf dist
# 새로 dist 폴더 바꿔주기
cp -r /home/ubuntu/honeyschool/S06P12B201/frontend/dist .
```
3. 도메인 접속해서 작동 유무 확인

## BackEnd 설정
1. 6단계에서 소스 다운 받았다고 가정 및 Openvidu 설치 시 사용한 p12키를 사용하여 SSL 구동합니다.
```bash
# 백엔드 소스 안에 진입 & 현재 
cd /home/ubuntu/honeyschool/S06P12B201/Backend/src/main/resources
# Openvidu 에서 사용한 키 복사
cp /home/ubuntu/opt/openvidu/certificates/live/i6b201.p.ssafy.io.p12 .
# application.properties 수정
nano application.properties

# SSL 인증 부분 수정
server.ssl.key-store=classpath:i6b201.p.ssafy.io.p12 # classpath:{키이름} classpath: src/main/resouces의 경로를 나타낸다. 
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=ssafy  # p12 키 생성하면서 사용한 비밀번호
```
2. 빌드하기
```bash
# 백엔드 프로젝트 최상위 폴더로 이동
cd /home/ubuntu/honeyschool/S06P12B201/Backend
# 권한 부여하기
chmod 700 gradlew
# build 하기
./gradlew clean build # 성공시 build 생성
# jar 파일 생성확인
cd /build/libs
ls
# Backend-0.0.1-SNAPSHOT.jar 파일 생성확인

# 구동확인
java -jar Backend-0.0.1-SNAPSHOT.jar
# 에러 없으면 build 성공
```
3. DockerFile 만들기
```bash
nano Dockerfile  # f 소문자로 해야합니다!

#아래 내용 작성
FROM openjdk:11
ARG JAR_FILE=./Backend-0.0.1-SNAPSHOT.jar
copy ${JAR_FILE} honeyschool.jar
ENTRYPOINT ["java", "-jar", "honeyschool.jar"]
EXPOSE 9999

# 설명
## FROM 명령어 -> 스프링 부트 애플레케이션이 돌아갈 베이스 이미지를 의미합니다. 베이스 이미지는 docker hub 사이트 참조
## ARG 환경변수를 만들어 준다. -> jar 파일의 위치를 정해줌
## COPY jar 파일을 honeyschool.jar이라는 이름으로 복사
## ENTRYPOINT 애플리케이션을 실행시킬 명령어
## EXPOSE 애플리케이션이 사용하는 포트 
```

3. Docker Image 만들기
```bash
docker build -t yunghun97/test .

```

4. Docker 컨테이너 기본 실행 시 (컨테이너 삭제 시 데이터 날라감)
```bash
docker run --name {원하는 컨테이너 이름} -d -p 9999:9999 {이미지 이름}

## -d 는 백그라운드에서 작동
## -p IN:OUT IN으로 들어오는 포트를 OUT 포트로 매핑해준다.
```
---  

4. 도커 Volume 생성 및 마운트 하기
1. 불륨 생성하기
```bash
docker volume create {volume 명}

# docker volume create files
```
2. 불륨 생성확인
```bash
docker volume ls

```
결과  
![불륨확인](https://user-images.githubusercontent.com/71022555/154391203-37f90b1c-22d9-4e44-afb5-eab5765bd5e7.png)  

3. 불륨 정보 확인
```bash
docker volume inspect {files}
```
결과  
![불륨정보확인](https://user-images.githubusercontent.com/71022555/154391362-2e440cde-a2d7-4c15-a3e6-a64c333a7515.png)  

4. 불륨 마운트
```bash
docker run -v {불륨이름}:{마운트할 컨테이너 내부 경로} --name {컨테이너이름} -d -p 9999:9999 {이미지이름}

# docker run -v files:/home/ubuntu/honeyschool/file --name honeyschool_be -d -p 9999:9999 yunghun97/v0.9
```
5. 마운트 적용 확인
```bash
docker inspect {컨테이너 이름}

#docker inspect honeyschool_be
```
결과  
![마운트결과확인](https://user-images.githubusercontent.com/71022555/154392384-9da1c54b-f57e-43cd-a666-ed82082c2a36.png)  

6. 불륨 동기화 되었는지 확인하기
```bash
docker volume inspect files
#생략
"Mountpoint": "/var/lib/docker/volumes/{불륨명}/_data",
#생략

# 해당 폴더로 이동
cd /var/lib/docker/volumes/files/_data
# 파일 확인
ls
```

## 🎉🎉🎉🎉🎉🎉🎉구동준비가 끝났습니다.!! 🎉🎉🎉🎉🎉🎉🎉

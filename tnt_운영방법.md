# TNT 운영방법


## 기본 개발 환경 💾 
1. ### JAVA 11
2. ### Front-end
    - Vue3
    - Vuex4    
    - Bootstrap 5
3. ### Back-end
    - SpringBoot
    - JPA
4. ### DB
    - MariaDB
    - Redis
5. ### 기타 기술
    - Jenkins
    - AWS
    - Docker


## 구동시키기 위해 필요한 것들 👩‍🏫
1. AWS DOCKER 설치
2. AWS Openvidu 설치
3. Certbot 설치
4. Java 11 설치
5. nginx 설치
6. MariaDB Docker로 설치
7. Jenkins 설치

## 운영메뉴얼 🎉
### 개발 환경
도메인 주소 예시 : j6b201.p.ssafy.io  
AWS 환경 : Ubuntu 20.04 LTS

[1. Docker 설치](#Docker-설치)  
[2. Docker-Compose 설치](#Docker-Compose-설치)  
[3. Certbot 설정](#Certbot-설정)  
[4. DB 설치](#DB-설치)  
[5. Nginx 설정](#Nginx-설정)  
[6. 배포 자동화 하기](#Jenkins-배포)  
## Docker 설치
```bash
# 패키지 업데이트
sudo apt-get update
# 도커 설치
sudo apt-get install docker.io -y
# docker 서비스 실행
sudo service docker start
# 권한 부여
sudo chmod 666 /var/run/docker.sock
```
--- 
## 🎈
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
---  
## 🎆
## DB 설치
1.  
```bash
docker pull mariadb
```
2.  
```bash
# HoneyShool 에서는 DB port 3310 사용중입니다.
# 다른 포트 설정 시 HoneySchool 소스에서 Backend/src/main/resources/application.properties 에서 DB 포트 변경
docker run --name 컨테이너이름 -d -p 3307:3307 -e MYSQL_ROOT_PASSWORD=비번 mariadb
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
---  
## 🎇  
## Certbot 설정
### https 설정을 위한 Certbot 설정  

### Nginx 설치
```bash
sudo apt install -y nginx
```
### Certbot 설치
```bash
sudo snap install --classic certbot
```

### 인증서발급
```bash
sudo certbot --nginx -d {발급받을 도메인}

# 이메일 입력
# 서비스 약관 동의
# 1. No redirect, Redirect 설정
```
  
### SSL 자동 갱신
```bash
sudo certbot renew --dry-run

#Congraturation 뜨면 적용 된것

# 수동 갱신 -> sudo certbot renew
```

### 키 확인
```bash
cd etc/letsencrypt/live/{도메인}
```

### pem 인증서 pkcs12로 변환
```bash
cd etc/letsencrypt/live/{도메인}

# openssl pkcs12 -export -inkey {privkey} -in {certificate} -out {pkcs12 키 이름}
openssl pkcs12 -export -inkey privkey.pem -in cert.pem -out ssafykey.p12
# 이후 비밀번호 설정하시면 됩니다.
```  
---  
## 🎃
## Nginx 설정
1. Nginx 설정
```bash
cd /etc/nginx/sites-enabled
nano default
```
2. defalut 설정 내용
```bash
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	# SSL configuration
	#
	# listen 443 ssl default_server;
	# listen [::]:443 ssl default_server;
	#
	# Note: You should disable gzip for SSL traffic.
	# See: https://bugs.debian.org/773332
	#
	# Read up on ssl_ciphers to ensure a secure configuration.
	# See: https://bugs.debian.org/765782
	#
	# Self signed certs generated by the ssl-cert package
	# Don't use them in a production server!
	#
	# include snippets/snakeoil.conf;
	
	ssl_certificate /etc/letsencrypt/live/j6b206.p.ssafy.io/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/j6b206.p.ssafy.io/privkey.pem;

#	root /var/www;
	root /var/jenkins/workspace/tnt/frontend/dist;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;

	server_name j6b206.p.ssafy.io;

	location / {
#		try_files $uri $uri/ index.html;
		return 301 https://j6b206.p.ssafy.io$request_uri;
	}
	access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;
		
	# pass PHP scripts to FastCGI server
	#
	#location ~ \.php$ {
	#	include snippets/fastcgi-php.conf;
	#
	#	# With php-fpm (or other unix sockets):
	#	fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
	#	# With php-cgi (or other tcp sockets):
	#	fastcgi_pass 127.0.0.1:9000;
	#}

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
	#location ~ /\.ht {
	#	deny all;
	#}
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#	listen 80;
#	listen [::]:80;
#
#	server_name example.com;
#
#	root /var/www/example.com;
#	index index.html;
#
#	location / {
#		try_files $uri $uri/ =404;
#	}
#}

server {

	# SSL configuration
	#
	#listen 443 ssl default_server;
	#listen [::]:443 ssl default_server;
	#
	# Note: You should disable gzip for SSL traffic.
	# See: https://bugs.debian.org/773332
	#
	# Read up on ssl_ciphers to ensure a secure configuration.
	# See: https://bugs.debian.org/765782
	#
	# Self signed certs generated by the ssl-cert package
	# Don't use them in a production server!
	#
	# include snippets/snakeoil.conf;

	root /var/jenkins/workspace/tnt/frontend/dist;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;
        server_name j6b206.p.ssafy.io; # managed by Certbot


	#location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
	#	try_files $uri $uri/ index.html;
	#}
	
	location /jenkins {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_http_version 1.1;
            proxy_request_buffering off;
            proxy_pass http://j6b206.p.ssafy.io:9099;
            proxy_read_timeout 90;
        }
	
	#location /{
	#
	#}	

	#location /test{
	#	rewrite ^ http://j6b206.p.ssafy.io:9090? permanent;
	#}
	# pass PHP scripts to FastCGI server
	#
	#location ~ \.php$ {
	#	include snippets/fastcgi-php.conf;
	#
	#	# With php-fpm (or other unix sockets):
	#	fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
	#	# With php-cgi (or other tcp sockets):
	#	fastcgi_pass 127.0.0.1:9000;
	#}

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
	#location ~ /\.ht {
	#	deny all;
	#}


    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/j6b206.p.ssafy.io/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/j6b206.p.ssafy.io/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
#server {
#    if ($host = j6b206.p.ssafy.io) {
#       return 301 https://$host$request_uri;
#    } # managed by Certbot
 
	
#	listen 80 ;
#	listen [::]:80 ;
#        server_name j6b206.p.ssafy.io;
#        return 404; # managed by Certbot
#}

```

3. Nginx 구동
```bash
service nginx restart
```  
---  
## 🎐
## Jenkins 배포

## 0. Jenkins Docker로 실행
```bash
## JAVA 11로 설치
docker run -d -p 9090:8080 -p 50000:50000 --restart=always -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -u root jenkins/jenkins:lts-jdk11

# 이미지 링크 https://hub.docker.com/r/jenkins/jenkins
```
## 1. Git 연동
> Git Plugin 설치

## 2. Node.js 설치
> Jenkins 관리 -> Global Tool Configuration > Node.JS 탭 작성

## 3. 프로젝트 생성 및 생성
> Freestyle project 
![image](https://user-images.githubusercontent.com/71022555/162229016-46d9a8bb-db28-46d7-b1e4-6ec76c3e1023.png)
  
- https 인 경우 Credentals 생성해서 해당 Credentials로 세팅 해줘야 합니다.

## 3-1 빌드 유발
![image](https://user-images.githubusercontent.com/71022555/162229189-629c652b-565e-4ceb-b78e-cb58848e4470.png)  
- Push Events일 때 설정

## 3-2 빌드 유발 -> Secret token 발급 받기
> 빌드 유발 탭에서 Secret token -> Generate 통해 토큰 생성 -> gitlab에서 사용
> ex) b356d45696925a2f09675c0ce64b0849
## 3-3 Build 설정
```bash
# Execute shell 기준
# 프론트 부분
cd frontend
npm install
npm run build

# 백엔드 부분
cd ..
cd backend
chmod 755 gradlew
./gradlew clean build
docker stop tnt # 처음 실행할 때 구성에서 제외
docker rm tnt # 처음 실행할 때  구성에서 제외 -> 처음에는 tnt라는 컨테이너가 존재하지 않아서 오류 발생합니다.
docker build -t yunghun97/tnt .
docker run --name tnt -d -p 9999:9999 yunghun97/tnt

# DockerFile(백엔드 루트 위치에 추가되어 있다고 가정)
## Docker file 소스

#FROM openjdk:11
#ARG JAR_FILE=./build/libs/TNT_hadoop-0.0.1-SNAPSHOT
#copy ${JAR_FILE} tnt.jar
#ENTRYPOINT ["java", "-jar", "tnt.jar"]
#EXPOSE 9990
```

## 4 Git Lab 설정
![image](https://user-images.githubusercontent.com/71022555/162230641-d5736cd4-50f1-47c0-ae10-68b20d5e889f.png)
  
> Setting -> Webhooks
## 4-1 WebHooks 설정
![image](https://user-images.githubusercontent.com/71022555/162230863-fbb9c142-a813-440a-af1d-732a89188086.png)  
```bash
URL = 도메인/project/프로젝트명
token = 발급받은 jenkins 프로젝트 토큰
```

## 4-2 배포 테스트
![image](https://user-images.githubusercontent.com/71022555/162250788-2f442112-a4ba-4478-b1f4-a64e71a9d714.png)  
  
## 4-3 200 확인
![image](https://user-images.githubusercontent.com/71022555/162250881-5a87e929-b30c-482b-b228-6ab8b8268d0d.png)
    
## 4-4 Jenkins 확인
![image](https://user-images.githubusercontent.com/71022555/162251895-1a88ebf4-6b3b-4360-b82e-205f0371ac55.png)
  

## 🎉🎉🎉🎉🎉🎉🎉구동준비가 끝났습니다.!! 🎉🎉🎉🎉🎉🎉🎉

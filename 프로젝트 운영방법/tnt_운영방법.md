# Today News Talk
<div align="center">
    <img src="https://img.shields.io/badge/Ubuntu-20.04.3 LTS-E95420?style=flat&logo=Ubuntu&logoColor=white"/><img src="https://img.shields.io/badge/MySQL-8.0.28-4479A1?style=flat&logo=MySQL&logoColor=white"/><img src="https://img.shields.io/badge/NGINX-1.18.0(ubuntu)-009639?style=flat&logo=NGINX&logoColor=white"/><br/><img src="https://img.shields.io/badge/Vue.js-3.2.31-4FC08D?style=flat&logo=Vue.js&logoColor=white"/><img src="https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=HTML5&logoColor=white"/><img src="https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=CSS3&logoColor=white"/><img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=JavaScript&logoColor=white"/><br/><img src="https://img.shields.io/badge/GitLab-FCA121?style=flat&logo=GitLab&logoColor=white"/><img src="https://img.shields.io/badge/Jira-0052CC?style=flat&logo=Jira Software&logoColor=white"/><img src="https://img.shields.io/badge/Notion-000000?style=flat&logo=Notion&logoColor=white"/><img src="https://img.shields.io/badge/Mattermost-0058CC?style=flat&logo=Mattermost&logoColor=white"/></div>


![Spring](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white) ![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white) ![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)![Bootstrap](https://img.shields.io/badge/bootstrap-%23563D7C.svg?style=for-the-badge&logo=bootstrap&logoColor=white)

# 서비스 소개
당일 뉴스들의 주요 키워드 및 통계를 보여주고
오늘의 뉴스의 전반적인 기사경향을 
한 눈에 알아볼 수 있도록 
기사를 가공하여 정보를 제공하는 서비스


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

## 포트 정보 ⚙
> Backend : 9999
> Mariadb : 3307
> Redis : 3308
> Jenkins : 9090, 50000
> Hadoop : 8088


## 구동시키기 위해 필요한 것들 👩‍🏫
1. AWS DOCKER 설치
2. Certbot 설치
3. Java 11 설치
4. nginx 설치
5. MariaDB Docker로 설치
6. Redis 설치
7. Jenkins 설치
8. Hadoop 에코시스템 구축

## 운영메뉴얼 🎉
### 개발 환경
도메인 주소 예시 : j6b201.p.ssafy.io  
AWS 환경 : Ubuntu 20.04 LTS

[1. Docker 설치](#Docker-설치)  
[2. Docker-Compose 설치](#Docker-Compose-설치)  
[3. Certbot 설정](#Certbot-설정)  
[4. DB 설치](#DB-설치)
[5. Redis 설치](#Redis-설치)
[6. Nginx 설정](#Nginx-설정)  
[7. 배포 자동화 하기](#Jenkins-배포)  
[8. Hadoop 설치](#Hadoop-설치)  
[9. Sqoop 설치](#Sqoop-설치)  
[10. Oozie 설치](#Oozie-설치)  
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
  
## 👕
## Redis 설치

1. docker pull redis
```bash
docker pull redis 
```
2. 이미지 확인
``` 
docker images
```
3. 도커로 만들기
```bash
sudo docker run -d -p 6380:6379 --name 이름 redis --requirepass 비번

# volume 마운트
# docker run -v {불륨이름or경로}:{마운트할 컨테이너 내부 경로} -d -p 3308:6379 --name {컨테이너 이름} redis --requirepass {비번}
docker run -v redisData:/data -d -p 3308:6379 --name redis redis --requirepass ssafy
```
4. 접속 테스트
```bash
docker exec -it 컨테이너아이디 redis-cli 
```

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

---
## 🐘
## Hadoop 설치
## 1. 우분투 컨테이너 설치
```
docker run -i -t --name hadoop-base ubuntu 
```
ctrl + P, Q 로 컨테이너 정지하지 않고 쉘 빠져나오기 가능
단, docker run -it 옵션인 경우에만 가능

## 2. open jdk 설치(컨테이너)
```
add-apt-repository ppa:openjdk-r/ppa 
apt-get update  
apt-get install openjdk-8-jdk 
java -version 
```

## 3. 하둡 설치 (컨테이너)
```
apt-get install wget 
cd ~ 
mkdir soft 
cd soft/ 
mkdir apache 
cd apache/ 
mkdir hadoop 
cd hadoop/ 
wget http://mirrors.sonic.net/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz 
tar xvzf hadoop-2.7.7.tar.gz 
```

## 4. bashrc 수정 (컨테이너)
```
apt-get install vim -y 
vi ~/.bashrc 
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export HADOOP_HOME=/root/soft/apache/hadoop/hadoop-2.7.7 
export HADOOP_CONFIG_HOME=$HADOOP_HOME/etc/hadoop 
export PATH=$PATH:$HADOOP_HOME/bin 
export PATH=$PATH:$HADOOP_HOME/sbin 
source ~/.bashrc 
```

## 5. 디렉토리 생성 및 하둡 설정파일 수정 (컨테이너)
```
cd $HADOOP_HOME/ 
mkdir tmp 
mkdir namenode 
mkdir datanode 
cd $HADOOP_CONFIG_HOME/ 
cp mapred-site.xml.template mapred-site.xml 
```

## 6. core, hdfs, mapred, yarn 파일 수정 (컨테이너)
hadoop-env.sh
```
# 추가
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

core-site.xml
```
<configuration>
    <property>
            <name>hadoop.tmp.dir</name>
            <value>/root/soft/apache/hadoop/hadoop-2.7.7/tmp</value>
            <description>A base for other temporary directories.</description>
    </property>

    <property>
            <name>fs.default.name</name>
            <value>hdfs://master:9000</value>
            <final>true</final>
            <description>The name of the default file system.  A URI whose
            scheme and authority determine the FileSystem implementation.  The
            uri's scheme determines the config property (fs.SCHEME.impl) naming
            the FileSystem implementation class.  The uri's authority is used to
            determine the host, port, etc. for a filesystem.</description>
    </property>
</configuration>
```

hdfs-site.xml
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
        <final>true</final>
        <description>Default block replication.
        The actual number of replications can be specified when the file is created.
        The default is used if replication is not specified in create time.
        </description>
    </property>

    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/root/soft/apache/hadoop/hadoop-2.7.7/namenode</value>
        <final>true</final>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/root/soft/apache/hadoop/hadoop-2.7.7/datanode</value>
        <final>true</final>
    </property>
</configuration>
```

mapred-site.xml
```
<configuration>
    <property>

        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>

    <property>

        <name>mapred.job.tracker</name>
        <value>master:9001</value>
        <description>The host and port that the MapReduce job tracker runs
        at.  If "local", then jobs are run in-process as a single map
        and reduce task.
        </description>
    </property>
</configuration>
```

yarn-site.xml
```
<configuration>

<!-- Site specific YARN configuration properties -->
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.resourcemanager.address</name>
    <value>master:8032</value>
  </property>
  <property>
    <name>yarn.resourcemanager.scheduler.address</name>
    <value>master:8030</value>
  </property>
  <property>
    <name>yarn.resourcemanager.resource-tracker.address</name>
    <value>master:8031</value>
  </property>
  <property>
    <name>yarn.resourcemanager.admin.address</name>
    <value>master:8033</value>
  </property>
  <property>
    <name>yarn.resourcemanager.webapp.adress</name>
    <value>master:8088</value>
  </property>
</configuration>
```

## 7. 네임노드 포맷 (컨테이너)
```
hadoop namenode -format 
```

## 8. ssh 설정 (컨테이너)
```
apt-get install ssh 
```
~/.bashrc 수정
```
#autorun  
/usr/sbin/sshd
```

## 9. 컨테이너 이미지 commit (호스트)
```
docker start <container id>
docker commit -m "hadoop install in ubunu" <container id> ubuntu:hadoop 
docker image ls 
```

## 10. master, slave 생성 (호스트)
```
docker run -i -t -h master --name master -p 50070:50070 -p 8088:8088 ubuntu:hadoop 
docker run -i -t -h slave1 --name slave1 --link master:master ubuntu:hadoop 
docker run -i -t -h slave2 --name slave2 --link master:master ubuntu:hadoop 
```

## 11. slave 컨테이너 ip 확인 (호스트)
```
docker inspect slave1 (172.17.0.4) 
docker inspect slave2 (172.17.0.5) 
```

## 12. 하둡 설정 및 구동 (컨테이너)
```
docker attach master (접속. 컨테이너는 이미 구동중) 

vim /etc/hosts 
172.17.0.3		master
172.17.0.4      slave1 
172.17.0.5      slave2 
vim $HADOOP_CONFIG_HOME/slaves 

# 데이터노드가 될 컨테이너 목록들.  
slave1 
slave2 
master 

start-all.sh 
```
hosts 파일의 내용은 컨테이너를 재시작하면 아래에서 기입한 내용은 없어진다. 재시작할때마다 다시 입력하든지, 컨테이너를 시작할 때 입력해주는 쉘 스크립트를 만들든지 하면 된다.

## 13. 워드카운트 테스트 (컨테이너)
```
cd ~ 
cd soft/apache/hadoop/hadoop-2.7.7 
hadoop fs -mkdir /input 
hadoop fs -put LICENSE.txt /input 
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount /input /output 
hadoop fs -cat /output/*
```

## 14. Resource manager 웹 UI 확인
http://<host ip>:8088 접속

![img](https://blog.kakaocdn.net/dn/eCtOU4/btqDA4KyP3s/ZKGYtmfBQuDPSo7aSVmLA0/img.png)   



---

## 💧
## Sqoop 설치
2.1. 다운로드

   - 스쿱 프로젝트 사이트에서 최신 안정 배포판을 다운로드 받아 적절한 위치에 압축 해제

   - bin 디렉토리 아래 sqoop 명령을 실행하여 정상 실행 여부 확인

## 최근 안정 버전 (1.4.7) 다운로드
```
wget http://mirror.navercorp.com/apache/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
```
## 압축 해제
```
tar xvf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
```
## 실행 확인
```
cd sqoop-1.4.7.bin__hadoop-2.6.0
bin/sqoop help
```

2.2. 환경 변수 설정

   - .bashrc 파일에 SQOOP_HOME 및 SCOOP_CONF_DIR 설정

## 텍스트 편집기로 .bashrc 파일 열기 & 스쿱의 환경변수 추가 
```
vi ~/.bashrc  
```
## scoop config 
```
export SQOOP_HOME=/home/hadoop/bigdata/sqoop-1.4.7.bin__hadoop-2.6.0 
export SQOOP_CONF_DIR=/home/<username>/<any directory>/sqoop-1.4.7.bin__hadoop-2.6.0/conf 
export PATH=$PATH:$SQOOP_HOME/bin 
```
   - sqoop-env.sh 파일에 하둡 관련 설정

## 스쿱 설치폴더의 conf 디렉토리로 이동
```
cd conf/
```
## sqoop-env-template.sh 파일을 sqoop-env.sh 이름으로 바꾸어 복사  
```
cp sqoop-env-template.sh sqoop-env.sh  
```
## sqoop-env.sh 파일을 수정하기 위해  텍스트 편집기로 열기 
```
vi sqoop-env.sh 
```
## hadoop 관련 설정 
```
export HADOOP_COMMON_HOME=/home/<username>/<any directory>/hadoop
export HADOOP_HOME=/home/<username>/<any directory>/hadoop 
export HADOOP_MAPRED_HOME=/home/<username>/<any directory>/hadoop
```

2.3. JDBC 연결 설정

   - 스쿱은 JDBC를 통한 RDBMS 연결을 지원

   - 연결하고자 하는 DBMS에 대한 JDBC 드라이버를 다운받아 $SQOOP_HOME/lib에 위치 시키고

   - SQOOP 실행 시 JDBC 연결 문자열을 파라미터로 사용하여 연결

## MySQL Connector/J 5.1.48 다운로드 & 설치 

## https://dev.mysql.com/downloads/connector/j/ 
## Select Operating System 콤보박스에서 Platform Independent 선택
## "Looking for previous GA version?" 링크 선택 > *.tar.gz 파일 다운로드

## 압축 해제 
```
tar xvf mysql-connector-java-5.1.48.tar.gz 
```
## jar파일을 cp 명령어로 sqoop의 라이브러 리로 복사
```
cd mysql-connector-java-5.1.48
cp mysql-connector-java-5.1.48-bin.jar $SQOOP_HOME/lib 
```
※ commons-lang-2.6.jar 파일을 다운로드 받아 $SQOOP_HOME/lib에 위치

   - sqoop import 수행 시, java.lang.NoClassDefFoundError: org/apache/commons/lang/StringUtils 발생

 

3. 스쿱 실행

3.1. import 및 export 작업 수행

   - SQOOP은 sqoop COMMAND [ARGS] 형태로 실행하며, sqoop help 입력하여 사용 가능한 명령 확인 가능 

   - classicmodels의 customers 테이블의 복제 테이블을 customers_export로 만들어 두고

     customers 테이블의 데이터를 import 한 후, import한 데이터를 customers_export 테이블에 export 하고자 함

## import (from classicmodels.customers to sqoop_out1 directory )
```
sqoop import --connect jdbc:mysql://localhost/classicmodels --table customers --target-dir sqoop_out1 --enclosed-by '\"' --username root -P 
```
## export (from sqoop_out1 to classicmodels.customers_export)
```
sqoop export --connect jdbc:mysql://localhost/classicmodels --table customers_export --export-dir sqoop_out1 --enclosed-by '\"' --username root -P
```
--enclosed-by '\"' : import / export 작없 시 필드 구분자로 ','를 디폴트로 사용함. customers 테이블의 필드 내 문자열이 ','를 포함하고 있어 "로 필드 값을 둘러 쌈 

 

3.2. 기타 작업

   - eval : SQL 쿼리를 수행하고 결과를 표시함

   - list-databases : 서버에서 사용가능한 데이터베이스 목록 표시

   - list-tables : 데이터베이스 내에 접근 가능한 테이블 목록 표시

## 조회
```
sqoop eval --connect jdbc:mysql://localhost/classicmodels --username root -P --query 'SELECT customerNumber, customerName, phone, city, state FROM customers where country = "USA"'
```
## dml 
```
sqoop eval --connect jdbc:mysql://localhost/classicmodels --username root --P --e 'INSERT INTO customers_export VALUES (500, "Mad Cat, Inc", "Christmas", "Merry", "010-1234-5678", "500, BNL", null, "New York", null, null, "USA", null, null)'
```
## 데이터베이스 목록 조회
```
sqoop list-databases --connect jdbc:mysql://localhost/classicmodels --username root --P
```
## 테이블 목록 조회
```
sqoop list-tables --connect jdbc:mysql://localhost/classicmodels --username root --P
```

---
## 🧭 
## Oozie 설치
## Java
```
java -version
```

## Maven
```
mvn -version
```

## Hadoop
+ core-site.xml
```
 <property>
    <name>hadoop.proxyuser.root.groups</name>
    <value>*</value>
  </property>
  <property>
    <name>hadoop.proxyuser.root.hosts</name>
    <value>*</value>
  </property>
```
+ oozie 디렉토리 생성
```
hadoop fs -mkdir /user/oozie
hadoop fs -chown oozie:hadoop /user/oozie
```

## MySQL
```
mysql --version
```

## Oozie 소스 다운로드 및 빌드
```
wget http://apache.mirror.cdnetworks.com/oozie/4.3.1/oozie-4.3.1.tar.gz
tar -xvzf oozie-4.3.1.tar.gz -C /app/oozie/.
mv /app/oozie/oozie-4.3.1 /app/oozie/4.3.1
cd /app/oozie/4.3.1
vi pom.xml
```
빌드
```
cd /app/oozie/4.3.1/bin
./mkdistro.sh -DskipTests
```

## oozie-site.xml
```
<configuration>
    <property>  
        <name>oozie.service.JPAService.jdbc.driver</name>  
        <value>com.mysql.jdbc.Driver</value>  
    </property>  

    <property>  
        <name>oozie.service.JPAService.jdbc.url</name>  
        <value>jdbc:mysql://localhost:3306/oozie</value>  
    </property>  

    <property>  
        <name>oozie.service.JPAService.create.db.schema</name>  
        <value>true</value>  
    </property>  

    <property>  
        <name>oozie.service.JPAService.jdbc.username</name>  
        <value>oozie</value>  
    </property>  

    <property>  
        <name>oozie.service.JPAService.jdbc.password</name>  
        <value>oozie</value>  
    </property>
</configuration>
```

## 라이브러리 추가
```
mkdir /app/oozie/libext
cd /app/oozie/libext
find /app/hadoop/share/hadoop -name "*.jar" | xargs  -I {} cp {} .
cp /app/hive/lib/mysql-connector-java-5.1.47.jar .
wget http://archive.cloudera.com/gplextras/misc/ext-2.2.zip
```

## webapp 생성
```
oozie-setup.sh prepare-war 
```

## hadoop파일시스템에 공유 라이브러리 업로드
```
oozie-setup.sh sharelib create -fs hdfs://192.168.179.144:8020
```

## mysql 메타데이터 생성
```
ooziedb.sh create -sqlfile ooziedb_init_schema.sql -run
```

## Oozie 기동 
```
oozied.sh start
```
http://192.168.179.144:11000/oozie/ url에서 web console 접근이 가능하다.

![img](https://oboki.net/workspace/wp-content/uploads/2019/03/web_console.png)

## 종료
```
oozied.sh stop
```


## 🎉🎉🎉🎉🎉🎉🎉구동준비가 끝났습니다.!! 🎉🎉🎉🎉🎉🎉🎉

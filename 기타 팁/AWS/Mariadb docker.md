# Mariadb docker로 설치

1.
```bash
docker pull mariadb
```
2.
```bash
docker run --name 컨테이너이름 -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=비번 mariadb
# docker run -v mariadb_volume:/var/lib/mysql --name mariadb -d -p 3307:3307 -e MYSQL_ROOT_PASSWORD=ssafy mariadb
# -v {불륨이름}:마운트할주소 --name 컨테이너이름 -d -p {접속하는포트}:{포워딩되는포트} -e ~~
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

1.
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
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
default-time-zone='+9:00'
```
4. 재실행
```bash
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


# MySQL docker 설치

### 1. mysql 이미지 받기
```bash
docker pull mysql
```

### 2. 컨테이너 실행
```bash
docker run --name 컨테이너이름 -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=비번 이미지이름

# 기본
# docker run --name mysql -d -p 3307:3307 -e MYSQL_ROOT_PASSWORD=ssafy mysql
## 불륨 설정
# docker run -v mysql_volume:/var/lib/mysql --name mysql -d -p 3307:3307 -e MYSQL_ROOT_PASSWORD=ssafy mysql

# -v {불륨이름}:마운트할주소 --name 컨테이너이름 -d -p {접속하는포트}:{포워딩되는포트} -e ~~
- -d : 컨테이너를 백그라운드로 실행
- -p : 3306:3306 호스트-컨테이너 간 포트 연결. 호스트에서 3306포트 접속 시 켄테이너 3306 프토로 포워딩 됨.
- -e : MYSQL_ROOT_PASSWORD=비번 : 컨테이너 내 환경변수 설정. root 계정의 패스워드를 mariadb로 지정
- mariadb : 다운로드 받은 이미지 이름  
```

### 3. MySQL Docker 컨테이너 접속
```bash
docker exec -it mysql bash
```

### 4. 시간 수정  

- 시스템 파일 수정
```bash
vim etc/mysql/my.cnf

[mysqld]
default-time-zone='+9:00'
```
  
- 쿼리로 수정
```sql
SET GLOBAL time_zone='Asia/Seoul';
set time_zone='Asia/Seoul';


<!-- time zone 확인 -->
select @@global.time_zone, @@session.time_zone;
SELECT now();
```
### 5. 계정 추가
mysql -u root -p  접속 후
```bash
create user 'username'@'%' identified by 'password';
grant all privileges on *.* to 'username'@'%';
flush privileges;
```

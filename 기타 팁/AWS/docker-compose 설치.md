# docker-compose 설치  

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
기초 설치 끝

## 도커 컴포트 shm_size 설정
default로 설정된 용량이 작기 때문에 대부분 재설정이 필요하다
1. 메모리 용량 확인
```bash
# /dev/shm을 확인하면 된다.
df -h
```
![image](https://user-images.githubusercontent.com/71022555/151701485-0e96694a-f1c6-4b90-a65e-c995a5466ad0.png)
  
2. 실제 호스트 공간의 전체를 도커가 공유할 수 있도록 설정한다.
```bash
## docker-compose.yml 파일 생성
vi docker-compose.yml
```
3. 기타 설정
```bash
db:
  image: docker.io/mysql
  ports:
    - "3306:3306"
  environment:
    - MYSQL_ROOT_PASSWORD=password

# application container
app:
  image: docker.io/tomcat
  ports:
    - "8080:8080"

# web container
web:
  image: docker.io/nginx
  ports:
    - "80:80"
```

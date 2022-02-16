# 도커 개발 기록

🏀
## SpringBoot 도커로 만들기
1. SpringFile jar 파일 만들어주기  

![bootjar](https://user-images.githubusercontent.com/71022555/154320018-bf2ba058-6478-4736-bce3-e862e7df9511.png)  

성공시 build/libs 에 이름-0.0.1-SNAPSHOT.jar 파일이 만들어진다.  

AWS 에서 만들 시
```bash
# 프로젝트 루트폴더로 이동
chmod 700 gradlew
./gradlew clean build
```
2. Dockerfile 만들기
편한 위치에 Dockerfile 만들어 준다.(일반적으로 프로젝트 루트위치 만든다.)
```bash
FROM openjdk:11
ARG JAR_FILE=./Backend-0.0.1-SNAPSHOT.jar
copy ${JAR_FILE} honeyschool.jar
ENTRYPOINT ["java", "-jar", "honeyschool.jar"]
EXPOSE 9999

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

4. Docker 컨테이너 실행
```bash
docker run --name {원하는 컨테이너 이름} -d -p 9999:9999 {이미지 이름}

## -d 는 백그라운드에서 작동
## -p IN:OUT IN으로 들어오는 포트를 OUT 포트로 매핑해준다.
```

## 도커 이미지 허브에 업로드하기
### 예시
![dockerImage](https://user-images.githubusercontent.com/71022555/154321799-5d8aa7a7-8f1a-4117-aee4-5121383e39b6.png)  

- docker push {저장소이름:태그네임} 처럼 이미지를 바꾸어야 한다.
- 현재 도커이미지 이름을 yunghun97/test라고 가정
1. 이미지 이름 변경
```bash
docker tag yunghun97/test yunghun97/honeyschool_be:v1

## docker tage {이미지이름} {이미지이름:원하는태그이름}
```
2. 업로드하기
```bash
docker push yunghun97/honeyschool_be:v1
```
결과  😀

![업로드성공](https://user-images.githubusercontent.com/71022555/154322449-50453328-fac4-4306-b1f6-94f9ac9ba22b.png)  


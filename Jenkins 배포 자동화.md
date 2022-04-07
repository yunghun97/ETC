# 🚐 Jenkins로 배포 자동화하기!


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
docker stop tnt
docker rm tnt
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
# URL = 도메인/project/프로젝트명
# token = 발급받은 jenkins 프로젝트 토큰
```

## 4-2 배포 테스트
![image](https://user-images.githubusercontent.com/71022555/162250788-2f442112-a4ba-4478-b1f4-a64e71a9d714.png)  
  
## 4-3 200 확인
![image](https://user-images.githubusercontent.com/71022555/162250881-5a87e929-b30c-482b-b228-6ab8b8268d0d.png)
    
## 4-4 Jenkins 확인
![image](https://user-images.githubusercontent.com/71022555/162251895-1a88ebf4-6b3b-4360-b82e-205f0371ac55.png)
  
# 😀 끝
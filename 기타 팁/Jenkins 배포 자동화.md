# ๐ Jenkins๋ก ๋ฐฐํฌ ์๋ํํ๊ธฐ!

## 0. Credentials ์ค์ 
> Dashboard > Credentials > (global ํด๋ฆญ) > System > Global credentials > Add Credentials
  
![image](https://user-images.githubusercontent.com/71022555/162360781-53e682cc-83de-45d6-9c26-e4e73e9f080d.png)
  
- UserName : Gitlab ID ์์ฑ
- Password : GitLab Password ์์ฑ
- ID : Credentials์ ๊ตฌ๋ถํ๊ธฐ ์ํ uniqueํ id ๊ฐ
- Description : ํด๋น Credentials ์ค๋ช ์์ฑ


## 1. Git ์ฐ๋
> Git Plugin ์ค์น
> Gitlab Hook Plugin
> GitLab Plugin
## 2. Node.js ์ค์น
> Jenkins ๊ด๋ฆฌ -> Global Tool Configuration > Node.JS ํญ ์์ฑ

## 3. ํ๋ก์ ํธ ์์ฑ ๋ฐ ์์ฑ
> Freestyle project 
![image](https://user-images.githubusercontent.com/71022555/162229016-46d9a8bb-db28-46d7-b1e4-6ec76c3e1023.png)
  
- https ์ธ ๊ฒฝ์ฐ Credentals ์์ฑํด์ ํด๋น Credentials๋ก ์ธํ ํด์ค์ผ ํฉ๋๋ค.

## 3-1 ๋น๋ ์ ๋ฐ
![image](https://user-images.githubusercontent.com/71022555/162229189-629c652b-565e-4ceb-b78e-cb58848e4470.png)  
- Push Events์ผ ๋ ์ค์ 

## 3-2 ๋น๋ ์ ๋ฐ -> Secret token ๋ฐ๊ธ ๋ฐ๊ธฐ
> ๋น๋ ์ ๋ฐ ํญ์์ Secret token -> Generate ํตํด ํ ํฐ ์์ฑ -> gitlab์์ ์ฌ์ฉ
> ex) b356d45696925a2f09675c0ce64b0849
## 3-3 Build ์ค์ 
```bash
# Execute shell ๊ธฐ์ค
# ํ๋ก ํธ ๋ถ๋ถ
cd frontend
npm install
npm run build

# ๋ฐฑ์๋ ๋ถ๋ถ
cd ..
cd backend
chmod 755 gradlew
./gradlew clean build
docker stop tnt
docker rm tnt
docker build -t yunghun97/tnt .
docker run --name tnt -d -p 9999:9999 yunghun97/tnt

# DockerFile(๋ฐฑ์๋ ๋ฃจํธ ์์น์ ์ถ๊ฐ๋์ด ์๋ค๊ณ  ๊ฐ์ )
## Docker file ์์ค

#FROM openjdk:11
#ARG JAR_FILE=./build/libs/TNT_hadoop-0.0.1-SNAPSHOT.jar
#copy ${JAR_FILE} tnt.jar
#ENTRYPOINT ["java", "-jar", "tnt.jar"]
#EXPOSE 9990
```

## 4 Git Lab ์ค์ 
![image](https://user-images.githubusercontent.com/71022555/162230641-d5736cd4-50f1-47c0-ae10-68b20d5e889f.png)
  
> Setting -> Webhooks
## 4-1 WebHooks ์ค์ 
![image](https://user-images.githubusercontent.com/71022555/162230863-fbb9c142-a813-440a-af1d-732a89188086.png)  
```bash
# URL = ๋๋ฉ์ธ/project/ํ๋ก์ ํธ๋ช
# token = ๋ฐ๊ธ๋ฐ์ jenkins ํ๋ก์ ํธ ํ ํฐ
```

## 4-2 ๋ฐฐํฌ ํ์คํธ
![image](https://user-images.githubusercontent.com/71022555/162250788-2f442112-a4ba-4478-b1f4-a64e71a9d714.png)  
  
## 4-3 200 ํ์ธ
![image](https://user-images.githubusercontent.com/71022555/162250881-5a87e929-b30c-482b-b228-6ab8b8268d0d.png)
    
## 4-4 Jenkins ํ์ธ
![image](https://user-images.githubusercontent.com/71022555/162251895-1a88ebf4-6b3b-4360-b82e-205f0371ac55.png)
  
# ๐ ๋
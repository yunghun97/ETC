# SPRING 개발 기록

## SpringBoot 에 HTTPS 적용하기 SSL 사용  

1. ### Certbot에서 인증서 생성  
결과 /etc/letsencrypt/live 경로 밑에 도메인 이름으로 저장이 된다.
- privket.pen : 개인키
- fullchain.pem : 내 기본 공개키 + 기타 정보를 포함한 공개 키
- cert.pen : 기본 공개키
- chain.pem : 기타 정보를 포함한 공개키
2. PKCS12로 변환하기
```bash
openssl pkcs12 -export -in fullcain.pem -inkey privkey.pem -out 파일명.p12 --name ssafy -CAfile chain.pem -caname root

# export : PKCS#12 파일 생성
# in 개인키 파일 이름(p12에 들어갈 인증서)
# inkey : 포함시킬 개인 키
# out : 생성될 p12 파일명
# name Java에서 KeyStore로 접근시 alias항목이 되는 부분
# CAfile 인증서 발급 체인(CA 인증서 묶음)
```
3. 비밀번호 세팅
```bash
# 아래 비밀번호 입력창이 자동으로 나온다.
Enter Export Password:
Verifying - Enter Export Passwrod:
```
4. 해당 파일 Spring 부트 프로젝트에 이동
- src/main/resources 밑에 이동했다고 가정 (apllication.properties 위치)
```java
server.ssl.key-store=classpath:{파일명.p12}   // 2에서 -out 뒤에 설정한 파일명 ex) classpath:파일명.p12
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password={비밀번호}  // 3에서 설정한 비밀번호 ex) ssafy
```
😀 결과 springboot 실행시 자동으로 https로 실행된다.   
![springssl결과](https://user-images.githubusercontent.com/71022555/154324899-9c432295-4ead-41f6-b662-06c9ad4dc906.png)

## Springboot QueryDSL 사용시 build에러 해결
- 사용환경 Springboot
- gradle 사용
1. build.gradle 이동
2. 해당 소스 밑에 넣어주기 (컴파일하기전에 querydslDir 파일들을 다 지우겠다는 뜻)
```java
compileQuerydsl.doFirst { 
	if(file(querydslDir).exists() ) delete(file(querydslDir)) 
}
```
3. 다시 build 하면 해결! 😀

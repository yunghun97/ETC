# ๐ถ SPRING ๊ฐ๋ฐ ๊ธฐ๋ก

## ๐จโ๐ Spring Property ์ ๋ณด
> https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html
## SpringBoot ์ HTTPS ์ ์ฉํ๊ธฐ SSL ์ฌ์ฉ  

1. ### Certbot์์ ์ธ์ฆ์ ์์ฑ  
๊ฒฐ๊ณผ /etc/letsencrypt/live ๊ฒฝ๋ก ๋ฐ์ ๋๋ฉ์ธ ์ด๋ฆ์ผ๋ก ์ ์ฅ์ด ๋๋ค.
- privket.pen : ๊ฐ์ธํค
- fullchain.pem : ๋ด ๊ธฐ๋ณธ ๊ณต๊ฐํค + ๊ธฐํ ์ ๋ณด๋ฅผ ํฌํจํ ๊ณต๊ฐ ํค
- cert.pen : ๊ธฐ๋ณธ ๊ณต๊ฐํค
- chain.pem : ๊ธฐํ ์ ๋ณด๋ฅผ ํฌํจํ ๊ณต๊ฐํค
2. PKCS12๋ก ๋ณํํ๊ธฐ
```bash
openssl pkcs12 -export -in fullcain.pem -inkey privkey.pem -out ํ์ผ๋ช.p12 --name ssafy -CAfile chain.pem -caname root

# export : PKCS#12 ํ์ผ ์์ฑ
# in ๊ฐ์ธํค ํ์ผ ์ด๋ฆ(p12์ ๋ค์ด๊ฐ ์ธ์ฆ์)
# inkey : ํฌํจ์ํฌ ๊ฐ์ธ ํค
# out : ์์ฑ๋  p12 ํ์ผ๋ช
# name Java์์ KeyStore๋ก ์ ๊ทผ์ aliasํญ๋ชฉ์ด ๋๋ ๋ถ๋ถ
# CAfile ์ธ์ฆ์ ๋ฐ๊ธ ์ฒด์ธ(CA ์ธ์ฆ์ ๋ฌถ์)
```
3. ๋น๋ฐ๋ฒํธ ์ธํ
```bash
# ์๋ ๋น๋ฐ๋ฒํธ ์๋ ฅ์ฐฝ์ด ์๋์ผ๋ก ๋์จ๋ค.
Enter Export Password:
Verifying - Enter Export Passwrod:
```
4. ํด๋น ํ์ผ Spring ๋ถํธ ํ๋ก์ ํธ์ ์ด๋
- src/main/resources ๋ฐ์ ์ด๋ํ๋ค๊ณ  ๊ฐ์  (apllication.properties ์์น)
```java
server.ssl.key-store=classpath:{ํ์ผ๋ช.p12}   // 2์์ -out ๋ค์ ์ค์ ํ ํ์ผ๋ช ex) classpath:ํ์ผ๋ช.p12
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password={๋น๋ฐ๋ฒํธ}  // 3์์ ์ค์ ํ ๋น๋ฐ๋ฒํธ ex) ssafy
```
๐ ๊ฒฐ๊ณผ springboot ์คํ์ ์๋์ผ๋ก https๋ก ์คํ๋๋ค.   
![springssl๊ฒฐ๊ณผ](https://user-images.githubusercontent.com/71022555/154324899-9c432295-4ead-41f6-b662-06c9ad4dc906.png)

## Springboot QueryDSL ์ฌ์ฉ์ build์๋ฌ ํด๊ฒฐ
- ์ฌ์ฉํ๊ฒฝ Springboot
- gradle ์ฌ์ฉ
1. build.gradle ์ด๋
2. ํด๋น ์์ค ๋ฐ์ ๋ฃ์ด์ฃผ๊ธฐ (์ปดํ์ผํ๊ธฐ์ ์ querydslDir ํ์ผ๋ค์ ๋ค ์ง์ฐ๊ฒ ๋ค๋ ๋ป)
```java
compileQuerydsl.doFirst { 
	if(file(querydslDir).exists() ) delete(file(querydslDir)) 
}
```
3. ๋ค์ build ํ๋ฉด ํด๊ฒฐ! ๐

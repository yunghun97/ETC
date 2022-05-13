# 🧙‍♂️ Certboot HTTPS 적용

## 👨‍🎓 실행 환경
- AWS EC2
- Ubuntu 20.04 LTS

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

# '/snap/bin' is not included in the PATH enviroment variable 에러 발생시
# 임시 해결
export PATH=$PATH:/snap/bin
# 지속 해결
# /etc/environment 들어가서 /snap/bin 리스트에 넣기
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

### 프론트 Nginx SSL 키 인증 설정
```bash
cd /etc/nginx/sites-enabled

nano default

# 해당 부분 설정
server{
    ssl_certificate /etc/letsencrypt/live/{도메인}/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/{도메인}/privkey.pem;
}
```

### Spring HTTPS 설정
```java
server.ssl.key-store=classpath:ssafykey.p12 // src/main/resources의 경로에 ssafykey.p12가 존재해야 합니다.
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=ssafy
```
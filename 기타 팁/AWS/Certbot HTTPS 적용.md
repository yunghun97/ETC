# ๐งโโ๏ธ Certboot HTTPS ์ ์ฉ

## ๐จโ๐ ์คํ ํ๊ฒฝ
- AWS EC2
- Ubuntu 20.04 LTS

### Nginx ์ค์น
```bash
sudo apt install -y nginx
```
### Certbot ์ค์น
```bash
sudo snap install --classic certbot
```

### ์ธ์ฆ์๋ฐ๊ธ
```bash
sudo certbot --nginx -d {๋ฐ๊ธ๋ฐ์ ๋๋ฉ์ธ}

# ์ด๋ฉ์ผ ์๋ ฅ
# ์๋น์ค ์ฝ๊ด ๋์
# 1. No redirect, Redirect ์ค์ 

# '/snap/bin' is not included in the PATH enviroment variable ์๋ฌ ๋ฐ์์
# ์์ ํด๊ฒฐ
export PATH=$PATH:/snap/bin
# ์ง์ ํด๊ฒฐ
# /etc/environment ๋ค์ด๊ฐ์ /snap/bin ๋ฆฌ์คํธ์ ๋ฃ๊ธฐ
```
  
### SSL ์๋ ๊ฐฑ์ 
```bash
sudo certbot renew --dry-run

#Congraturation ๋จ๋ฉด ์ ์ฉ ๋๊ฒ

# ์๋ ๊ฐฑ์  -> sudo certbot renew
```

### ํค ํ์ธ
```bash
cd etc/letsencrypt/live/{๋๋ฉ์ธ}
```

### pem ์ธ์ฆ์ pkcs12๋ก ๋ณํ
```bash
cd etc/letsencrypt/live/{๋๋ฉ์ธ}

# openssl pkcs12 -export -inkey {privkey} -in {certificate} -out {pkcs12 ํค ์ด๋ฆ}
openssl pkcs12 -export -inkey privkey.pem -in cert.pem -out ssafykey.p12
# ์ดํ ๋น๋ฐ๋ฒํธ ์ค์ ํ์๋ฉด ๋ฉ๋๋ค.
```

### ํ๋ก ํธ Nginx SSL ํค ์ธ์ฆ ์ค์ 
```bash
cd /etc/nginx/sites-enabled

nano default

# ํด๋น ๋ถ๋ถ ์ค์ 
server{
    ssl_certificate /etc/letsencrypt/live/{๋๋ฉ์ธ}/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/{๋๋ฉ์ธ}/privkey.pem;
}
```

### Spring HTTPS ์ค์ 
```java
server.ssl.key-store=classpath:ssafykey.p12 // src/main/resources์ ๊ฒฝ๋ก์ ssafykey.p12๊ฐ ์กด์ฌํด์ผ ํฉ๋๋ค.
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=ssafy
```
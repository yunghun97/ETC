# ๐ช Nginx ์ค์ 

## DDOS ๋๋น

### Request ์์ฒญ ๋น์จ ์ ํ(limit_req)
```bash
limit_req_zone $binary_remote_addr zone=ddos_req:10m rate=10r/s;
# Here, the states are kept in a 10 megabyte zone โoneโ, and an average request processing rate for this zone cannot exceed 10 request per second.
server{
    location /{
        limit_req zone=ddos_req;
    }
}
```
### ์ฐ๊ฒฐ ์ ์ ํ
```bash
limit_conn_zone $binary_remote_addr zone=ddos_conn:10m;
# ํ๋์ ํด๋ผ์ด์ธํธ ip๋น ๋์์ ์ต๋ 10๊ฐ์ request๋ง ์ฒ๋ฆฌ๋๊ฒ ํด์ค๋ค.
# keeps the result of limiting the number of connections 
server{
    location /{
        limit_con ddos_conn 10;
    }
}
```
### ๋๋ฆฐ ์ฐ๊ฒฐ ๋ซ๊ธฐ
๋ฐ์ดํฐ๋ฅผ ๋ฐ๋ฅด๊ฒ ์๋ตํ์ง ์๋ ์ฐ๊ฒฐ์ ๋ซ๋๋ค.
```bash
server{
    client_body_timeout 5s;
    client_header_timeout 5s;
}
```

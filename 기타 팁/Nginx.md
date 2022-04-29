# 💪 Nginx 설정

## DDOS 대비

### Request 요청 비율 제한(limit_req)
```bash
limit_req_zone $binary_remote_addr zone=ddos_req:10m rate=10r/s;
# Here, the states are kept in a 10 megabyte zone “one”, and an average request processing rate for this zone cannot exceed 10 request per second.
server{
    location /{
        limit_req zone=ddos_req;
    }
}
```
### 연결 수 제한
```bash
limit_conn_zone $binary_remote_addr zone=ddos_conn:10m;
# 하나의 클라이언트 ip당 동시에 최대 10개의 request만 처리되게 해준다.
# keeps the result of limiting the number of connections 
server{
    location /{
        limit_con ddos_conn 10;
    }
}
```
### 느린 연결 닫기
데이터를 바르게 응답하지 않는 연결을 닫는다.
```bash
server{
    client_body_timeout 5s;
    client_header_timeout 5s;
}
```

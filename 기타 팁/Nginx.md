# 💪 Nginx 설정

## DDOS 대비

### Request 요청 비율 제한(limit_req)
```bash
limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;
# Here, the states are kept in a 10 megabyte zone “one”, and an average request processing rate for this zone cannot exceed 10 request per second.
server{
    location /{
        limit_req zone=zone_one;
    }
}
```
### 연결 수 제한
```bash
limit_conn_zone $variable zone=name:size;
# limit_conn_zone $variable zone=name:10m;
# keeps the result of limiting the number of connections 
server{
    location /{
        limit_con zone_addr 10;
    }
}
```

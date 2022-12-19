## 🎒 개발 환경

**CentOS 7.6**

 

### 1. vi 편집기 열기
```bash
vi /etc/nginx/conf.d/default.conf
```

### 2. 이미지 경로 매핑

```bash
location /images/ {
        alias /var/jenkins/workspace/manual/frontend/public/images/;
}

# /images/test.png 로 호출 시 경로를 alias로 설정한 위치에서 이미지 탐색 진행
```

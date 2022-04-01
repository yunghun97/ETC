1. 사용중인 포트와 PID 확인
```bash
netstat -a -o
```
  
 2. pid로 포트 죽이기
 ```bash
 taskkill /f /pid 포트번호
 ```


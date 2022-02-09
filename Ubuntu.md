

## 포트 죽이기.
1. 특정 포트 확인  
$ netstat -nap | grep port
```bash
netstat -nap | grep 80
```
  
2. 특정 포트에서 사용하는 프로그램 확인  
$ lsof -i TCP:port
```bash
lsof -i TCP:80
```
  
3. 특정 포트를 사용하는 프로그램 죽이기  
$ fuser -k -n tcp port
```bash
fuser -k -n tcp 80
```

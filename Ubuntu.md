

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

## 🏳‍🌈 압축 하기
### tar 압축
```bash
tar -cvf {압축파일명.tar} {대상 파일}
# tar -cvf test.tar a
```

### tar.gz 압축
```bash
tar -zcvf {압축파일명.tar.gz} {대상 파일}
# tar -zcvf test.tar a
```
  
### zip 압축
```bash
zip {압축파일명.zip} {대상 파일}
#  zip test.zip ./*    // 현재 디렉토리를 test.zip으로 압축
#  zip test.zip -r ./*  // 현재 디렉토리 및 하위 디렉토리까지 모두 압축
```
## 🏴 압축 풀기
### tar 압축 풀기
```bash
tar -xvf {압축파일명.tar}
# tar -xvf test.tar
```

### tar.gz 압축 풀기
```bash
tar -zxvf {압축파일명.tar.gz}
# tar -zxvf test.tar.gz
```
  
### zip 압축 풀기
```bash
unzip {압출파일명.zip}
# unzip test.zip
# unzip test.zip -d {압축 풀 위치}
```
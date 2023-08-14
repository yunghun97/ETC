# 🏐 갤럭시 워치 페이스 스튜디오 사용 방법

## 1. 워치 페이스 스튜디오 다운로드
(링크)[https://developer.samsung.com/watch-face-studio/ko/download.html]


## 2. adb 다운로드

### Mac OS 기준
```brew
brew install --cask android-platform-tools
```
- 설치 후 PC 재부팅 진행

### 3. 갤럭시 워치 개발자 옵션 활성화
1. adb 디버깅 활성화
2. 무선 디버깅 활성화
3. 새기기 등록

### 4. adb 갤럭시 워치 연결
```
adb pair HOST[:PORT] [PAIRING CODE]
Successfully paired to 0.0.0.0:12345 [guid=adb-asdsadasdasd]
```

### 5. 갤럭시 워치 스튜디오에서 워치 연결

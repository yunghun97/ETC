# 🐸 Apache Nutch 설치 및 사용법

## 🪐 개발 환경
- Ubuntu 20.04 LTS
- Nutch 1.18  
[다운링크](https://nutch.apache.org/download/)

### 0. JAVA HOME 설정
```bash
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
```
### 1. 다운받기
```bash
wget https://dlcdn.apache.org/nutch/1.18/apache-nutch-1.18-bin.tar.gz
```
  
### 2. 압축 풀기
```bash
tar -zxvf apache-nutch-1.18-bin.tar.gz
```
  
### 3. data 폴더, seed 폴더, 파일 만들기
```bash
mkdir data
cd data
mkdir seed
vi seed.txt
```

### 4. URL 설정
```
vi seed.txt

# URL 입력
https://nutch.apache.org/index.html
```

### 5. 크롤링 하기 위한 최소 설정
```bash
cd conf
vi nutch-site.xml

# Property 추가
<property>
	  <name>http.agent.name</name>
	  <value>test-crawler</value>
	  <description>HTTP 'User-Agent' request header. MUST NOT be empty - 
	  please set this to a single word uniquely related to your organization.

	  NOTE: You should also check other related properties:

	    http.robots.agents
	    http.agent.description
	    http.agent.url
	    http.agent.email
	    http.agent.version

	  and set their values appropriately.

	  </description>
</property>

# 필터 설정은 vi conf 에서 urlfilter들을 수정하면 됩니다.
```

### 6. 크롤링 명령어 확인 및 크롤링 실행
```bash
# nutch 루트 디렉토리로 먼저 이동
bin/crawl

bin/crawl -s {seed_dir} {crawl_dir} {num_rounds}
# bin/crawl -s data/seed data/crawl 3

# bin/crawl -s data/seed --size-fetchlist 500 data/crawl 1
```

### 7. 데이터 변환
기존의 데이터는 알아보기가 어렵다 이를 보기 편하게 변경
```bash
# 명령어 확인 -> bin/nutch
bin/nutch dump -outputDir {출력 폴더} -segment {segment 폴더}
# bin/nutch dump -outputDir data/output -segment data/crawl/segments/
```
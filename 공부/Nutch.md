# ๐ธ Apache Nutch ์ค์น ๋ฐ ์ฌ์ฉ๋ฒ

## ๐ช ๊ฐ๋ฐ ํ๊ฒฝ
- Ubuntu 20.04 LTS
- Nutch 1.18  
[๋ค์ด๋งํฌ](https://nutch.apache.org/download/)

### 0. JAVA HOME ์ค์ 
```bash
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
```
### 1. ๋ค์ด๋ฐ๊ธฐ
```bash
wget https://dlcdn.apache.org/nutch/1.18/apache-nutch-1.18-bin.tar.gz
```
  
### 2. ์์ถ ํ๊ธฐ
```bash
tar -zxvf apache-nutch-1.18-bin.tar.gz
```
  
### 3. data ํด๋, seed ํด๋, ํ์ผ ๋ง๋ค๊ธฐ
```bash
mkdir data
cd data
mkdir seed
vi seed.txt
```

### 4. URL ์ค์ 
```
vi seed.txt

# URL ์๋ ฅ
https://nutch.apache.org/index.html
```

### 5. ํฌ๋กค๋ง ํ๊ธฐ ์ํ ์ต์ ์ค์ 
```bash
cd conf
vi nutch-site.xml

# Property ์ถ๊ฐ
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

# ํํฐ ์ค์ ์ vi conf ์์ urlfilter๋ค์ ์์ ํ๋ฉด ๋ฉ๋๋ค.
```

### 6. ํฌ๋กค๋ง ๋ช๋ น์ด ํ์ธ ๋ฐ ํฌ๋กค๋ง ์คํ
```bash
# nutch ๋ฃจํธ ๋๋ ํ ๋ฆฌ๋ก ๋จผ์  ์ด๋
bin/crawl

bin/crawl -s {seed_dir} {crawl_dir} {num_rounds}
# bin/crawl -s data/seed data/crawl 3

# bin/crawl -s data/seed --size-fetchlist 500 data/crawl 1
```

### 7. ๋ฐ์ดํฐ ๋ณํ
๊ธฐ์กด์ ๋ฐ์ดํฐ๋ ์์๋ณด๊ธฐ๊ฐ ์ด๋ ต๋ค ์ด๋ฅผ ๋ณด๊ธฐ ํธํ๊ฒ ๋ณ๊ฒฝ
```bash
# ๋ช๋ น์ด ํ์ธ -> bin/nutch
bin/nutch dump -outputDir {์ถ๋ ฅ ํด๋} -segment {segment ํด๋}
# bin/nutch dump -outputDir data/output -segment data/crawl/segments/
```
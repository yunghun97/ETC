# ๐ณ Redis Spring Boot ์ฌ์ฉํ๊ธฐ

## ๊ฐ๋ฐ ํ๊ฒฝ
- Spring Boot
- Gradle
- Lettuce ์ฌ์ฉ

## ์ฐธ๊ณ  ๋ฌธ์
> https://docs.spring.io/spring-data/redis/docs/current/reference/html/
### 1. spring redis ์ถ๊ฐํ๊ธฐ
- application.properties
```bash
#Redis
spring.redis.host=์ฃผ์
spring.redis.port=ํฌํธ๋ฒํธ
spring.redis.password=๋น๋ฐ๋ฒํธ(์ค์  ์)
```
  
### 2. Redis Config ํ์ผ ๋ง๋ค๊ธฐ
```java
@Configuration
@EnableRedisRepositories
public class RedisConfig {
	@Value("${spring.redis.host}")
	private String host;
	@Value("${spring.redis.port}")
	private int port;
	@Value("${spring.redis.password}")
	private String password;
	
	@Bean
	public RedisConnectionFactory redisConnectionFactory() {
		RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration();
		redisStandaloneConfiguration.setHostName(host);
		redisStandaloneConfiguration.setPort(port);
		redisStandaloneConfiguration.setPassword(password);
		return new LettuceConnectionFactory(redisStandaloneConfiguration);
	}
	
	@Bean
	public RedisTemplate<?, ?> redisTemplate(){
		RedisTemplate<byte[], byte[]> template = new RedisTemplate<>();
		template.setConnectionFactory(redisConnectionFactory());
		return template;
	}
	
}

/**
@Configuration
์ค์  ํด๋์ค๋ก ์ฌ์ฉ

@EnableRedisRepositories
RedisRepository ๋ฅผ ์ฌ์ฉํ๋ค๊ณ  ์ง์ 

@PropertySource(value = "application.properties") -> ์ถ๊ฐ ์ํด๋ ์๊ด์๋ค.
ํ๋กํผํฐ ์ค์  ํ์ผ์ application.properties ๋ก ์ฌ์ฉ
*/
```
  
### 3. ํ์คํธ์ฉ ํด๋์ค ๋ง๋ค๊ธฐ
```java
@RedisHash("news")
@Getter
@Setter
public class News {
	@Id
	private String id;
	private String title;
	private String repoter;
	private String content;
	public News(String id, String title, String repoter, String content) {
		super();
		this.id = id;
		this.title = title;
		this.repoter = repoter;
		this.content = content;
	}
}
/**
@RedisHash๊ฐ jpa์์ entity์ ํด๋นํ๋ ์ด๋ธํ์ด์์ด๊ณ  ์ด๋ธํ์ด์์ ๋ค์ด๊ฐ๋ value์ธ "news"๋ @Id๊ฐ๊ณผ ๊ฒฐํฉํด์ key๋ฅผ ์์ฑํ  ๋ ์ฌ์ฉํ๋ ๊ฐ์ด๋ค.

์ถ์ฒ: https://anomie7.tistory.com/42 [๋ง์ง๋ง์ ๋ง์ง๋ง๊น์ง ๋คํ๋ ์ต์ ]
*/
```
### 4. Repository ๋ง๋ค๊ธฐ
```java
public interface NewsRedisRepository extends CrudRepository<News, String> {

}
``` 
  
### 5. ํ์คํธ ์ฝ๋ ๋ง๋ค๊ณ  ๋๋ฆฌ๊ธฐ
```java
@Service
public class RedisImpl implements Redis{
	@Autowired
	NewsRedisRepository newsRedisRepository;
	
	@Override
	public News RedisTest() {
		News news =new News(null, "๊ธฐ์ฌ์ ๋ชฉ", "๊ถ์ํ", "redis ํ์คํธ์๋๋ค."); 
		newsRedisRepository.save(news);
		return news;
	}

}
```

### 6. redis์์ ๊ฒฐ๊ณผ ํ์ธํ๊ธฐ
```bash
# ๋น๋ฒ ์์ ๋
ssafy ํนํ:0> AUTH ๋น๋ฐ๋ฒํธ
"OK"

# ๋ชจ๋  key๊ฐ ๋ณด๊ธฐ
ssafy ํนํ:0> keys *
1)  "news"
2)  "news:591fab7e-ee6a-44fa-9408-87057ec4c19e"
3)  "news:64449476-477e-4953-aff8-8ab7f3736cc6"

# ํด๋น ํค VALUE ๋ณด๊ธฐ
ssafy ํนํ:0>hgetall news:591fab7e-ee6a-44fa-9408-87057ec4c19e
1)  "_class"
2)  "com.ssafy.tnt.entity.News"
3)  "content"
4)  "redis ํ์คํธ์๋๋ค."
5)  "id"
6)  "591fab7e-ee6a-44fa-9408-87057ec4c19e"
7)  "repoter"
8)  "๊ถ์ํ"
9)  "title"
10)  "๊ธฐ์ฌ์ ๋ชฉ"
```

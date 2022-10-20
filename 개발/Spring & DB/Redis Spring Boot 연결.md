# 🍳 Redis Spring Boot 사용하기

## 개발 환경
- Spring Boot
- Gradle
- Lettuce 사용

## 참고 문서
> https://docs.spring.io/spring-data/redis/docs/current/reference/html/
### 1. spring redis 추가하기
- application.properties
```bash
#Redis
spring.redis.host=주소
spring.redis.port=포트번호
spring.redis.password=비밀번호(설정 시)
```
  
### 2. Redis Config 파일 만들기
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
설정 클래스로 사용

@EnableRedisRepositories
RedisRepository 를 사용한다고 지정

@PropertySource(value = "application.properties") -> 추가 안해도 상관없다.
프로퍼티 설정 파일을 application.properties 로 사용
*/
```
  
### 3. 테스트용 클래스 만들기
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
@RedisHash가 jpa에서 entity에 해당하는 어노테이션이고 어노테이션에 들어가는 value인 "news"는 @Id값과 결합해서 key를 생성할 때 사용하는 값이다.

출처: https://anomie7.tistory.com/42 [마지막의 마지막까지 다하는 최선]
*/
```
### 4. Repository 만들기
```java
public interface NewsRedisRepository extends CrudRepository<News, String> {

}
``` 
  
### 5. 테스트 코드 만들고 돌리기
```java
@Service
public class RedisImpl implements Redis{
	@Autowired
	NewsRedisRepository newsRedisRepository;
	
	@Override
	public News RedisTest() {
		News news =new News(null, "기사제목", "권영현", "redis 테스트입니다."); 
		newsRedisRepository.save(news);
		return news;
	}

}
```

### 6. redis에서 결과 확인하기
```bash
# 비번 있을 때
ssafy 특화:0> AUTH 비밀번호
"OK"

# 모든 key값 보기
ssafy 특화:0> keys *
1)  "news"
2)  "news:591fab7e-ee6a-44fa-9408-87057ec4c19e"
3)  "news:64449476-477e-4953-aff8-8ab7f3736cc6"

# 해당 키 VALUE 보기
ssafy 특화:0>hgetall news:591fab7e-ee6a-44fa-9408-87057ec4c19e
1)  "_class"
2)  "com.ssafy.tnt.entity.News"
3)  "content"
4)  "redis 테스트입니다."
5)  "id"
6)  "591fab7e-ee6a-44fa-9408-87057ec4c19e"
7)  "repoter"
8)  "권영현"
9)  "title"
10)  "기사제목"
```

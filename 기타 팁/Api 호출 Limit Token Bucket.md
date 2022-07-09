# 🌭 Token Bucket
  

## Bucket4j 사용
  
### build.gradle
```java
// https://mvnrepository.com/artifact/com.github.vladimir-bukhtoyarov/bucket4j-core
implementation 'com.github.vladimir-bukhtoyarov:bucket4j-core:7.5.0'
```

## 인터셉터 설정
```java
@Configuration
@RequiredArgsConstructor
public class WebMvcConfig implements WebMvcConfigurer{
    
    private final ApiLimitInterceptor apiLimitInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry){
        registry.addInterceptor(apiLimitInterceptor)
            .order(0) // 우선순위 설정
            .addPathPatterns("/api/**");
    }
}
```

## Bucket 설정
```java
@Component
public class ApiLimitInterceptor implements HandlerInterceptor {
    private static Bucket bucket;

    public ApiLimitInterceptor(){
        // Buk
        Refill refill = Refill.intervally(1, Duration.ofSeconds(10));
        // 해당 Band의 총 크기
        // 총 크기가 limit이며 refill 주기를 갖는 Bucket
        Bandwidth limit = Bandwidth.classic(3, refill);
        bucket = Bucket.builder().addLimit(limit).build();
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws IOException { 
        if(bucket.tryConsume(1)){ // token 1개 제거 -> 성공 = true 실패 = false return 함
            return true;
        }else{ // Bucket에 토큰이 없는 경우
            response.sendError(429, "현재 사용자가 너무 많습니다. 잠시 후 다시 시도해주세요");
            return false; 
        }
    }
}

```
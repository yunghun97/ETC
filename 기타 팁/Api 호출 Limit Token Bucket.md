# ๐ญ Token Bucket
  

## Bucket4j ์ฌ์ฉ
  
### build.gradle
```java
// https://mvnrepository.com/artifact/com.github.vladimir-bukhtoyarov/bucket4j-core
implementation 'com.github.vladimir-bukhtoyarov:bucket4j-core:7.5.0'
```

## ์ธํฐ์ํฐ ์ค์ 
```java
@Configuration
@RequiredArgsConstructor
public class WebMvcConfig implements WebMvcConfigurer{
    
    private final ApiLimitInterceptor apiLimitInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry){
        registry.addInterceptor(apiLimitInterceptor)
            .order(0) // ์ฐ์ ์์ ์ค์ 
            .addPathPatterns("/api/**");
    }
}
```

## Bucket ์ค์ 
```java
@Component
public class ApiLimitInterceptor implements HandlerInterceptor {
    private static Bucket bucket;

    public ApiLimitInterceptor(){
        // Buk
        Refill refill = Refill.intervally(1, Duration.ofSeconds(10));
        // ํด๋น Band์ ์ด ํฌ๊ธฐ
        // ์ด ํฌ๊ธฐ๊ฐ limit์ด๋ฉฐ refill ์ฃผ๊ธฐ๋ฅผ ๊ฐ๋ Bucket
        Bandwidth limit = Bandwidth.classic(3, refill);
        bucket = Bucket.builder().addLimit(limit).build();
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws IOException { 
        if(bucket.tryConsume(1)){ // token 1๊ฐ ์ ๊ฑฐ -> ์ฑ๊ณต = true ์คํจ = false return ํจ
            return true;
        }else{ // Bucket์ ํ ํฐ์ด ์๋ ๊ฒฝ์ฐ
            response.sendError(429, "ํ์ฌ ์ฌ์ฉ์๊ฐ ๋๋ฌด ๋ง์ต๋๋ค. ์ ์ ํ ๋ค์ ์๋ํด์ฃผ์ธ์");
            return false; 
        }
    }
}

```
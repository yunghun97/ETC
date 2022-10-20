# 🎫 Spring boot Test Code

## 개발 환경
- Spring Boot
- Junit 5

### 테스트 코드

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // 모든 Bean 로딩, WebEnviroment.RANDOM_PORT = 테스트의 웹 환경을 설정 Spring boot의 내장 서버를 랜덤으로 띄움 
class TokenUserControllerTest { 

    @Autowired
    private TestRestTemplate restTemplate;

    private static Map<String, String> map = new HashMap<>();

    @Test // Text 할 메소드에 작성
    void serverTokenCheck() throws InterruptedException {
    // 현재 Bucket 크기 1분에 20,  10초에 10개
        // 최초 Bucket 크기 = 10
    // 충전 주기 : 10초에 10개 OR 1분에 60개
        // limit : 10초에 1개, 1분에 2개
        map.put("userId", "admin");
        map.put("password", "password1!");

        System.out.println("-------토큰 비우기-------");
        sendRequest(11);
        System.out.println("-----대기--------");
        Thread.sleep(10000);
        sendRequest(11); // 에러 발생- >

        Thread.sleep(10000);
        sendRequest(5); // 1분에 20개이므로 에러

        Thread.sleep(40000); // 40초 대기
        sendRequest(11);
        Thread.sleep(10000);
        sendRequest(15); // 1분에 20개이므로 10개 정상 5개 다 에러
        Thread.sleep(10000);
        sendRequest(5); // 1분에 20개이므로 5개 다 에러
    }

    void sendRequest(int num) {
        System.out.println("-----------"+num+" 토큰 소모----------");
        for (int i = 0; i < num; i++) {
            System.out.println(restTemplate.postForEntity("/api/user/login", map, Object.class));
        }
    }
```

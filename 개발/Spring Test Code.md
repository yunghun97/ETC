# ð« Spring boot Test Code

## ê°ë° íê²½
- Spring Boot
- Junit 5

### íì¤í¸ ì½ë

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // ëª¨ë  Bean ë¡ë©, WebEnviroment.RANDOM_PORT = íì¤í¸ì ì¹ íê²½ì ì¤ì  Spring bootì ë´ì¥ ìë²ë¥¼ ëë¤ì¼ë¡ ëì 
class TokenUserControllerTest { 

    @Autowired
    private TestRestTemplate restTemplate;

    private static Map<String, String> map = new HashMap<>();

    @Test // Text í  ë©ìëì ìì±
    void serverTokenCheck() throws InterruptedException {
    // íì¬ Bucket í¬ê¸° 1ë¶ì 20,  10ì´ì 10ê°
        // ìµì´ Bucket í¬ê¸° = 10
    // ì¶©ì  ì£¼ê¸° : 10ì´ì 10ê° OR 1ë¶ì 60ê°
        // limit : 10ì´ì 1ê°, 1ë¶ì 2ê°
        map.put("userId", "admin");
        map.put("password", "password1!");

        System.out.println("-------í í° ë¹ì°ê¸°-------");
        sendRequest(11);
        System.out.println("-----ëê¸°--------");
        Thread.sleep(10000);
        sendRequest(11); // ìë¬ ë°ì- >

        Thread.sleep(10000);
        sendRequest(5); // 1ë¶ì 20ê°ì´ë¯ë¡ ìë¬

        Thread.sleep(40000); // 40ì´ ëê¸°
        sendRequest(11);
        Thread.sleep(10000);
        sendRequest(15); // 1ë¶ì 20ê°ì´ë¯ë¡ 10ê° ì ì 5ê° ë¤ ìë¬
        Thread.sleep(10000);
        sendRequest(5); // 1ë¶ì 20ê°ì´ë¯ë¡ 5ê° ë¤ ìë¬
    }

    void sendRequest(int num) {
        System.out.println("-----------"+num+" í í° ìëª¨----------");
        for (int i = 0; i < num; i++) {
            System.out.println(restTemplate.postForEntity("/api/user/login", map, Object.class));
        }
    }
```

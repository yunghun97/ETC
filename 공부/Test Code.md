# 🐧 Test Code  

### 개발 환경
- Spring Boot
- Gradle
- MacOS
- Intellij

## 🐥 Junit5

- Junit Jupiter
  - TestEngine API 구현체 - Junit5를 구현
- Junit Platform
  - test를 실행하기 위한 뼈대
  - 각종 IDE를 연동을 보조하는 역할
- Junit Vintage
  - TestEngine 구현체 JUnit,3,4 구현
  - Junit 3,4 버전의 코드를 실행

### 1. 사용 설정
- Gradle 사용 시 자동으로 설정되어있음.
```bash
tasks.named('test') {
    useJUnitPlatform()
}
```

### 2. 테스트 코드 파일 추가
```
Command + Shift + T
```

### 3. Junit LifeCycle 어노테이션

|Annotation|Desc|
|:--|:--|
|@Test|테스트용 메소드를 표현|
|@BeforeEach|각 테스트 메소드 시작 전 실행되는 메소드 표현|
|@AfterEach|각 테스트 메소드가 시작된 후 실행되는 메소드 표현|
|@BeforeAll|테스트 시작 전 실행되어야 하는 메소드 표현(Static 처리)|
|@AfterAll|테스트 종료 후에 실행되어야 하는 메소드를 표현|

### Junit Main Annotation

**@SpringBootTest**
- 통합 테스트 용도
- SpringBootApplication의 모든 하위 Bean을 스캔하고 로드함
- 그 후 Test용 Applicatino Context를 만들어 Bean 추가, MockBean을 찾아 교체

**@ExtendWith**
- Junit4의 @RunWith 어노테이션
- @ExtendWith는 메인으로 실행될 Class 지정 가능
- @SpringBootTest는 기본적으로 @ExtendWith가 추가되어 있습니다.

**@WebMvcTest(Class명.class)**
- ()안에 작성된 클래스만 실제 로드하여 테스트 진행
- 매개변수를 지정하지 않으면 @Controoler, @RestController, @RestControllerAdivce 등 컨트롤러와 연관된 Bean이 모드 로드
- Spring의 모든 Bean을 로드가 필요없을 때 사용

**@Autowired about Mockbean**
- Controller의 API를 테스트하는 용도인 MockMvc 객체를 주입 받음
- Perform()메소드를 활용하여 컨트롤러의 동작을 확인할 수 있음
- andExpect(), andDo(), andReturn() 등의 메소드를 같이 활용함

**@MockBean**
- 테스트할 클래스에서 주입 받고 있는 객체에 대해 가짜 객체를 생성해주는 어노테이션
- 해당 객체는 실제 행위를 하지 않음
- given() 메소드를 활용하여 가짜 객체의 동작에 대해 정의하여 사용할 수 있음

**@AutoConfigureMockMvc**
- spring.test.mockmvc의 설정을 로드하면서 MockMvc의 의존성을 자동으로 주입
- MockMvc 클래스는 REST API 테스트를 할 수 있는 클래스
 
 
**@Import**
- 필요한 Class들을 Configuration으로 만들어 사용할 수 있음
- Configuration Component 클래스도 의존성 설정할 수 있음
- Import된 클래스는 주입으로 사용 가능

### Get, Post 테스트 코드
```java
    @Test
    @DisplayName("카테고리 Insert")
    void insertCategory() throws Exception {
        Category category = new Category("key","테스트", 9, null);

        mockMvc.perform(
                MockMvcRequestBuilders.post("/api/category/insert")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(category))
                        )
                .andExpect(status().isOk())
                .andDo(print());
    }

    @Test
    @DisplayName("특정 카테고리 가져오기")
    void getCategory() throws Exception {
        mockMvc.perform(
                MockMvcRequestBuilders
                        .get("//api/category/select")
                        .param("id", "category1")
        ).andExpect(status().isOk())
                .andDo(print());
    }
```

### 테스트 코드 실행 순서
```java
// 명시한 순서대로 진행
@TestMethodOrder(MethodOrderer.OrderAnnotation.class) // 
class CategoryControllerTest {

}

@Order(n) // n이 작은거부터 실행
```


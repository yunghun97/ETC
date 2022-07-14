# 🍕 Swagger

- Spring boot


### build.gradle

```
implementation group: 'io.springfox', name: 'springfox-swagger2', version: '3.0.0'
implementation group: 'io.springfox', name: 'springfox-swagger-ui', version: '3.0.0'
```

### Swagger.config
```java
@Configuration
@EnableSwagger2
// http://localhost:9999/swagger-ui/index.html
public class SwaggerConfig implements WebMvcConfigurer {

    @Bean
    public Docket api(){
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.test"))
                .paths(PathSelectors.ant("/**")) // swagger 적용할 Request 설정
                .build()
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("제목")
                .description("<h2>프로젝트 설명</h2>")
                .contact(new Contact("yunghun97", "https://github.com/yunghun97", "yunghun97@naver.com"))
                .version("1.0")
                .build();
    }
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/swagger-ui/**").addResourceLocations("classpath:/META-INF/resources/webjars/springfox-swagger-ui/");
    }
}
```

# 📩 Spring Boot 기본 메일 보내기

## 1. builder.gradle
```java
// https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-mail
implementation 'org.springframework.boot:spring-boot-starter-mail:2.7.1'
```

## 2. applcation.yml 설정
```java
spring:
  mail:
    host: smtp.naver.com
    port: 465
    username: 아이디@naver.com
    password: 비밀번호
    properties:
      mail.smtp.auth: true
      mail.smtp.ssl.enable: true
```

## 3. 소스 

```java
public class MailService {
    private final JavaMailSender mailSender;

    public void sendMail() throws MessagingException {

        SimpleMailMessage simpleMailMessage = new SimpleMailMessage();
        simpleMailMessage.setFrom("yunghun97@naver.com"); // 발신자
        simpleMailMessage.setTo("yunghun97@naver.com"); // 수신자
        simpleMailMessage.setSubject("테스트 메일"); // 제목
        simpleMailMessage.setText("내용~~"); // 내용

        mailSender.send(simpleMailMessage);
    }
}
```

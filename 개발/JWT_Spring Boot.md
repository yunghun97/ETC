# 🥚 JWT Spring Boot

## 🥫 gradle
```yml
// https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-api
implementation 'io.jsonwebtoken:jjwt-api:0.11.5'
implementation 'io.jsonwebtoken:jjwt-impl:0.11.5'
implementation 'io.jsonwebtoken:jjwt-jackson:0.11.5'
```

## 🍞 Java Source

### 토큰 생성
```java
private String SECRET_KEY = "aisdjasidjaiosjdosdiovddsojpxijozsijodeijow"; // 암호화하는 비밀키
public String createToken(String id, String role){
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        byte[] secretKeyBytes = DatatypeConverter.parseBase64Binary(SECRET_KEY);
        Key signinKey = new SecretKeySpec(secretKeyBytes, signatureAlgorithm.getJcaName());
        Map<String, String> map = new HashMap<>();
        map.put("role", role);
        map.put("id", id);
        return Jwts.builder().setSubject(id).setClaims(map).signWith(signinKey, signatureAlgorithm).compact();
}
```

### 토큰 정보 가져오기
```java
public String getSubject(String token){
    Claims claims = Jwts.parserBuilder()
          .setSigningKey(DatatypeConverter.parseBase64Binary(SECRET_KEY))
          .build()
          .parseClaimsJws(token)
          .getBody();

    return claims.toString();
}
```

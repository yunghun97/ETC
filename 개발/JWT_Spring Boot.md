# π₯ JWT Spring Boot

## π₯« gradle
```yml
// https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-api
implementation 'io.jsonwebtoken:jjwt-api:0.11.5'
implementation 'io.jsonwebtoken:jjwt-impl:0.11.5'
implementation 'io.jsonwebtoken:jjwt-jackson:0.11.5'
```

## π Java Source

### ν ν° μμ±
```java
private String SECRET_KEY = "aisdjasidjaiosjdosdiovddsojpxijozsijodeijow"; // μνΈννλ λΉλ°ν€
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

### ν ν° μ λ³΄ κ°μ Έμ€κΈ°
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

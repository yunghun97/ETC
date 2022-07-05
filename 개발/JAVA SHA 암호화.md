# ⚽ JAVA SHA 암호화

## 😀 사용 이유
- Password를 DB에 평문으로 저장 시 보안문제 발생 가능

## 😄 소스
```java
public class SHA256Encrypt {

    public static String encryptionPassword(String password){
        try {
            /* MessageDigest 클래스의 getInstance() 메소드의 매개변수에 "SHA-256" 알고리즘 이름을 지정함으로써 
				    해당 알고리즘에서 해시값을 계산하는 MessageDigest를 구할 수 있다 */
            MessageDigest messageDigest = MessageDigest.getInstance("SHA-256");
            // 바이트 배열로 해쉬를 반환한다.
            byte[] hash = messageDigest.digest(password.getBytes(StandardCharsets.UTF_8));

            StringBuilder sha256hash = new StringBuilder();

            // 256비트로 생성 => 32Byte => 1Byte(8bit) => 16진수 2자리로 변환 => 16진수 한 자리는 4bit
            for (byte b : hash) {
                sha256hash.append(String.format("%02x", b));
            }

            return sha256hash.toString();
        }catch (NoSuchAlgorithmException e){
            return e.toString();
        }
    }
}
```

#### 참고자료
###### https://qh5944.tistory.com/194

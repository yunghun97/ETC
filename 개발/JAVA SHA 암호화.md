# โฝ JAVA SHA ์ํธํ

## ๐ ์ฌ์ฉ ์ด์ 
- Password๋ฅผ DB์ ํ๋ฌธ์ผ๋ก ์ ์ฅ ์ ๋ณด์๋ฌธ์  ๋ฐ์ ๊ฐ๋ฅ

## ๐ ์์ค
```java
public class SHA256Encrypt {

    public static String encryptionPassword(String password){
        try {
            /* MessageDigest ํด๋์ค์ getInstance() ๋ฉ์๋์ ๋งค๊ฐ๋ณ์์ "SHA-256" ์๊ณ ๋ฆฌ์ฆ ์ด๋ฆ์ ์ง์ ํจ์ผ๋ก์จ 
				    ํด๋น ์๊ณ ๋ฆฌ์ฆ์์ ํด์๊ฐ์ ๊ณ์ฐํ๋ MessageDigest๋ฅผ ๊ตฌํ  ์ ์๋ค */
            MessageDigest messageDigest = MessageDigest.getInstance("SHA-256");
            // ๋ฐ์ดํธ ๋ฐฐ์ด๋ก ํด์ฌ๋ฅผ ๋ฐํํ๋ค.
            byte[] hash = messageDigest.digest(password.getBytes(StandardCharsets.UTF_8));

            StringBuilder sha256hash = new StringBuilder();

            // 256๋นํธ๋ก ์์ฑ => 32Byte => 1Byte(8bit) => 16์ง์ 2์๋ฆฌ๋ก ๋ณํ => 16์ง์ ํ ์๋ฆฌ๋ 4bit
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

#### ์ฐธ๊ณ ์๋ฃ
###### https://qh5944.tistory.com/194

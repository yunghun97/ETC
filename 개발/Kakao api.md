# ๐ฎ Naver api

## ๐ Kakao ๋ก๊ทธ์ธ
### ๊ฐ๋ฐ ํ๊ฒฝ
- Vue.js(3.x)
- Veux(4.x)

### index.html script ์ถ๊ฐ
```html
<script type="text/javascript" src="https://developers.kakao.com/sdk/js/kakao.min.js"></script>
```

### main.js ์นด์นด์ค ์ด๊ธฐํ (script ๋จ๊ณ์์ ํด๋๋๋ค.)
```javascript
window.Kakao.init(์นด์นด์คํค);
```

### ๋ก๊ทธ์ธ ํ์ด์ง div ์ถ๊ฐ
```html
<div id="kakao_id_login" @click="kakaoLogin"></div>
```

### ๋ก๊ทธ์ธ javascript
```javascript
const kakaoLogin = () => {
      // console.log(window.Kakao);
      window.Kakao.Auth.login({
        success: GetMe,
      });
    }
    const GetMe = () =>{
      // console.log(authObj);
      window.Kakao.API.request({
        url:'/v2/user/me',
        success : res => {
          const kakao_account = res.kakao_account;
          console.log(kakao_account);
            const userInfo = {
              nickname : kakao_account.profile.nickname,
              email : kakao_account.email,
              gender : kakao_account.gender,
              age : kakao_account.age_range,
              birthday : kakao_account.birthday,
              account_type : 2,
            }
            console.log(userInfo);         
```

### ํ ํฐ ๋ง๋ฃ
```javascript
window.Kakao.Auth.logout(()=>{
    console.log('๋ก๊ทธ์์ ๋์์ต๋๋ค.',window.Kakao.Auth.getAccessToken());
});
// ํ ํฐ์ ๋ง๋ฃ์ํค๋ ๋ถ๋ถ์ด์ฌ์ ์ฌ์ดํธ ๋ก๊ทธ์์์ ๋ณ๋์ ๋ก์ง์ผ๋ก ์ฒ๋ฆฌํ๋ฉด๋ฉ๋๋ค.
// + ํด๋น ์๋น์ค์์ ํํด์ํค๋ ๋ถ๋ถ์ ๊ณ์ ์ฃผ์ธ์ด ํํด
```

### ๋ก๊ทธ์์ (์ฝ๋ ํฅํ ์ถ๊ฐ ์์ )
```
POST /v1/user/logout HTTP/1.1
Host: kapi.kakao.com
Authorization: Bearer ${ACCESS_TOKEN}
```

### ์ฐ๊ฒฐ๋๊ธฐ (์ฝ๋ ํฅํ ์ถ๊ฐ ์์ )
```
POST /v1/user/unlink HTTP/1.1
Host: kapi.kakao.com
Authorization: Bearer ${ACCESS_TOKEN}
```
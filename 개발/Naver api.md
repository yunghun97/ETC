# ๐ค Naver api

## ๐ Naver ๋ก๊ทธ์ธ
### ๊ฐ๋ฐ ํ๊ฒฝ
- Vue.js(3.x)
- Veux(4.x)
- Naver api ๋ฒ์  2.0.2

### index.html script ์ถ๊ฐ
```html
<script type="text/javascript" src="https://static.nid.naver.com/js/naveridlogin_js_sdk_2.0.2.js" charset="utf-8"></script>
```

### ๋ก๊ทธ์ธ ํ์ด์ง div ์ถ๊ฐ
```html
<div id="naver_id_login" @click="naverLogin"></div>
```
### ๋ก๊ทธ์ธ ํ์ด์ง javascript
```javascript
onMounted(() => {      
      var naverLogin = new window.naver.LoginWithNaverId({
        clientId: process.env.VUE_APP_NAVER_KEY, // ํด๋ผ์ด์ธํธ id
        callbackUrl: process.env.VUE_APP_NAVER_CALLBACK_URL, // ์ฝ๋ฐฑ URL
        isPopup: true, // ํ์์ฐฝ ์์ฑ
        loginButton: {color: "green", type: 3, height: '60'} // ๋ก๊ทธ์ธ ๋ฒํผ ์์ฑ
      });
      naverLogin.init();      
})
    
    // ๋ก๊ทธ์ธ ๋ฒํผ ์ปค์คํํ๊ธฐ ์ํด์ ์ฌ์ฉํ๋ ์ฝ๋์๋๋ค. ์์จ๋ ๋ฌด๊ด
const naverLogin = () =>{
    var btnNaverLogin = document.getElementById("naverIdLogin").firstChild;
    btnNaverLogin.click();
}
```
  
### ์ฝ๋ฐฑ ํ์ด์ง javascript
```javascript
var naverLogin = new window.naver.LoginWithNaverId(
    {
        clientId: process.env.VUE_APP_NAVER_KEY,
        callbackUrl: "http://localhost:8080",
        isPopup: true,
        callbackHandle: true // ์ฝ๋ฐฑ ์ฒ๋ฆฌ
    }
);
naverLogin.init();

window.addEventListener('load', ()=>{
    naverLogin.getLoginStatus(function (status) {
        if (status) {
        var email = naverLogin.user.getEmail(); // ํ์๋ก ์ค์ ํ ๊ฒ์ ๋ฐ์์ ์๋์ฒ๋ผ ์กฐ๊ฑด๋ฌธ์ ์ค๋๋ค.
        console.log(naverLogin.user); 
        if( email == undefined || email == null) {
            alert("์ด๋ฉ์ผ์ ํ์์ ๋ณด์๋๋ค. ์ ๋ณด์ ๊ณต์ ๋์ํด์ฃผ์ธ์.");
            naverLogin.reprompt();
            return;
        }
    } else {
        console.log("callback ์ฒ๋ฆฌ์ ์คํจํ์์ต๋๋ค.");
    }
        });
});
opener.parent.location="/"
window.close();
```
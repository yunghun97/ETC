# 🤔 Naver api

## 📄 Naver 로그인
### 개발 환경
- Vue.js(3.x)
- Veux(4.x)
- Naver api 버전 2.0.2

### index.html script 추가
```html
<script type="text/javascript" src="https://static.nid.naver.com/js/naveridlogin_js_sdk_2.0.2.js" charset="utf-8"></script>
```

### 로그인 페이지 div 추가
```html
<div id="naver_id_login" class="pt-1 mb-4" @click="naverLogin"></div>
```
### 로그인 페이지 javascript
```javascript
onMounted(() => {      
      var naverLogin = new window.naver.LoginWithNaverId({
        clientId: process.env.VUE_APP_NAVER_KEY, // 클라이언트 id
        callbackUrl: process.env.VUE_APP_NAVER_CALLBACK_URL, // 콜백 URL
        isPopup: true, // 팝업창 생성
        loginButton: {color: "green", type: 3, height: '60'} // 로그인 버튼 생성
      });
      naverLogin.init();      
})
    
    // 로그인 버튼 커스텀하기 위해서 사용하는 코드입니다. 안써도 무관
const naverLogin = () =>{
    var btnNaverLogin = document.getElementById("naverIdLogin").firstChild;
    btnNaverLogin.click();
}
```
  
### 콜백 페이지 javascript
```javascript
var naverLogin = new window.naver.LoginWithNaverId(
    {
        clientId: process.env.VUE_APP_NAVER_KEY,
        callbackUrl: "http://localhost:8080",
        isPopup: true,
        callbackHandle: true // 콜백 처리
    }
);
naverLogin.init();

window.addEventListener('load', ()=>{
    naverLogin.getLoginStatus(function (status) {
        if (status) {
        var email = naverLogin.user.getEmail(); // 필수로 설정할것을 받아와 아래처럼 조건문을 줍니다.
        console.log(naverLogin.user); 
        if( email == undefined || email == null) {
            alert("이메일은 필수정보입니다. 정보제공을 동의해주세요.");
            naverLogin.reprompt();
            return;
        }
    } else {
        console.log("callback 처리에 실패하였습니다.");
    }
        });
});
opener.parent.location="/"
window.close();
```
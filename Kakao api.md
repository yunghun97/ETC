# 😮 Naver api

## 📄 Kakao 로그인
### 개발 환경
- Vue.js(3.x)
- Veux(4.x)

### index.html script 추가
```html
<script type="text/javascript" src="https://developers.kakao.com/sdk/js/kakao.min.js"></script>
```

### main.js 카카오 초기화 (script 단계에서 해도된다.)
```javascript
window.Kakao.init(카카오키);
```

### 로그인 페이지 div 추가
```html
<div id="kakao_id_login" @click="kakaoLogin"></div>
```

### 로그인 javascript
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

### 토큰 만료
```javascript
window.Kakao.Auth.logout(()=>{
    console.log('로그아웃 되었습니다.',window.Kakao.Auth.getAccessToken());
});
// 토큰을 만료시키는 부분이여서 사이트 로그아웃은 별도의 로직으로 처리하면됩니다.
// + 해당 서비스에서 탈퇴시키는 부분은 계정주인이 탈퇴
```

### 로그아웃 (코드 향후 추가 예정)
```
POST /v1/user/logout HTTP/1.1
Host: kapi.kakao.com
Authorization: Bearer ${ACCESS_TOKEN}
```

### 연결끊기 (코드 향후 추가 예정)
```
POST /v1/user/unlink HTTP/1.1
Host: kapi.kakao.com
Authorization: Bearer ${ACCESS_TOKEN}
```
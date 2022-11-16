# 😗 React 쿠키 사용 방법

## 1. 라이브러리 설치 
```
npm install react-cookie

# https://www.npmjs.com/package/react-cookie
```

## 2. 사용할 페이지 상위에 CookieProvider로 감싸주기 
```javascript
import { CookiesProvider } from "react-cookie";


function App() {
  return (
    <CookiesProvider>
      <Page1/>
      <Page2/>
    </CookiesProvider>
```

## 3. Set, Get Cookie
```javascript
import { Cookies } from "react-cookie";

const cookies = new Cookies();

<!-- 쿠키 설정 -->
<!--set(name: string, value: any, options?: CookieSetOptions | undefined): void  -->
cookies.set("cookie-test", "true");


<!-- 쿠키 데이터 가져오기 -->
<!--Cookies.get(name: string, options?: CookieGetOptions | undefined): any -->
cookies.get("cookie-test")
```

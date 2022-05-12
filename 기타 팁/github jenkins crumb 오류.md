# github jenkins crumb 오류

## 방법 1
1. jenkins plugin crumber 다운
2. 젠킨스 관리 -> Configure Global Security
3. 설정 변경
> CSRF Protection -> strict crumb 클릭  
> Check Session ID 체크박스 해제

## 방법 2
1. 젠킨스 관리 -> Script Console 접속
```bash
hudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION = true
```
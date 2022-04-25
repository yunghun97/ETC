## CORS 처리
### 헤더에 추가
```java
@CrossOrigin(origins = "*")
@RestController
public class Test02 extends HttpServlet {
	@GetMapping(value="/test02", produces = "text/html; charset=utf-8")
	public String service(HttpServletResponse response) {
		System.out.println("요청 들어옴...");
		response.setHeader("Access-Control-Allow-Origin", "http://127.0.0.1:5500");
//		response.setHeader("Access-Control-Allow-Origin", "*");
		return "<h1>Ajax CORS 호출 성공^^</h1>";
	}
}
```
<br>

### 다른서버에 통신 후 결과 StringBuilder에 저장 후 return
```java
@RestController
@CrossOrigin(origins = "*")
public class Test03 {
	
	@GetMapping(value="/test03")
	public String service(HttpServletResponse response) {
		StringBuilder sb = new StringBuilder();
		try {
			String SERVICE_KEY = "서비스키";
			System.out.println("요청 들어옴...");
			StringBuilder urlBuilder = new StringBuilder("http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev");
			urlBuilder.append("?serviceKey=" + URLEncoder.encode(SERVICE_KEY, "UTF-8")); /*Service Key*/
			urlBuilder.append("&LAWD_CD=11110"); /*지역코드*/
			urlBuilder.append("&DEAL_YMD=202110"); /*계약월*/
			URL url = new URL(urlBuilder.toString());
			HttpURLConnection conn = (HttpURLConnection) url.openConnection();
			conn.setRequestMethod("GET");
//			conn.setRequestProperty("Accept", "application/json");
			System.out.println("Response code: " + conn.getResponseCode());
			BufferedReader rd;
			if(conn.getResponseCode() >= 200 && conn.getResponseCode() <= 300) {
				rd = new BufferedReader(new InputStreamReader(conn.getInputStream()));
			} else {
				rd = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
			}
			String line;
			while ((line = rd.readLine()) != null) {
				sb.append(line);
			}
			rd.close();
			conn.disconnect();
			System.out.println(sb.toString());
		} catch (Exception e) {
			e.printStackTrace();
		}
		return sb.toString();
	}
}
```

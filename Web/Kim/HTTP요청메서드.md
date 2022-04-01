# HTTP 요청 메서드(GET, POST)

어떤 웹 사이트에 접속할 때, 클라이언트는 서버에게 웹 사이트를 요청하고 서버는 요청에 따라 응답합니다.   

<div align="center">
  <img src="https://user-images.githubusercontent.com/84266499/161308307-fece399c-cca6-4ef2-be1a-56db4524b2e8.png" width="35%" height="35%"/>
</div>

위와 같이 요청과 응답을 주고 받을 때, HTTP 프로토콜을 이용해서 주고받는다.     
HTTP의 동작은 클라이언트가 서버에 **요청**을 보내면서 시작되는데, 요청 시에 어떤 목적인지에 따라서 전송 방식이 달리지게 된다.    
대표적으로 GET방식과 POST방식이 있으며, 오늘은 이 두 방식의 차이점에 대해서 중점적으로 알아보겠다.      

> REST API라고 기존의 GET/POST 중심의 방식에서 요청에 조금 더 명확한 의미를 주기 위해서 DELETE, PUT등의 방식을 추가로 활용하는 방식도 있음     
> [링크](https://www.notion.so/3d6c70b3a86f450fbcf1ce2398d33560) 

## HttpRequestMethod     
클라이언트(브라우저)가 서버(톰캣)으로 요청을 보낼 때, 데이터 요청 방식을 지정할 수 있다.     

### 전송 페이지    
````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="<%=request.getContextPath() %>/send" method="post">
		  <input type="hidden" name="act" value="send"> 
      이름을 입력하세요: <input type="text" name="name"> <input type="submit" value="확인">
	</form>
</body>
</html>
````        
`<form>` 태그의 `method`속성을 통해서 요청 방식을 지정해줄 수 있음      
단순 링크를 요청하는 `get`방식의 경우 아래와 같이도 가능하다.     
````jsp
<a href="${root}/서블릿이름?act=동작이름&속성명=${넘겨줄속성}">
````

### 결과 페이지    
````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	안녕하세요, <%=request.getParameter("name") %>님!
</body>
</html>
````
### Servlet
````java
package servlet;

import java.io.*;
import javax.servlet.*;

@WebServlet("/send")
public class TestServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		String act = request.getParameter("act");

		if (act.equals("send")) {
			request.getRequestDispatcher("result.jsp").forward(request, response);
		}
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      request.setCharacterEncoding("utf-8");
      // GET방식과 동일
	}
}

````
 
 HTTP 프로토콜을 이용하여 요청/응답을 전송할 때 `request`, `response`의 형태는 아래와 같다.      
 
<div align="center">
  <img src="https://user-images.githubusercontent.com/84266499/161311640-6177ad7e-60a9-419b-8a82-ed6091b8d97c.png" width="35%" height="35%"/>
</div>

 
## get방식     
- 서버로부터 무언가를 얻기 위한 요청방식(링크 등)      
- 값을 전달할 때 URL에 값을 붙여서 전달한다.      
![image](https://user-images.githubusercontent.com/84266499/161312300-4179b039-7b81-4a73-8257-70399770bf7f.png)      
- 요청 URL 길이에 제한이 있기 때문에 대량의 데이터 전송시에는 부적합하다.     
- URL에 전송되는 데이터가 노출되기때문에 민감한 데이터는 보내지 않는다.     
크롬에서 헤더정보를 볼 수 있다.      
(F12 - Network)에서 확인 가능

![image](https://user-images.githubusercontent.com/84266499/161305578-a68d808e-8a75-4afe-8b90-ac7d7e8f2a9d.png)


## post방식      
- 브라우저 측에서 서버에 데이터를 전송하는 것이 주 목적이다.     
![image](https://user-images.githubusercontent.com/84266499/161305663-0f88ea19-2ba9-4459-b141-8d69ff253657.png)      
- HTTP Body에 데이터를 담아서 전송한다.      
- 데이터 전송 시 길이제한이 없다.      

Body로 데이터를 전송하기 때문에 외부에 전송하는 데이터가 노출되지 않는다.     
**== POST로 보내면 안전하다? 보안이 좋다?**    
이와는 별개의 이야기다. 겉으로 드러나지 않을 뿐 가로채서 보면 다 보임     
보안을 위해서는 암호화를 위한 추가적인 프로토콜이 필요하다.(http**s**)    

![image](https://user-images.githubusercontent.com/84266499/161305705-bf83143f-2ba6-4b0c-9176-73536695af0d.png)

Body에 act=send?name=갑경 의 형태로 전달된다.

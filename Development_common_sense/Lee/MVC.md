## MVC 패턴이란

<aside>
💡 어플리케이션을 세 개의 영역으로 분활하고 각 구성 요소에게 고유한 역할을 부여하는 개발 방식이다.
MVC 패턴을 도입하면 도메인(비지니스 로직) 영역과 `UI` 영역이 분리되므로 서로 영향을 주지 않고 유지보수가 가능하다.

</aside>

## MVC 패턴 구조

> MVC 패턴은 Model, View, Controller 세 개의 컴포넌트로 이루어져있으며, 각 컴포넌트는 고유한 역할을 수행한다.
>
<img src="https://user-images.githubusercontent.com/46440898/162586211-8a2bbcbb-1cf2-4808-9705-e3911402fece.png" width="400" height="350" align="left"/>

<img src="https://user-images.githubusercontent.com/46440898/162586204-28c231e7-af60-4901-8c5c-5f12a7ec55ae.png" width="450" height="380"/>

<p>출처 : [https://ko.wikipedia.org/wiki/모델-뷰-컨트롤러](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC)</p>


---

## Model

컨트롤러가 호출할 때, 요청에 맞는 역할을 수행한다.
비지니스 로직을 구현하는 영역으로 데이터를 처리하는 부분이다.

`DB` 에 연결하고 데이터 `CRUD` 등의 작업을 수행한다.

상태의 변화가 있을 때 컨트롤러와 뷰에 통보에 후속 조치 명령을 받을 수 있게 한다.

## View

컨트롤러부터 받은 결과 값을 가지고 사용자에게 출력할 화면을 만드는 일을 한다.

## Controller

**컨트롤러**는 클라이언트의 요청을 받았을 때, 그 요청에 대해 실제 업무를 수행하는 모델 컴포넌트를 호출한다.

클라이언트가 보낸 데이터가 있다면, 모델에 전달하기 쉽게 데이터를 가공한다.
모델이 업무를 마치면 그 결과를 뷰에 전달한다.

---

## MVC 구동 원리

![mvcServlet](https://user-images.githubusercontent.com/46440898/162586495-c773ddd9-1253-4133-aa34-886222d16539.png)

## Servlet 과 JSP 관점에서의 MVC 구조 순서

1. 웹 브라우저가 웹 서버에 요청
2. 웹 서버는 들어온 요청에 따라 `Servlet` 을 요청
3. `Servlet` 은 모델 자바 객체의 메서드를 호출
4. 데이터를 가공하여 값을 생성하거나, `JDBC` 를 사용하여 `DB` 와의 인터랙션을 통해 값 객체 생성
5. 수행을 마친 결과 값을 `Controller`에게 `return`
6. `Controller` 는 `Model` 로 부터 받은 결과 값을 `View` 에게 전달
7. `JSP` 는 전달받은 값을 참조하여 출력할 결과 화면을 만들고 `Controller` 에게 전달
8. `View` 로 부터 받은 화면을 웹 서버에게 전달
9. 웹 브라우저는 웹 서버로부터 요청한 결과 값을 응답받고 그 값을 화면에 출력

---

## Django MTV

`django`는 `MVC` 패턴과 같은 개념으로 `MTV(Model - Template - View)` 패턴을 기반으로 한다.

## Model

`DB`를 다루기 위해서는 `SQL`이라는 언어를 알아야하지만, `django`는 이 `SQL`을 몰라도 `DB` 작업을 가능하게 해주는 `ORM`을 제공한다.

> `ORM`이란?
>
>
> **`Object-Relational Mapping`**의 약자로, `SQL`이라는 언어 대신 데이터베이스를 쉽게 연결해주는 방법.
>

## 2. Template

`template` 는 **사용자에게 보여지는 부분**이다. `HTML`파일이 이 `template`을 담당한다.
`django template` 문법에 맞게 `python` 문법을 활용하여 작성하면 되므로, **다른 작업들과 화면 디자인 작업을 분리**하여 확장성을 극대화 시킬 수 있다. 즉, 보여지는 부분을 만드는 사람은 그 부분에만 집중하여 만들 수 있게 도와주는 역할을 한다.

django 템플릿 공식 문서 : [https://docs.djangoproject.com/en/4.0/topics/templates/](https://docs.djangoproject.com/en/4.0/topics/templates/)

## 3. View

뷰는 웹 요청을 받고, 전달받은 데이터들을 해당 어플리케이션의 로직으로 가공하여, 그 결과를 템플릿에 보내준다. 데이터를 가공하는 처리를 해야한다 싶으면 뷰에서 함수를 작성하면 된다.


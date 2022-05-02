# IoC(Inversion of Control, 제어의 역행)

- **객체에 대한 제어권이 개발자로부터 컨테이너로 넘어간 것**
- 객체의 생성부터 생명주기 관리까지 전부 컨테이너가 맡아서 하기 때문에 제어를 컨테이너가 갖고 있다.
  스프링에서 제공하는 컨테이너를 IoC 컨테이너라고 하기도 한다.
- 컨테이너가 직접 빈을 생성/관리하기 때문에 개발자는 코드에 `new` 등으로 선언하지 않아도 되며 이는 각 클래스들의 의존도를 줄여준다.
- 메소드나 객체의 호출작업을 개발자가 결정하는 것이 아닌, 외부에서 결정되는 것이다.
- 객체 간의 관계가 느슨하게 연결된다.(loose coupling)
- IoC의 구현 방법 중 하나가 DI(Dependency Injection)

# DI**(Dependency Injection)**

- Object에 lookup 코드를 사용하지 않고 컨테이너가 직접 의존 구조를 Object에 설정 할 수 있도록 지정해주는 방식.
- Setter Injection과 Constructor Inject
- DI(의존성 주입)를 통해서 모듈 간의 결합도가 낮아지고 유연성이 높아진다.

```java
<bean id="b" class="com.example.B" />
<bean id="a" class="com.example.A" />
  <constructor-arg ref="b" />
</bean>
```

```java
@Repository
public class BookDaoImpl implements BookDao {
	
	@Autowired
	private DataSource dataSource;
}
```

## Container

- 객체의 생성, 사용, 소멸에 해당하는 라이프사이클을 담당
- 라이프사이클을 기본으로 어플리케이션에 사용에 필요한 주요 기능을 제공
- 기능
    - 라이플사이클 관리
    - Dependency 객체 제공
    - Thread 관리
    - 기타 어플리케이션 실행에 필요한 환경
- 필요성
    - 비지니스 로직 외에 부가적인 기능들에 대해서는 독립저긍로 관리되도록 하기 위함
    - 서비스 look up 이나 Configuration에 대한 일관성을 갖기 위함
    - **서비스 객체를 사용하기 위해 각각 Factory 또는 Singleton 패턴을 직접 구현하지 않아도 됨**

## IoC Container

- 오브젝트의 생성과 관계설정, 사용, 제거 등의 작업을 어플리케이션 코드 대신 독립된 컨테이너가 담당한다.
- 컨테이너가 코드 대신 오브젝트에 대한 제어권을 갖고 있어 IoC라고 부름.
- 이런 이유로, 스프링 컨테이너를 IoC 컨테이너라고 부르기도 한다.
- 스프링에서 IoC를 담당하는 컨테이너에는 `BeanFactory`(객체를 만들어 주는 공장이라 생각하자), `ApplicationContext`(`BeanFactory`의 하위 클래스라고 볼 수 있다)가 있다.

## BeanFactory

- Spring DI Container가 관리하는 객체를 Bean이라 하고 이 빈들의 생명주기를 관리하는 의미로 BeanFactory라 한다.
- BeanFactory에 여러 가지 컨테이너 기능을 추가하여 ApplicationContext라 한다.
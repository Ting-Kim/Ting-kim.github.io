# 스프링(Spring Framwork) 개념

## 주요개념

- bean(빈)
- AOP(Aspect Oriented Programming)
- 프록시(proxy)
- Spring을 이용한 MVC 패턴
- DI(Dependency Injection)
- Transaction(트랜잭션)
- Servlet Filter
- IoC(Inversion Of Control) 컨테이너 : 부품 혹은 조립된 부품들이 담긴 컨테이너

#### bean 태그

```
<!-- xml 파일 (지시서) -->
<bean id="객체명" class="객체화할 클래스명(패키지명 포함)" />

<!-- Setter 사용 -->
<bean id="exam" class="spring.di.entity.NewlecExam">
	<property name="kor">
		<value>10</value>		<!-- value를 이렇게 줄 수도 있음. -->
	</property>
	<property name="eng" value="10" />
	<property name="mat" value="10" />
	<property name="com" value="10" />
</bean>

<!-- ex. ExamConsole console = new GridExamConsole(); -->
<bean id="console" class="spring.di.ui.InlineExamConsole">

	<property name="(Setter 함수명에서 set을 생략)" value="객체의 이름(value 타입일 경우 - 특정 인자의 경우?)" ref="객체의 이름(ref 타입일 경우 - 객체의 경우?)"/>

<!-- console.setExam(exam); (Setter 함수 지시사항)-->
	<property name="exam" ref="exam"/>

</bean>
```

```
<!-- xml 파일 (지시서) -->
<bean id="객체명" class="객체화할 클래스명(패키지명 포함)" />

<!-- 생성자를 통해서 생성하는 경우 -->
<bean id="exam" class="spring.di.entity.NewlecExam">
	<!-- index 없어도 됨 -->
	<constructor-arg index="0" value="10" />
	<constructor-arg index="1" value="20" />
	<constructor-arg index="2" value="30" />

	<!-- name, type 속성도 사용 가능 -->
	<constructor-arg name="mat" type="int" value="40" />
</bean>
```

```
public class Program{

	public static void main(String[] args){

		ApplicationContext context = new ClassPathXmlApplicationContext("spring/di/setting.xml");

		// bean 객체를 불러오는 두가지 방법
		//ExamConsole console = (ExamConsole)context.getBean("console");
		ExamConsole console = context.getBean(ExamConsole.class);
		console.print();
	}
}
```

#### DI(Dependency Injection)

```
// has-a 관계(일체형)
class A
{
	private B b;

	public A(){
		b = new B();
	}
}

// 조립형
class A
{
	private B b;

	public A(){

	}
	public void setB(B b){
		this.b = b;
	}
}
```

'부품 조립'

- Setter Injection : Setter 함수를 통해서 조립
- Contruction Injection : 생성자를 통해서 조립

##### ApplicationContext 종류

- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext
- XmlWebApplicationContext
- AnnotationConfigApplicationContext

#### AOP(Aspect Oriented Programming)

공통적인 기능은 특정 부분에만 필요한게 아니라, 어플리케이션 전반적인 부분에 필요한 부분.<br>
(공통기능과 핵심기능을 구분할 수 있어야 함)

- 권한을 요청하는 부분 -> 공통 기능(Crosscutting Concerns)<br>
  (로깅, 권한)

- write.do -> 비지니스 로직

#### 스프링 AOP 반드시 알아야 할 용어 5가지

- ㄱ. Aspect : 공통 관심사항(공통기능)을 모듈화한 것. 여러 비지니스 로직을 구현할 때 공통으로 사용되는 기능.
- ㄴ. Advice : 실질적으로 이루어지는 기능.
  ?)공통 기능을 핵심 기능에 언제 적용할지(시점).
- ㄷ. JointPoint : Advice를 적용할 수 있는 위치(지점). - 메서드 실행 시, 필드 변경 시 등..
- ㄹ. PointCut : Advice를 실제 적용한 지점.
  (PointCut에 사용되는 것이 AspectJ 문법)
- ㅁ. Weaving : 공통 기능을 핵심 기능인 비지니스 로직에 적용하는 것.

ArroundAdvice<br>- 보조 기능 클래스는 MethodInterCeptor 인터페이스를 구현<br>- app.LogPrintArroundAdvice 클래스 추가

##### 스프링은 프록시 기반의 AOP

- 사용 이유는 접근 제어 및 부가기능을 추가하기 위함.

---

20200804 Spring Days02

    	// BeforeAdvice
    	// 		핵심 관심 시행 ( core concern )이 처리되기 이전에 실행할 공통
    	// 		MethodBeforeAdvice를 구현
    	//		aop.LogPrintBeforeAdvice

    	// AfterAdvice						예외 발생여부 상관없이 처리되는 실행할 공통기능
    	// AfterReturningAdvice		핵심 기능이 예외없이 처리 되는 실행할 공통기능(예외 발생하면 실행 x)
    	//		-AfterReturningAdvice 인터페이스 구현
    	//		-aop.LogPrintAfterReturningAdvice
    	// AfterThrow~Advice			핵심 기능이 예외가 발생한 후 처리 되는 실행할 공통기능

// Before Advice 메서드 구현
// Joinpoint 인자는 실제 대상 객체(target)의 메서드 정보 또는
// 매개변수 값 등을 얻어올 수 있는 객체..
// 실제 대상 객체(target) - joinPoint.getTarget()
// 매개변수 값 - joinPoint.getArgs()
// 메서드 정보 - joinPoint.getSignature().getName()

pointCut 표현식 예시
<aop:before method="before" pointcut="execution(public _ aop.._(_,_))"/>
public => 접근제한자(스프링에선 보통 public, 생략 가능)

\*=> 모든 리턴 타입<br>
aop => 패키지명<br>
.. => aop 패키지 안의 모든 클래스(0개 이상) <br> \* => 모든 메서드 타입<br>
() => 파라미터 타입<br>
\*,\* 인자의 모든 타입

### context:component-scan 사용법

애노테이션(Annotation) 종류

- @Component
- @Service
- @Controller
- @Repository

위 넷은 스프링 2.5 버전 이상부터 사용가능한 streotype 애노테이션이다. 이들로 등록된 빈은 default로 스캔해준다.<br>
(default 애노테이션을 스캔하지 않는 방법도 있다. use-default-filters를 이용하면 된다. 기본값은 true이기 때문에 false로 선언해주면 디폴트 애노테이션들을 스캔하지 않는다.)

```
<context:component-scan base-package="패키지명" />
```

base-package : 패키지를 어디부터 스캔할지 지정해주는 부분<br>
(여러개도 지정 가능하다. ex. "newlecture.dao, controllers.customer")

### Spring MVC 패턴

구성요소

- DispatcherServlet : 모든 요청을 전달 받음(클라이언트의 요청을 전달받는 서블릿). 클라이언트의 요청을 Controller에 전달 및 Controller의 리턴 결과값을 View에 전달.
- HandlerAdapter ( Spring Bean 객체 ) : DispatcherServlet 요청을 변환해서 Conroller에 전달
- Controller ( Spring Bean 객체 )
- HandlerMapping ( Spring Bean 객체 )
- ViewResolver ( Spring Bean 객체 )
- ModelAndView(컨트롤러 실행 결과를 담을 객체)

Spring에서의 Controller는 모델 객체를 말한다.
일반적인 Handler를 Controller라고 칭함.<br>
보통 작업이 많이 필요한 부분은 Controller, JSP 이다. (나머지는 프레임워크 내부에 구현되어 있음.)

### XML 설정

DispatcherServlet 클래스에는 <servlet-name> 태그에 의해 서블릿 이름을 지정하여 정의한다.

이 서블릿 이름에 ‘-servlet.XML’ 라는 문자열을 부가함으로써 컨테이너 상에 로드된 스프링 설정 파일명이 결정된다.

##### front Controller / controller

Front Controller를 서블릿으로 등록할 때 XmlWebApplicationContext 스프링 컨테이너가 빈 객체 생성 및 조립을 하기 위해 dispatcher-servlet.xml(파일명 형식 : servletName-servlet.xml) 파일이 꼭 필요하다.

(\*View가 Spring에서는 꼭 JSP가 아닐 수 있다.)

##### 파라미터 받는 방법 3가지

1. String p_page = request.getParameter("pg");
2. @RequestParam
3. 커맨드 객체

#### 스프링에서 제공하는 파일 업로드 방법

1. form 태그의 enctype="multipart/form-data"
2. 스프링에서 제공하는 멀티파트 지원 기능 사용하기 위해서 [MultipartResolver 객체]를 빈으로 등록<br>
   (주의) 빈으로 등록될 때 빈 이름:
   (DispatcherServlet에서 위 이름을 사용하기 때문)

3. MultipartResolver 객체는 2가지 종류
   - CommonsMultipartResolver<br> - Commons FileUpload API를 이용해서 처리 - 설정하는 프로퍼티(속성)<br> - maxUploadSize : 최대 업로드 가능한 바이트 크기<br> - maxInMemorySize<br> - defaultEncoding
   - StandardServletMultipartResolver - 서블릿 3.0의 part를 이용해서 처리

#### error message :

```
심각: 경로 [/SpringMVC03]의 컨텍스트 내의 서블릿 [dispatcher]을(를) 위한 Servlet.service() 호출이, 근본 원인(root cause)과 함께, 예외 [Request processing failed; nested exception is org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors
Field error in object 'notice' on field 'file': rejected value []; codes [typeMismatch.notice.file,typeMismatch.file,typeMismatch.org.springframework.web.multipart.commons.CommonsMultipartFile,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [notice.file,file]; arguments []; default message [file]]; default message [Failed to convert property value of type 'java.lang.String' to required type 'org.springframework.web.multipart.commons.CommonsMultipartFile' for property 'file'; nested exception is java.lang.IllegalStateException: Cannot convert value of type [java.lang.String] to required type [org.springframework.web.multipart.commons.CommonsMultipartFile] for property 'file': no matching editors or conversion strategy found]]을(를) 발생시켰습니다.
org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors
Field error in object 'notice' on field 'file': rejected value []; codes [typeMismatch.notice.file,typeMismatch.file,typeMismatch.org.springframework.web.multipart.commons.CommonsMultipartFile,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [notice.file,file]; arguments []; default message [file]]; default message [Failed to convert property value of type 'java.lang.String' to required type 'org.springframework.web.multipart.commons.CommonsMultipartFile' for property 'file'; nested exception is java.lang.IllegalStateException: Cannot convert value of type [java.lang.String] to required type [org.springframework.web.multipart.commons.CommonsMultipartFile] for property 'file': no matching editors or conversion strategy found]
```

```
<form action="" method="post" enctype="multipart/form-data">
<!-- enctype="multipart/form-data" 를 추가 안해줘서 생기는 문제.. 왜? -->
```

20200806 Spring Days04
주요 문제들

- HashMap 사용 이유
- 쓰레드
- 트랜젝션
- ACID(트랜잭션의 종류(?))
- Propagation : 트랜잭션 개시할지 등 전파행위에 관한 속성.
- 전파행위 : 전파행위(거동)는 트랜잭션을 개시할지 혹은 기존 트랜잭션을 이용할지 등 트랜잭션 경계(Transaction Boundary)를 설정할 때 이용하는 속성으로 가장 중요한 속성
- isolation : 트랜잭션 격리레벨에 관한 속성으로 기본값은 Default레벨이며 실제 사용하는 데이터베이스(JDBC) 등의 기본값을 따릅니다.

Transaction을 Annotation을 통해 설정하는 경우

```

   <!-- @Transactional 어노테이션을 사용한 트랜잭션처리를 위해 추가한 코딩( 반드시 필요 ! ) >> 트랜잭션을 설정하려면 트랜잭션 매니저는  한곳에는 들어가야함. -->
   <tx:annotation-driven transaction-manager="transactionManager"/>
```

1. interface에 Transaction 어노테이션(Annotation)을 걸어주면 그 인터페이스를 Overriding한 클래스들에 모두 적용됨.

## 20200809 집에서 복습

#### @AutoWired

자동으로 찾아서 Setter에 인자를 대입시켜줌.
근데 어떤 기준으로 매칭시키는지 의문임.. (자료형이나 객체명이 기준이라면 동일한게 있을 경우 어떻게 처리하는지 등)

```
//spring.di.ui.InlineExamConsole Class
@Autowired
public void setExam(Exam exam) {
		this.exam = exam;
}

//xml 파일
<bean class="spring.di.entity.NewlecExam" p:kor="10" p:eng="10"/>
<bean class="spring.di.entity.NewlecExam p:kor="20" p:eng="20""/>
// 데이터 값이 다른 두개의 빈이 있는 경우 Err 발생

<bean id="exam1" class="spring.di.entity.NewlecExam" p:kor="10" p:eng="10"/>
<bean class="spring.di.entity.NewlecExam p:kor="20" p:eng="20""/>
// 빈의 id가 인자명이랑 달라도 Err발생

<bean id="exam" class="spring.di.entity.NewlecExam" p:kor="10" p:eng="10"/>
<bean class="spring.di.entity.NewlecExam p:kor="20" p:eng="20""/>
// 이 경우는 인자명과 bean의 id명이 동일해서 정상적으로 출력
```

```
//spring.di.ui.InlineExamConsole Class
@Autowired
@Qualifier("exam2")
public void setExam(Exam exam) {
		this.exam = exam;
}

//xml 파일
<bean id="exam1" class="spring.di.entity.NewlecExam" p:kor="10" p:eng="10"/>
<bean id="exam2" class="spring.di.entity.NewlecExam p:kor="20" p:eng="20""/>

// 이 경우는 @Qualifier 어노테이션을 사용해서 id값 지정
```

인젝션 방법 3가지

```

public class InlineExamConsole implements ExamConsole {

	//@Autowired	//1번
	//@Qualifier("exam2")
	private Exam exam;

	//@Autowired	//2번
	//@Qualifier("exam2")
	public InlineExamConsole(Exam exam) {
		this.exam = exam;
	}

	@Autowired		//3번
	@Qualifier("exam2")
	public void setExam(Exam exam) {
		this.exam = exam;
	}
}

// 1번은 기본생성자 호출되는 과정에서 Injection이 되는 경우(기본생성자 없으면 실행 불가(오버로드 생성자도 없는 경우는 가능))
// 3번은 Setter가 실행되면서 Injectrion이 되는 경우

// 2번은 저렇게 쓰면 안되고 이렇게 써야함.

	@Autowired	//2번
	public InlineExamConsole(@Qualifier("exam2")Exam exam) {
		this.exam = exam;
	}
```

넣어줄 인자가 null일 경우에도 실행되게 하려면?<br>
Autowired 어노테이션에 required 속성을 false로 지정하면 된다.

```
	//@Autowired(required=false)
	//@Qualifier("exam2")
	private Exam exam;
```

### AOP(Aspect Oriented Programming)

OOP - 사용자가 이용하는 업무로직에만 관심이 있었음.

개발자나 관리자에 의해 업무로직을 구현하기 위한 코드들이 있는데..
관점지향은 그러한 코드들과 주 업무로직의 단위업무(객체들)를 분리해서 전체적으로 바라보는 방법론이다..

- 로그처리
- 보안처리
- 트랜잭션처리

이것들은 이용자의 요구사항이 아니다..
Cross-cutting Concern은 코드에서 따로 분리하고 결합할 수 있어야 함.

Proxy 클래스로 Cross-cutting Concern을 구현하고, 중간에 Core Concern을 호출..!
(~ 뉴렉처 - 19강 'Java 코드로 AOP이해')

```
package spring.aop;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

import spring.aop.entity.Exam;
import spring.aop.entity.NewlecExam;

public class Program {
	public static void main(String[] args) {
		// Exam exam만 사용하면 원래 업무만 사용함.
		Exam exam = new NewlecExam(1,1,1,1);


		// Exam을 쓰는것 같이 느껴질 수 있지만, proxy를 쓰게되면 곁다리 업무를 포함해서 원래 Exam 업무를 같이 사용함.
		Exam proxy = (Exam)Proxy.newProxyInstance(NewlecExam.class.getClassLoader(), new Class[] {Exam.class},
				new InvocationHandler() {

					@Override
					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
						long start = System.currentTimeMillis();

						Object result = method.invoke(exam, args);    // obj는 실제 업무 객체, 호출하는 메서드가 가지고있는 파라미터를 args에다가 넣어주고 그걸 인자로 받음.

						long end = System.currentTimeMillis();
						String message = (end - start) + "ms 시간이 걸렸습니다.";
						System.out.println(message);
						return result;
					}
				}
		);

		System.out.printf("total is %d\n", proxy.total());
		System.out.printf("total is %.0f\n", proxy.avg());
	}
}
```

실제 업무(Com Concern)와 보조 업무(Cross-cutting)를 분리하고 결합하는 경우는 다음 4가지가 있다.

- Before Advice(앞만 필요)
- After returnning Advice(뒤만 필요)
- After throwing(예외처리만)
- Around Advice(앞뒤 모두 필요)

포인트컷(PointCut)과 조인 포인트(JoinPoint) 그리고 위빙(Weaving)

조인 포인트를 Cut하는 별도의 정보를 PointCut이라 함.
(객체에 존재하는 어떤 객체에 한해서만 JoinPoint를 설정하는 정보..?)

### Annotation 모음

@Controller - 클래스

- 컨트롤러 객체임을 명시

@RequestMapping -클래스, 메소드

- 특정 URL에 매칭되는 클래스나 메소드임을 명시

@RequestParam -파라미터

- 요청에서 특정한 파라미터 값을 찾아낼 때 사용

@RequestHeader -파라미터

- 요청에서 특정 HTTP 헤더 정보를 추출할 때 사용

@PathVariable -파라미터

- 현재 URL에서원하는 정보를 추출할 때 사용

@CookieValue -파라미터

- 현재 사용자의쿠키가 존재하는 경우 쿠키의 이름을 이용해서 쿠키값 추출

@ModelAttribute -메소드, 파라미터

- 자동으로 해당 객체를 뷰까지 전달하도록 만든 것

@SessionAttribute -클래스

- 세션상에서 모델의 정보를 유지하고 싶은 경우

@InitBinder -메소드

- 파라미터를 수집해서 객체로 만들 경우 커스터마이징

@ResponseBody -메소드,리턴타입

- 리턴 타입이 HTTP의 응답 메시지로 전송

@RequsetBody -파라미터

- 요청 문자열이 그대로 파라미터로 전달

@Repository -클래스

- DAO 객체

@Service - 클래스

- 서비스 객체

### ref)

- https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-XML-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC : [Spring] 스프링 XML 설정 파일 작성 방법 정리
- https://zorba91.tistory.com/249 : [Spring] context:component-scan 사용법 정리
- https://docs.spring.io/spring/docs/3.0.0.M4/reference/html/ch03s10.html : 스프링 공식문서(3.10 Classpath scanning and managed components)
- https://reiphiel.tistory.com/entry/understanding-of-spring-transaction-management-practice [레이피엘의 블로그] Spring 트랜잭션 관리방법
- https://freehoon.tistory.com/110 Spring 블로그 만들기 - 8.트랜잭션 처리
- https://devbox.tistory.com/entry/Spring-webxml-%EA%B8%B0%EB%B3%B8-%EC%84%A4%EC%A0%95 [Spring] web.xml 기본 설정

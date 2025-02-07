---
layout: single
title: "Spring_01_스프링 프레임워크"
date: 2024-10-13
categories:
  - Spring
tags:
  - SpringFramework
---

<br>

---

### 스프링 프레임워크
: 자바 플랫폼을 위한 오픈 소스 어플리케이션 프레임워크. <br>
JEE를 개발하기 위한 솔루션. <br>
자바 객체를 담아서 직접 관리하는 경량 컨테이너. <br>
객체의 생성부터 소멸에 이르기까지 라이프 사이클을 관리한다. <br>

> POJO (Plain Old Java Object) <br>
평범한 자바 오브젝트 <br>
상속이나 포함관계가 없는 순수하게 멤버로만 구성된 클래스 <br>
<br>

EJB (Enterprise JavaBeans) <br>
빈 객체 <br>

<hr>


- 동작 순서

![](/assets/image/2024-10-15-Spring_MVC_Container.001.png) | ![](/assets/image/2024-10-15-Spring_MVC_Container.003.png)

<details>
<summary>View & ViewResolver 방식 모식도</summary>
<img src="/assets/image/2024-10-15-Spring_MVC_Container.004.png" />

<pre><code>
- DispatcherServlet 
    = “Front Controller”
    : 클라이언트의 모든 요청을 전달받는다. 
- HandlerMapping 
    : 요청에 맞는 컨트롤러 검색 (cf. 핸들러 = 컨트롤러 | 매핑 = 검색) 
    → “return Controller”
- Controller 
    : Model에 Business Logic 요청 
    → "return ModelAndView" : 모델에서 처리한 결과와 뷰리졸버에서 처리할 정보를 담아서 반환
- ViewResolver 
    = “application.properties”
    : 컨트롤러로부터 "View 객체"를 받아서 폴더 경로 및 확장자를 작성한 후 반환
    → "return View”
</code></pre>
</details>

<br>
<hr>
<br>

### Maven Project (Eclipse) <br>


1> &lt;dependency&gt; pom.xml 파일에 org.springframework를 추가 <br>

<pre><code>
&lt;dependencies&gt;
    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework&lt;/groupId&gt;
        &lt;artifactId&gt;spring-context&lt;/artifactId&gt;
        &lt;version&gt;6.1.10&lt;/version&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework&lt;/groupId&gt;
        &lt;artifactId&gt;spring-core&lt;/artifactId&gt;
        &lt;version&gt;6.1.10&lt;/version&gt;
    &lt;/dependency&gt;
&lt;/dependencies&gt;
</code></pre>


<details>
<summary>pom.xml 설정 이미지</summary>
<div class="image-container">
    <img class="image-medium" src="/assets/image/2024-10-15-Spring-maven.png">
</div>
</details>


2> interface 생성

<pre><code>
package pack;

public interface MessageInter {
	void sayHello(String name);
}
</code></pre>


3> 구현체 생성 

<pre><code>
package pack;

public class Message implements MessageInter{
	@Override
	public void sayHello() {
		System.out.println("Hello");
	}
}
</code></pre>


4> init.xml 파일에 bean 태그 작성 : 생성자를 호출한다.

<pre><code>
&lt;!-- * bean 설정 등의 작업을 진행 * --&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                            http://www.springframework.org/schema/beans/spring-beans.xsd"&gt;
     
    &lt;!-- bean : jsp 액션태그와 유사 "생성자 호출" : 싱글톤으로 객체 인스턴스 생성 --&gt;
    &lt;!-- 객체 변수 mes로 설정 --&gt;
    &lt;bean id="mes" class="pack.Message" /&gt;
    
&lt;/beans&gt;
</code></pre>
> pack.Message mes = new pack.Message(); 와 같다.
<br>


5> main() 실행 : init.xml 에서 bean 객체 호출

<pre><code>
package pack;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MessageMain {
    public static void main(String[] args) {
        // init.xml 에서 환경 설정 : 빈 객체 인스턴스 생성 &lt;bean id="mes" class="pack.Message1"/&gt;
        // 스프링이 생성한 객체를 읽어서 실행
        ApplicationContext context = new ClassPathXmlApplicationContext("init.xml");

        // 다형성 활용 (MessageInter 인터페이스에 Message 참조값 저장)
        MessageInter inter = (MessageInter) context.getBean("mes"); // "반환 타입 : Object" : 강제 형변환 (Casting) 필수
        inter = context.getBean("mes", MessageInter.class); // 미리 클래스타입을 설정할 수 있다.
        inter.sayHello();
    }
}
</code></pre>
> org.springframework.context.ApplicationContext <br>
org.springframework.context.support.ClassPathXmlApplicationContext <br>
org.springframework.context.ApplicationContext.getBean()

<br>

ApplicationContext 는 BeanFactory를 상속한 인터페이스로 Spring Container라고 불린다. 

<br>
<hr>



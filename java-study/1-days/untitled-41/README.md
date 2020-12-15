---
description: 2020.12.15 - 84일차
---

# 84 Days - Spring : String, Annotation\(Board\), xml\(Book\), 생성자객체주입, Android : Event

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring - Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : MVC 

### request, response

* void\(Lv1\) -&gt; ModelAndView\(Lv2\) -&gt; String\(Lv3\)
* req, res를 사용해야한다는 것은 상속을 받아야 하므로 의존적인 구성이 된다는 것이다.
* void타입과 ModelAndView타입을 활용하는 메서드를 갖는 클래스는 req, res를 상속받아야 한다.
* 이를 벗어나기 위해 String타입을 사용하게 된다.

### String

* string 타입의 메서드를 활용하면서 req와 res를 주입받지 않아도 되게 되면서 페이지간의 관계가 더 자유로워 지게 되었다.
* req, res를 받지 않으므로 forward와 sendRedirect, getAttirbute, getParamter, setAttribute를 사용할 수 없게 되는 대신, 어노테이션을 활용해 이와 같은 역할을 똑같이 수행할 수 있다.
* 기존의 ModelAndView에 담던 페이지 이동은 리턴값을 이용한다. - return "redirect:xxx.jsp";

### 추상클래스와 인터페이스

* 인터페이스를 활용하는 경우, 타입과 파라미터가 고정된 메서드를 오버라이드 해야만한다.
* 하지만 추상클래스를 활용하는 것보다는 재사용성이 높다.
* 인터페이스는 자체적으로 인스턴스화 될 수 없기때문에 구현체 클래스를 활용하고, 이 다형성으로 인해 같은 인터페이스 이더라도 다른 결과를 출력할 수 있기 때문이다.

## Spring : Annotation X, XML O

### 어노테이션을 사용하지 않는 경우

* spring-servlet.xml : Controll 계층 + ViewResolver + Resource\(상수 : 이미지, 애니메이션, 색상, ....\) spring-service.xml : Model 계층 spring-data.xml : Model 계층\(DB에 자원관리\) mybatis-config.xml
* xml문서에 매번 빈을 등록해야 하고, java와 여러 xml문서를을 동기화해야하므로 복잡하다.
* 동기화 - DispatcherServlet과 업무별 Controller - XXXController와 XXXLogic : java와 java - XXXLogic과 SqlXXXDao : java와 java - java와 java이지만 일반 인스턴스화가 아닌 xml을 통한 외부 주입을 받기 위한 코드가 필요하다.

### 응답페이지 : ViewResolver + ModelAndView

* ViewResolver - 응답할 view 페이지에 대한 url정의 - 접두어 : "WEB-INF/views/"   접미어 : ".jsp" - 작성시 "/"와 응답페이지를 호출하는 클래스에서의 페이지 이름에 ".jsp"가 붙는지 주의 한다.
* ViewName - Lv2   ModelAndView mav = new ModelAndView\( \);    mav.setViewName\("/XXX/xxxList"\)
* 응답페이지 : WEB-INF/views/XXX/xxxList.jsp
* 컨트롤계층 안에서 모두 정해진다.

### spring-servlet.xml : Controller

* &lt;bean id=" " class=" "&gt; : setter메서드를 호출하는 곳, class=실제 위치\(패키지 + 파일이름\)      &lt;property name=" " ref=" "/&gt; : setter메서드 이름 &lt;bean&gt;
* 이렇게 등록해야 필요한때, bean에 등록된 클래스의 id가 호출되면 작성된 setter메서드에 외부에서 객체를 생성해서 주입 해준다.

### spring-service.xml : Logic

* &lt;bean id=" " class=" "&gt; : spring-servlet.xml의 proterty ref에 작성된 이름과 일치하는 id      &lt;property name=" " ref=" "/&gt; : 이 bean의 class가 주입받아야하는 객체의 setter메서드와 객체이름 &lt;bean&gt;

### spring-data.xml : Dao, 연동자원

```markup
	<bean id="data-source-target" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName">
         <value>oracle.jdbc.driver.OracleDriver</value>
        </property>
		<property name="url">
			<value>jdbc:oracle:thin:@ip:1521:식별</value>
		</property>
		<property name="username">
			<value>scott</value>
		</property>
		<property name="password">
			<value>tiger</value>
		</property>
	</bean>

	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="configLocation" value="WEB-INF/mybatis-config.xml"/>
		<property name="dataSource" ref="data-source-target"/>
	</bean>
	
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory"/><!-- 생성자 객체 주 -->
	</bean>
	
	<bean id="sql-member-dao" class="com.spring.mvc1.SqlMemberDao">
		<property name="sqlSessionTemplate" ref="sqlSessionTemplate"/>
	</bean>	
```

* service.xml에서 Dao를 호출해서 data.xml로 오는 것 까지는 java : xml이지만, mybatis와 연동되는 부분은 xml : xml이다.
* 자바가 아니기 때문에 setter메서드가 아닌 생성자로 객체 주입을 해야한다. --22
* SqlSessionTemplate : selectOne\( \), selectList\( \), insert\( \), update\( \), delete\( \) SqlSessionFactoryBean : DB와의 Connection 연결통로
* 순서상으로 FactoryBean이 먼저 생성되어야 Template를 사용 할 수 있다. Template가 FactoryBean에게 의존해 있다.

### Book : xml

{% page-ref page="book-xml.md" %}

## Spring : Annotation O, XML X

### 어노테이션을 사용하는 경우

* 컨트롤 계층 : @Controller 모델     계층 : @Service
* @Autowired 어노테이션을 인스턴스 변수에 붙이기만 하면 된다.

### url-pattern : @RequestMapping , DI : @Autowired

```java
package com.mvc3.board;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/board/*")
public class BoardController {	
	
	@Autowired
	public BoardLogic boardLogic = null;
	
	@RequestMapping("/boardList.sp3")
	public String boardList(Map<String,Object> pMap) {
		logger.info("boardList 호출 성공 : "+pMap);

		return "foward:list.jsp";
	}
}
```

* 어노테이션과 String을 활용한다면 기존의 url-pattern은 어떻게 작성 할까?
* 역시 어노테이션을 활용한다. : @RequestMapping
* Controller클래스는 @Controller 어노테이션을 작성하고, 그 밑에 url을 작성한다. @RequestMapping\(/member/\*\) or \(/order/\*\)
* 그리고 Controller안에서 구현하는 메서드에 업무내용 url을 작성해 분류한다. @RequestMapping\(/memberList.do\) or \(/memberInsert.do\)
* 의존성 주입은 xml작성 대신에 멤버변수에 어노테이션 @Autowired를 붙인다.

### Board : Annotation

{% page-ref page="board-annotation.md" %}

## Spring : 생성자 객체 주입

### spring의 IoC

* 어노테이션을 작성하지 않는 경우에 외부에서 객체를 주입받는 방법은 두가지가 있다.
* java에서 setter메서드를 활용하거나, \(java : xml\) xml을 통한 생성자 객체 주입법을 활용하거나 \(xml : xml\)

### 생성자 객체 주입법

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="data-source-target" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName">
         <value>oracle.jdbc.driver.OracleDriver</value>
        </property>
		<property name="url">
			<value>jdbc:oracle:thin:@192.168.0.187:1521:orcl11</value>
		</property>
		<property name="username">
			<value>scott</value>
		</property>
		<property name="password">
			<value>tiger</value>
		</property>
	</bean>
	
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="configLocation" value="WEB-INF/mybatis-config.xml"/>
		<property name="dataSource" ref="data-source-target"/>
	</bean>
	
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory"/>
	</bean>
	
	<bean id="sql-member-dao" class="com.spring.mvc1.SqlMemberDao">
		<property name="sqlSessionTemplate" ref="sqlSessionTemplate"/>
	</bean>	
</beans>
```

* 27번이 생성자 객체 주입법에 해당한다. 
* spring 과 mybatis가 만나는 부분을 xml에서 처리하는 방식이다.

## AndroidStudio : Event

### activity : intent

* 한 프로젝트에 여러 activity를 사용 할 수도 있다.
* manifest에 등록해서 활용한다.
* 이때 사용하는 것이 intent이다.

### Event Model

* 델리게이션 이벤트 모델 : 기존방식
* 하이어라키 이벤트 모델 : 단말기로 확인 가능한 모델

### Event처리 - 1 : 인터페이스 Override

```java
package com.example.delegationevent69;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        LinearLayout linearLayout = new LinearLayout(this);
        Button btn = new Button(this);
        btn.setText("전송");
        btn.setOnClickListener(onClick(v:this));
        linearLayout.addView(btn);
        setContentView(linearLayout);
    }

    @Override
    public void onClick(View v) {
        Toast.makeText(getApplicationContext(), "버튼누름",Toast.LENGTH_LONG).show();
    }
}
```

* 기존에 java에서 Event를 처리하는것과 비슷한 형태
* View.OnClickListerner 인터페이스를 implements하고 onClick메서드를 오버라이드 한다.

### Event처리 - 2 : 익명클래스

```java
package com.example.delegationevent69;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        LinearLayout linearLayout = new LinearLayout(this);
        Button btn = new Button(this);
        btn.setText("전송");
        
        btn.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View v) {
                Toast.makeText(getApplicationContext(), "버튼 누름",Toast.LENGTH_LONG).show();
            }
        });
        linearLayout.addView(btn);
        setContentView(linearLayout);
    }
}
```

* 인터페이스를 implements하지 않고 익명클래스로 필요한때에 작성한다.
* 20-25번

### Event처리 - 3 : 내부클래스

```java
package com.example.delegationevent69;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    class MyEventHandler implements View.OnClickListener{
        @Override
        public void onClick(View v) {
            Toast.makeText(getApplicationContext(), "버튼누름",Toast.LENGTH_LONG).show();
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        LinearLayout linearLayout = new LinearLayout(this);
        Button btn = new Button(this);
        btn.setText("전송");
        
        MyEventHandler myEvent = new MyEventHandler();
        btn.setOnClickListener(myEvent);
        
        linearLayout.addView(btn);
        setContentView(linearLayout);
    }
}
```

* 클래스 내부에 View.OnClickListener를 implements하는 이벤트핸들러 구현체 클래스를 정의한다.
* 이벤트를 적용할 객체에는 인스턴스화를 통해 이벤트를 처리한다.
* 27-28번

후기 : 매일 최선을 다하자 프로젝트에도 수업에도! 


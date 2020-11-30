# Sonata b

### 코드 : Sonata.java

```java
package com.di;

public class Sonata {
	String carColor = null;
	int speed = 0;
	int wheelNum = 0;
	
	//생성자 객체주입법 -- 생성자로 초기화
	public Sonata() {}
	
	public Sonata(int speed) {
		this.speed = speed;
	}
	
	public Sonata(String carColor, int speed) {
		this.carColor = carColor;
		this.speed = speed;
	}
	
	public Sonata(String carColor, int speed, int wheelNum) {
		this.carColor = carColor;
		this.speed = speed;
		this.wheelNum = wheelNum;
	}
	
	//Object가 제공하는 메서드
	@Override
	public String toString() {
		return "그녀의 자동차는 "+this.carColor+" 이고, 현재속도는 "+this.speed+" 이고, 바퀴 수는 "+this.wheelNum;
	}
}

```

### 코드 : sonataBean.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans 
    xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
   <bean id="myCar" class="com.di.Sonata"/><!-- xml을 통해 소나타객체를 주입받을 수 있는 준비 끝 -->
   
   <bean id="herCar" class="com.di.Sonata">
   	<constructor-arg index="0" type="java.lang.String" value="흰색"></constructor-arg>
   	<constructor-arg index="1" type="int" value="50"></constructor-arg>
   </bean>
   
   <bean id="himCar" class="com.di.Sonata">
   	<constructor-arg index="0" type="java.lang.String" value="검정색"></constructor-arg>
   	<constructor-arg index="1" type="int" value="50"></constructor-arg>
   	<constructor-arg index="2" type="int" value="4"></constructor-arg>
   </bean>
</beans>
```

### 코드 : SonataSimulation.java

```java
package com.di;

import org.springframework.beans.factory.BeanFactory;//spring-bean.jar
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.context.ApplicationContext;//spring-context.jar
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;

//request, response를 사용할 수 없다.
//서버와 통신하지 않는, local로 실행되므로
//웹서비스를 제공할 수 없으므로 안드로이드와 연계가 불가능하다.
//자바는 web.xml을 지원하지 않기 때문에 io로 읽고, 쓰기를 해야하는데 속도가 느리고, 서버에 부담을 준다.
//그러므로 자바로 web서비스를 하지말자
//spring-core.jar가 관리하는 ApplicationContext, BeanFactory가 bean을 관리한다.
//<bean id="" class|type=""/>로 관리한다. --type인 경우에는 추상클래스, 인터페이스까지 올 수 있다.
public class SonataSimulation {
	
	//자바 어플리케이션에서는 톰캣서버를 기동하지 않으므로 xml문서를 스캔하지 않기 때문에 직접 가져와야한다.
	//직접 스캔하기때문에 setter메서드가 필요없이 id로 직접 접근한다.
	//생성자 객체 주입법은 필요할까?
	//setter객체 주입법 코드는 자바에서 처리하고, 생성자 객체 주입법 코드는 xml에서 처리한다.
	//기존에는 VO class를 직접 만들어 사용했다. 기본적으로 멤버변수가 private이므로 접근하기 위해 setter, getter메서드로 활용했다.
	//이 변수를 별도로 초기화 하기 위해 값을 결정하는 클래스에서 직접 인스턴스화해서 set메서드로 값을 초기화하거나 생성자 파라미터에 담아 초기화한다.
	//dVO.setViewName("xxx.jsp") - new DeptVO("10", true, "xxx.jsp">
	//setter객체 주입법은 동종간 처리시 사용되고 - 권장사항
	//생성자 객체 주입버은 이종간 처리시 사용한다. - 자바와 myBatis, 자바와 Oracle이런 처리
	//결론 : xml과 xml사이에서도 객체 주입을 처리할 수 있다. 지원한다.
	/*
	 * ArrayList al = new ArraList();
	 * ArrayList al = null;
	 * ArrayList al = 타입.methodA();
	 * 나는 인스턴스화를 할 수 있다.
	 */
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("com\\di\\sonataBean.xml");
		Sonata myCar = (Sonata)context.getBean("myCar");
		
		Resource resource = new FileSystemResource("C:\\workspace_sts3\\spring3\\src\\main\\java\\com\\di\\sonataBean.xml");
	    BeanFactory factory = new XmlBeanFactory(resource);//가운데 줄은 depricated대상이다.
	    Sonata herCar = (Sonata)factory.getBean("herCar");
	    Sonata himCar = (Sonata)factory.getBean("himCar");
	    
	    Sonata gnomCar = new Sonata();
	    
	    System.out.println("myCar : "+myCar);
	    System.out.println("herCar : "+herCar);
	    System.out.println("himCar : "+himCar);
	    System.out.println("himCar : "+gnomCar);

	}

}
```




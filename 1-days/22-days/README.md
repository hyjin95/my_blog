---
description: 2020.08.28 - 21일차
---

# 22 Days - 문제풀이 및 복습

## 복습

### 전제조건 : 자바에서는 같은 이름의 메서드를 선언할 수 있다. 이를 가능하게 하는 매카니즘은 무엇인가? 

* 메서드 오버로딩
* 메서드 오버라이드
* 오버라이드는 오버로드 전제조건에 서로 상속관계에 있어야한다. -&gt; 인터페이스와 추상클래스

### 왜 인터페이스와 추상클래스 중심의 코딩을 전개 해야 하는가? 

* 인터페이스 - 기능을 활용하기 위함이다. - 결합도를 낮춰주어 단위테스트가 가능하다. -&gt; 통합테스트가능 -&gt; 남품가능\(배포\) - 테스트 서버와 운영서버가 다르다. - 개발은 테스트 서버에서 한다.
*  추상클래스 - 확장의 개념으로, 어떤 객체에 기능을 붙여 새로운 종자, 객체를 만들어준다. - 상속을 해야한다 -&gt; 결합도가 높다. 단점1 - 다중상속 불가 단점2
* 결합도와 단일상속의 단점을 보완하기위해 인터페이스가 제공된다. 

## 문제풀이

### override, overloading

```java
package scjp.method;

public class MethodOver {
	public void setVar(int a, int b, float c) {
		
	}

	public void setVar(int a, long b, float c) {//타입이 다르거나 파라미터갯수가 달라야만한다.
		
	}
	
	public void setVar(int a,float c) {//파라미터갯수가 다른경우
		
	}
	
	public int setVar(int a, int b, int c) {//타입이 다른경우
		return a+b+c;
	}
	
	public int setVar(int a, int b) {//타입과 파라미터가 다른경우 == 오버라이딩
		return a+b;
	}
	
	public static void main(String[] args) {
		//대표적인 오버로딩의 예==print
		System.out.println(5);
		System.out.println();
	}
}
```

{% page-ref page="undefined.md" %}

## 한줄일기

코로나 사회적 거리두기 2.5단계에 의해 학원이 당분간 쉬게 되었다. 요즘에는 하루가 30시간이였으면 했으니까..재충전하고 열심히 다시 달려보자


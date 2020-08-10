---
description: 2020.08.07 - 8일차
---

# 8 Day - 야구 숫자 게임

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

### 구현 준비

1. 화면을 그린다. - 소통\(파라미터, 리턴타입 이용\)을 위한 종이 스케치하는 단
2. 게임의 규칙을 말해본다. - 업무, 규칙을 순서대로 정리하는 단계
3. 판정하는 기능은 메소드로 꺼낸다. - 판정 : if \(기준이 필요하다\) - 기능 : 메소드\(메소드이름 정하기\) 
4. 이벤트 처리 대상인 것들을 적어본다. - 이벤트 처리 : 요청 - 대상 : 버튼
5. 전역변수로 할 것과 지역변수로 할 것을 정한다.

### 규칙

* 시스템이 뽑은 숫자 : int com\[ \]  : a, b, c

| 인덱스 값 | com\[0\] | com\[1\] | com\[2 |
| :---: | :---: | :---: | :---: |
| 값 | a | b | c |

* 사용자가 고른 숫자 : int my\[ \]  : @, \#, $ 

| 인덱스  | my\[0\] | my\[1\] | my\[2\] |
| :---: | :---: | :---: | :---: |
| 값 | @ | \# | $ |

* com\[0\] == my\[0\] 의 경우는 strike로 카운트
* com\[0\] == my\[1\] \|\| my\[2\] 의 경우는 ball로 카운트
* for\(for\)문을 사용한다. - 랜덤채번 : do-while - 판정 : if
* 변수는 strike, ball이다 - 카운트가 진행 횟수마다 변화해야하므로 둘다 지역변수로 사용한다.
* 3strike는 정답이다.
* 사용자가 JTextFeild에 입력한 숫자들은 sting타입이다. - 비교하는 시스템 채번 숫자는 int이므로 형전환을 해야한다.
* 사용자 입력 값이 "256"일때, 각 인덱스 값에 넣기위해 자리수를 분리해야한다, - 256/100 = 2 - 256=int, 100=int이므로 int인 2만 남는다. - \(256%100\)\*10 = 5 - 몫=2, 나머지 0.56 -&gt; 5.6 int인 5만 남는다. - 265%10 = 5

### 클래스 쪼개기

* UI       : BaseBallGameView.java
* Event : BaseBallGameEvent.java
* 로직  : BaseBallGamelogic.java - my\[ \]는 view에서 입력되는 String 값 - com\[ \]은 logic에서 채번되는 int  

### 예외처리

* Try-catch
* 사용자가 입력하는 입력창에 읽은 수 없는 값이 들어와서 읽을 수 없는 등 과 같은 오류가 일어나면 다음 다음 진행으로 넘어간다.

### return을 이용한 메소드 나가기

```java
package game.baseball;
/*
 * for문 안에서 break문을 만나면 for문을 빠져나가는 것이고
 * if문 안에서 return을 만나면 메소드를 빠져나간다.
 */

public class Account1 {

	public String account(String user) {
		//문자열의 길이를 체크하고 싶을 때
		//if문 안에 return이오면 메소드를 탈출한다.(return을 반환하고 메소드를 더이상 진행하지 않는다.)
		if (user.length()!=3) {
			return "세자리 숫자를 입력하시오.";
		}//end of if		
		int temp = Integer.parseInt(user);
		int my[] = new int[3];
		my[0] = temp/100;
		my[1] = (temp%100)/10;
		my[2] = temp%10;
		 
		for(int x=0;x<3;x++) {
			System.out.println(my[x]+"");
		}//end of for
		return "0스트라이크 2볼";
	}//end of account 
	
	public static void main(String[] arg) {
		Account1 al = new Account1();
		String result = al.account("25687");
		System.out.println("1.256:"+result);
	}
	
}

```

if문 안에 return값이 오면 if가 true 일때 return값을 반환하고 메소드를 더 진행하지 않는다.

### Break를 이용한 for문 나가기

```java
package book.ch4;

public class ForTest2 {

	public static void main(String[] args) {
		
		for(int i=0;i<5;i++) {
			if(i==1) break;
			//0은 if를 통과해서 출력되고 1은 break걸려서 for문을 빠져나가 10번으로 진행된다.
		  System.out.println(i);
		}//end of for
		
		System.out.println("여기");
	}

}

```

for 문 안의 if조건이 true가 되면 for문이 종료된다.

### 주소 개념 인스턴스 변수 응용

```java
package game.baseball;

class Param{
	int ival;
}

public class TestParam {
	void effectParam(Param p) {
		p = new Param();//주소 재정의 : 주소변경 abcd ->1234
		p.ival = 500;//1234p.ival=500;
		System.out.println("sub ival = "+p.ival);
	}

	public static void main(String[] args) {
		//내 안에 있는 메소드 이지만 static영역에서 호출하려면 인스턴스화 필수
		TestParam tp = new TestParam();
		//effectParam메소드를 호출 할 예정인데 메소드에 파라미터 Param타입이 들어있음.
		//그래서 인스턴스화 함. 인스턴스화 하지 않으면 가리키는 객체는 없이 타입만 있는 것이다.
		Param p = new Param();//abcd p
		//main메소드 안에서 선언했기에 지변의 성격을 갖는다.
		//그 클래스에 선언된 ival변수는 처음에 0이였으나 아래 초기화를 통해 100이 담겼다.
		p.ival = 100;//abcd p
		//내 안의 메소드를 호출 하는데 이때 18번에 정의한 주소번지를 넘김
		tp.effectParam(p);//값이 아닌 참조에 의한 호출이다. abcd주소를 주엇지만 1234로 이사를감
		System.out.println("main ival = "+p.ival);//abcd p 출력
 
	}
}
```

main 메소드에서 p라는 주소로 똑같은 우편물 2개를 보냈지만 main우체국에서 집하되어 보내진 우편물이 있고, TestParam 우체국에서는 우편물을 이사간 주소보낸다. 이때 main 우체국은 변화한 주소를 모르기때문에 24번에서 ival값이 변화되었어도 영향을 받지않는다. 

후기  
새로운 개념을 배워서 이해하 과정도, 내 주신 문제를 코드로 작성해서 실행하는 결과물도 점점 재미를 붙여가고있다. 앞으로도 잘 따라갈 수 있길!


---
description: 2020.08.07 - 8일차
---

# 8 Day - 야구 숫자 게임 구현하기

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

```java
package book.ch4;

public class ForTest2 {

	public static void main(String[] args) {
		
		for(int i=0;i<5;i++) {
			if(i==1) break;
			System.out.println(i);//0은 if를 통과해서 출력되고 1은 break걸려서 for문을 빠져나가 10번으로 진행된다.
		}//end of for
		
		System.out.println("여기");
	}

}

```

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


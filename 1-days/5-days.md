---
description: 2020.08.04 - 5일차
---

# 5 Days - 조건문과 반복문, 연산자활용

### 학습목표

* 조건문과 반복문을 자유롭게 사용할 수 있다.

```java
package book.ch4;

public class ForTest {

	public static void main(String[] args) {
		//for(int i=1; i<=3; i=i+1) {
			//System.out.println(i);//1,2,3	
		int sum = 0;
		for(int i=1; i<=5; i= i+1) {
			if(i%2==0) {
				System.out.println(i);//2,4
				sum+=i;
			}			
		}
		System.out.println(sum);//6
	}

}

```

```java
package book.ch4;

import javax.swing.JOptionPane;
//모든 클래스는 Object로 부터 상속받아서 만들어진 것이다.
public class IfTest extends Object//상속(의존적), 부모객체
                                   {

	public static void main(String[] args) {
		//사용자로부터 점수를 입력 받고, 점수 변수를 선언하세요.
		//"점수를입력하세요."=객체
		//String보다 상위이다.
		String score = JOptionPane.showInputDialog("점수를 입력해주세요.");//요청=파라미터
		//score는 string타입이다.
		//범위가 int이므로 비교값도 int로 형변환 해야한다. 타입이 다르면 비교가 불가능하므로
		int score2 = Integer.parseInt(score);
		//너 90점 이상이니?
		//if()괄호 안에는 T나 F로 결과가 나오는 묻는값이 나와야한다. 단항연산자만은 있을 수 없다.
		//90은 상수
		if(score2>=90) {
			System.out.println("당신은 A학점을 받았습니다.");//응답			
		}
		//else if는 조건식이 있다. else는 조건식이 없이 그냥 나머지를 말한다.
		//위의 if에서 90점 이상은 참으로, 내려오지 않기때문에 굳이 &&조건을 쓰지 않아도 된다.
		else if(score2>=80) {			
			System.out.println("당신은 B학점을 받았습니다.");//응답
		}
		else if((score2>=70)&&(80>score2)) {			
			System.out.println("당신은 C학점을 받았습니다.");//응답
		}
		else if((score2>=60)&&(70>score2)) {			
			System.out.println("당신은 D학점을 받았습니다.");//응답
		}
		else if((score2>=50)&&(70>score2)) {			
			System.out.println("당신은 F학점을 받았습니다.");//응답
		}
		else {
			System.out.println("당신은 낙제하였습니다.");
		}
		//System.out.println(score2);
		

	}

}

```

```java
package book.ch4;

public class Test_forif {

	public static void main(String[] args) {
		int sum1 = 0;
		int sum2 = 0;
		
		for(int i=1; i<=10; i=i+1) {
			if(i%2==0){
				sum1 = sum1+i;//sum1+=i					
			}//if종료
			else {
				sum2 = sum2+i;//sum2+=i
			}//else종료			
		}//for종료
		System.out.println("짝수의 합 = "+sum1);
		System.out.println("홀수의 합 = "+sum2);

	}

}

```


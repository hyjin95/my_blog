# p491~4 Exception 예외처리

```java
package book.ch14;

public class p491 {
	
	public static void main(String[] args) {
		int arr[] = new int[3];//0,1,2
		try {
			for(int i=0;i<=3;i++) {//0,1,2,3 탈출
				System.out.println("//////////////////");//어디서 탈출햇는지 보기 위함
				arr[i] = i;//arr[3] = 3; ArrayIndexOutofBoundsException 발생
				System.out.println("//////////////////");//어디서 탈출했는지 보기 위함
				System.out.println(arr[i]);
			}
		}catch(Exception e) {
			System.out.println(e.toString());
			System.out.println(e.getMessage());
			e.printStackTrace();//라인번호도 같이 출력, throws한 경우도 출력
		}
	}
}
```

```java
package book.ch14;

public class p491_2 {
	
	public static void main(String[] args) {
		int arr[] = new int[3];//0,1,2
		try {
			for(int i=0;i<=3;i++) {//0,1,2,3 탈출
				System.out.println("//////////////////");//어디서 탈출햇는지 보기 위함
				arr[i] = i;//arr[3] = 3; 여기서 ArrayIndexOutofBoundsException 발생
				System.out.println("//////////////////");//어디서 탈출했는지 보기 위함
				System.out.println(arr[i]);
			}
		}catch(ArrayIndexOutOfBoundsException ae) {
			System.out.println("ArrayIndexOutBoundException : "+ae.toString());//여기서 그물에 걸린다.
		}catch(Exception e) {//여기까지 실행되지 않는다. 윗 그물에 걸렸으므로.
			System.out.println(e.toString());
			System.out.println(e.getMessage());
			e.printStackTrace();//라인번호도 같이 출력, throws한 경우도 출력
		}
	}
}
```

```java
package book.ch14;

public class p491_3 {
	
	public static void main(String[] args) {
		int arr[] = new int[3];//0,1,2
		try {
			for(int i=0;i<=3;i++) {//0,1,2,3 탈출
				System.out.println("//////////////////");//어디서 탈출햇는지 보기 위함
				arr[i] = i;//arr[3] = 3; 여기서 ArrayIndexOutofBoundsException 발생
				System.out.println("//////////////////");//어디서 탈출했는지 보기 위함
				System.out.println(arr[i]);
			}
		}catch(ArrayIndexOutOfBoundsException ae) {
			System.out.println("ArrayIndexOutBoundException : "+ae.toString());//여기서 그물에 걸린다.
		}catch(Exception e) {//여기까지 실행되지 않는다. 윗 그물에 걸렸으므로.
			System.out.println(e.toString());
			System.out.println(e.getMessage());
			e.printStackTrace();//라인번호도 같이 출력, throws한 경우도 출력
		}finally {//제일 마지막에 추가할수 있는 예약어
			System.out.println("예외가 발생하더라도 무조건 실행된다. 단, 가상버신과의 연결고리가 끊기면 예외");
		}
	}
}
```

```java
package book.ch14;

public class p491_4 {
	
	//메서드 오버로딩 조건에 예외처리를 던지고 안던지고 하는 문제는 영향이 없다.
	public void methodA() throws ArrayIndexOutOfBoundsException{
		int arr[] = new int[3];//0,1,2
		for(int i=0;i<=3;i++) {//0,1,2,3 탈출
			System.out.println("//////////////////");//어디서 탈출햇는지 보기 위함
			arr[i] = i;//arr[3] = 3; 여기서 ArrayIndexOutofBoundsException 발생
			System.out.println("//////////////////");//어디서 탈출했는지 보기 위함
			System.out.println(arr[i]);
		}		
	}
	
	public void methodA(int i) throws ArrayIndexOutOfBoundsException, Exception{
		
	}
	
	public static void main(String[] args) {
		p491_4 p4 = new p491_4();
		//p4.methodA();에러발생
		try {
			p4.methodA();
		}catch(ArrayIndexOutOfBoundsException ae) {
			System.out.println("여기 : "+ae.toString());//여기서 그물에 걸린다.
		}
	}
}
```




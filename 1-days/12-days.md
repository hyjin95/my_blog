---
description: 2020.18.13 - 12일차
---

# 12 Days -

```java
package ocjp.basic;
//배열의 시작 인덱스는 항상 0이다.[0]
//a2의 주소번지와 a2[0]방의 주소번지가 같기때문이다.
public class IntArray {
	void a2Print(int[] a) {
		
		for(int i=0;i<2;i++) {//배열의 크기=2, 2개만 보여줘
	  //for(int i=0;i<a.length;i++) {//a.length는 참조형이다.
			System.out.println(a[i]);//0 1
		}//end of for
		
		//개선된 for문, 너가 가진거 다 보여줘
		for(int b:a) {//a가 가진거 다 보여줘 a2[10]이라면 10개 값이 나온다.
			System.out.println(b);
		}
	}//end of a2Print

	public static void main(String[] args) {
		int i, j = 0;//j만 초기화 된것.
		i = 2;
		System.out.println(i+", "+j);
		int x[], y = 0;//y=int 라서 오류가 없고
		//int[]a, b = 0;//b=b[] 라서 오류가 생긴다.
		int[]a2, b2;//배열 두개 선언		
		//선언할때에는 반드시 [ ]가 있어야 하지만 생성할때에는 생략한다.
		//파라미터 자리에 배열을 넘길 수 있다. -> 클래스 활용
		a2 = new int[1];//값 2개 0 0
		b2 = new int[3];//값 3개 0 0 0 

		IntArray ia = new IntArray();
		ia.a2Print(a2);//a2[2]는 
	}
}
```


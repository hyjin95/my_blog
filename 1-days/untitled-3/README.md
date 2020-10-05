---
description: 2020.10.05 - 35일차
---

# 35 Days - Map, List, iterator, syncronized 대기실 구현

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### API

* CollectionFramework
* 패키지 -&gt; interfaces, classes, exceptions
* interfaces를 확인하는 경우에는 추상클래스를 먼저 확인한다.
* 보통 해당 클래스,인터페이스의 생성자, 메서드, 필드를 확인하는 용도

### interface 인스턴스화

* 인터페이스 변수 = new 구현체클래스\( \); - 가장 재사용성이 높다. 권장사항 - 상속을 받는 구조가 아니므로 결합도가 낮고 독립적이다. - 단위테스트, 통합테스트 가능
* 추상클래스 변수 = new 구현체클래스\( \); - 상속을 하는 구조이므로 독립적이지 않다. - 부모의 추상메서드를 강제로 오버라이딩해야한다.
* 구현체클래스 변수 = new 구현체클래스\( \); - 변수\(구현체클래스\)가 제공해주는 변수, 메서드만 사용가능

### 제네릭 &lt; &gt;

* List&lt;Object&gt;, List&lt;Element&gt; = List&lt;E&gt;
* 최상위 단위는 Object이지만 최근 여러 종류의 원소, 집합의 의미로 Element=E를 사용한다.
* 의미적으로 Element가 Object보다 크다.

## 필기

### List 인터페이스

* 대표적인 인터페이스 중 하나
* 배열형태의 자료 구조
* 대표적인 구현체클래스 : ArrayList, Vector
* ArrayList - 동기화를 지원하지 않는다. - 읽기, 쓰기가 빠르다. - 싱글스레드에서 안전하다.
* Vector - 동기화 지원, data의 안전성과 신뢰성이 높다. - 읽기, 쓰기가 느리다. - 동기화를 하면 시간이 더 걸린다. 조건을 비교하므로, 순서를 따져 대기시킨다. - 멀티스레드에서 안전하다.

### List&lt;타입&gt;

```java
public class MapTest {

	void methodA(List<Button> li) {}//권장사항O
	void methodB(ArrayList<Integer> al) {}//권장사항X, ArrayList는 싱글스레드용으로 경합이 벌어질때 사용하면 버그의 원인이 될 수 있다.
	void methodC(Vector<String> v) {}//권장사항X
	void methodD(Vector<Object> v) {}//권장사항X

	public static void main(String[] args) {
		MapTest mt = new MapTest();
		ArrayList<Integer> al = new ArrayList<>();
		Vector<String> v = new Vector<>();
		List<Button> li = new ArrayList<>();
		List<Object> li2 = new ArrayList<>();
		mt.methodA(li);//li만 가능
		mt.methodB(al);//al만 가능
		mt.methodC(v);//v만가능
		mt.methodD(v);//불가능
```

* 더 작은 타입은 더 큰 타입의 안에 들어갈 수 있다.
* 타입이 더 크더라도 제네릭타입이 다르면 성립하지 않는다.
* 17번 : 동일한 Vector타입이지만 제네릭타입이 달라 성립하지 않는다.            

### Map 인터페이스

* Map&lt;key, value&gt;
* 대표적인 인터페이스 중 하나
* 검색속도가 가장 빠른 인터페이스
* 대표적인 구현체 클래스 : HashMap, HashTable
* key : 값을 읽을\(꺼낼\) 수 있게 해주는 중복불가능한 key
* ORM\(ObjectRelationMap\)솔루션 중 하나인 MyBatis\(오픈소스\)에서 많이 사용된다.

### List 값유지, Map 검색

```java
		//웹서비스 or 모바일 서비스시에는 우선순위가 높은 편
		//단점 : 순서가 없어 차례를 맞출 수 없다.
		List<Map<String,Object>> mapList = new ArrayList<>();//map에담긴 정보 유지하기
		Map<String, Object> map = new HashMap<>();
		Map<String, Object> map2 = new Hashtable<>();
		//map.put(key, value);
		map.put("one", 10);
		map.put("two", "가산동");
		map.put("three", 3.14);
		mapList.add(map);
		
		map = new HashMap<>();
		map.put("one", 20);
		map.put("two", "창동");
		map.put("three", 3.15);
		mapList.add(map);
		
		map = new HashMap<>();
		map.put("one", 30);
		map.put("two", "방학동");
		map.put("three", 5.18);
		mapList.add(map);
```

```java
		//one,two,three는 컬럼, 뒤의 value는 row가 된다.
		//map에 있는 정보를 출력해보시오.
		Object obj[] = map.keySet().toArray();//1줄로 나타내보기
		Set<String> set = map.keySet();//2줄로 나타내보기 -1, Set클래스에서 toArray를 제공해준다.
		Object obj2[] = set.toArray();//2줄로 나타내보기 -2
		for(int i=0;i<obj.length;i++) {
			String key = obj[i].toString();
			System.out.println(map.get(key));
		}
		//ArrayList에 있는 정보를 출력해보시오.
		for(int j=0;j<mapList.size();j++) {
			Map rmap = mapList.get(j);
			Object keys[] = rmap.keySet().toArray();//1줄로 나타내보기
			for(int i=0;i<obj.length;i++) {
				String key = keys[i].toString();
				System.out.println(rmap.get(key));
			}
		}
	}}
```

### syncronized

```java
public class MapTest {
	synchronized void m() {}//동기화 메서드
	void m2() {
		//synchronized { }//부분동기화
	}
```

* 스레드 동기화 메서드
* 멀티스레드의 경우 동기화가 이루어지지않는 상태로 진행하게되면 스레드끼리 공유하는 data의 안정성과 신뢰도를 보장할 수 없다.
* 여러 스레드가 하나의 자원을 사용할때 현재 데이터를 사용하는 스레드를 제외하고는 다른 스레드들은 데이터에 접근 할 수 없도록 막는 개념 : block, unblock
* syncronized예약어를 과도하게 사용하게되면 성능저하가 일어난다.

### 자료구조에서 값을 꺼내는 인터페이스

* enumeration  - 웹 에서 사용 - 제거메서드가 없다.
* iterator - 자바에서 사용 - 제거메서드가 있다.




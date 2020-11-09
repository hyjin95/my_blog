---
description: 2020.10.05 - 35일차
---

# 35 Days - Map, List, iterator, enumeration, syncronized, 대기실구현,  Container, content, tabbed

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

### 기본 Layout

* JFrame : BorderLayout
* JPanel : FlowLayout

## 필기

### List 인터페이스

* 배열형태의 자료 구조
* 대표적인 구현체클래스 : ArrayList, Vector
* **데이터가 뭉텅이로 저장**된다. 저장속도가 빠르다.
* **데이터범위를 순차적으로 표시할때** 사용된다. 
* **데이터 특징이 포지션별로 존재할때 유리**하다.
* **ArrayList** - 동기화를 지원하지 않는다. - 읽기, 쓰기가 빠르다. - 싱글스레드에서 안전하다.
* **Vector** - 동기화 지원, data의 안전성과 신뢰성이 높다. - 읽기, 쓰기가 느리다. - 동기화를 하면 시간이 더 걸린다. 조건을 비교하므로, 순서를 따져 대기시킨다. - 멀티스레드에서 안전하다.      

### Map 인터페이스

* **Map&lt;key, value&gt;** : 데이터를 키와 값으로 관리
* **검색속도가 가장 빠른** 자료구조 인터페이스
* 데이터가 저장될때 빈공간을 찾아 저장된다.
* **저장할 데이터가 unique한 key값을 가질때** 사용된다. 
* **빈번한 검색과, 특정데이터를 순간마다 캐치**해야할때 유리하다.
* 대표적인 구현체 클래스 : HashMap, HashTable
* key : 값을 읽을\(꺼낼\) 수 있게 해주는 중복불가능한 key, 구분자 역할로 List보다 검색효율이 높다.
* **ORM**솔루션 중 하나인 MyBatis\(오픈소스\)에서 많이 사용된다.
*  데이터 추가 : map.put\(key값, value값\);
*  데이터 추출 : map.get\(key값\);
* **HashMap**

  - **동기화를 지원하지 않는다.**  
  - Map&lt;key타입, value타입&gt; map = new HashMap&lt;&gt;\(\);  
  - 동기화를 지원하지 않는다.  
  - 빠르다.  
  - 싱글스레드에서 안전하다.  
  - 하나의 null key, n개의 null value 허용  
  - Fail-Fast Iterator 반환

* **HashTable** - **동기화 지원** - 느리다. - 멀티스레드에서 안전하다. - null 불허 - Enumeration 반환

### Enumeration&Iterator

* 자료구조\(Collection\)에 저장된 값에 접근하거나 꺼낼때 사용되는 인터페이스
* **enumeration**  - 웹 에서 사용 - 제거메서드가 없다. - hasMoreElements\( \) : 남은 데이터가 있으면 true, 없으면 false 반환. while문에서 사용 - nextElement\( \) : 자료구조의 다음 데이터 반환
* **iterator** - 자바에서 사용 - 데이터 추출 방식을 표준화하여 코드의 재활용이 가능하다. - remove\( \) : 제거메서드가 있다. - hasNext\( \) : 남은 데이터가 있으면 true, 없으면 false 반환. while문에서 사용 - next\( \) : 자료구조의 다음 데이터 반환 - Fail-Fast Iterator : 다른 스레드에서 해당 스래드 자료를 수정하려고 하면                                          ConcurrentModificationException을 발생시켜 일관성을 보장하는 것

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

### ORM&Room

* ObjectRelationMap
* 데이터베이스와 객체지향 프로그래밍 언어간에 호환되지 않는 데이터를 변환해주는 프로그래밍 기법
* DB테이블과 매핑되는 객체를 만들어 그 객체에서 DB를 관리하는 것
* Room  - SQLite위에 추상화 계층을 제공하는 라이브러리 - 적은 양의 정형화된 데이터를 로컬에서 캐시와 같이 데이터를 유지하는 것 - 장치가 네트워크에 액세스 할 수 없는 상태라도 계속 탐색이 가능하다. - 이후에 온라인 상태가 되면 그동안의 모든 컨텐츠 변경 내용이 서버에 동기화 된다.

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

### List 유지, Map 검색

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

* 계속 put되면서 사라지는 map의 값을 유지하려면 List를 사용한다.
* map을 담기위해 map타입의 List를 ArrayList로 생성한다.
* map의 제네릭의 key는 String타입이고, 값은 여러종류를 담기위해 Object타입으로 한다.

### List의 map값 

![List&amp;lt;Map&amp;gt;](../../../.gitbook/assets/1%20%2815%29.png)

* n개인 key를 Object타입의 배열에 담는다. - 소유주.keySet\( \)함수를 이용해 map의 key값을 set한다. - 소유주.toArray\( \)함수를 이용해 배열에 set한다. - 소유주.keySet\( \).toArray\( \);
* n개의 key를 갖는 n개의 map을 List에 담아 유지한다. - List&lt;Map&lt; , &gt;&gt;

```java
		//one,two,three는 컬럼, 뒤의 value는 row가 된다.
		Object obj[] = map.keySet().toArray();//1줄
		Set<String> set = map.keySet();//2줄-1
		Object obj2[] = set.toArray();//2줄-2
		for(int i=0;i<obj.length;i++) {
			String key = obj[i].toString();
			System.out.println(map.get(key));//단위테스트
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

* Set클래스가 제공해주는 toArray\( \) 메서드를 이용해 추출할 수 있다.
* 2번 : 권장사항
* 5번 : 검색되는 만큼 반복
* 6번 : key를 map에서 받아온다.
* 10번 : ArrayList의 방 갯수만큼 반복
* 11번 : ArrayList를 검색할 Map선언, 생성. j=List 방 번호
* 12번 : 11번에서 검색된 방의 key값을 담은 배열 선언, 생성
* 13번 : 검색되는 만큼 반복
* 14번 : 방마다 key를 꺼내서 받는다.
* 15번 : 14번에서 꺼낸 key로 rmap검색, 출력 반복

{% page-ref page="map-list.md" %}

## 대기실&단톡방 구현

### Container

![](../../../.gitbook/assets/1%20%2816%29.png)

* javax.swing 패키지에서 제공되는 화면 클래스
* Object를 상속하는 Window, JFrame의 부모 클래스
* window\(창\)의 역할을 하고, 컨테이너가 될수 있는 컨포넌트는 JFramem JPanel, JApplet이 있다. - JFrame, JApplet : 최소화, 최대화, 닫기 버튼을 갖는 윈도우 구조의 최종 컨테이너 - JPanel : 투명 윈도우로 중간 컨테이너의 역할로서 JFrame이나 JApplet에 추가된다.
* JPanel을 JFrame에 추가할때 사용하는 클래스  - getContentPane\( \)

### getContentPane\( \)

* java GUI library Swing에서 제공하는 JFrame의 객체 메서드
* contentPane : 일반 컴포넌트를 가질 수 있는 패널 - 프레임 객체의 getContentPane\( \)메서드를 이용해 얻을 수 있다.
* Container con = this.getContentPane\( \); - this에 연결된 패널을 얻어\(알아내서\) 패널\(con\)을 붙인다.

### JTabbedPane\( \)

* 위에서 얻은 패널을 탭 형식으로 프레임에 보여주는 메서드

### Login

* 로그인 성공시 새로운 clientVer2클래스로 연결한다.

### ChatClientVer2

![](../../../.gitbook/assets/2%20%2811%29.png)

* 패널과 contentPane을 이용해 대기실과 단톡방 화면을 구현한다.

### WaitRoom

* 대기실
* JPanel 상속

### MessageRoom

* 단톡방
* JPanel 상속

{% page-ref page="undefined/" %}

후기 : 내일을 위해 구글에 검색해가면서 열심히 복습하자! 내일은 코로나때문에 쉬었던 운동을 다시 시작하려고 한다. 건강도 공부도 열심히 챙겨야지 성실하게 살자!


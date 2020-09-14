# 자바 && 쿼리문

### INSERT

```java
	/*
	 * @param b_no : 자동 채번하는 시퀀스 활용하기
	 * @param b_name 
	 * @param b_img 
	 * @param publish
	 * @param author
	 */
	//b_no는 쿼리문에서 select문으로 시퀀스를 사용하므로 파라미터에 필요없다.
	public int bookInsert(String b_name, String b_img, String publish, String author) {
		return 0;
	}
```

### INSERT VALUES에 SELECT문 넣기

```java
package design.book;

import java.util.ArrayList;
import java.util.List;

public class BookTest {
	
	/*
	 * @param bVO
	 * @return : 0이면 등록실패, 1이면 등록성공 -> 사용자에게 피드백 줄 수 있다.
	 * query:INSERT INTO book(b_no, b_name, publish, author, b_img)
	 *       	VALUES(SELECT seq_book_no.nextval FROM dual, '자바의 정석', '도우출판사', '남궁성', 'java.png')
	 * b_no = SELECT문
	 */
	public int bookInsert(BookVO bVO) {
		return 0;
	}
	public int bookUpdate(BookVO bVO) {
		return 0;
	}
```

* INSERT할 값에 SELECT문을 넣을 수 있다.

### DELETE

```java
	/*
	 * @param deptno
	 * @return
	 * query : DELETE FROM book WHERE b_no=?
	 */
	public int bookDelete(int deptno) {
		return 0;
	}
```

### 서브쿼리 & List타입

```java
	/*
	 * SELECT ename, sal FROM emp WHERE sal >= (SELECT sal FROM emp WHERE empno =:x) --7566
	 */
	public List<BookVO> bookList(int b_no) {
		//List<BookVO> bList = null;->nullpointexception
		List<BookVO> bList = new ArrayList<>();
		return bList;
	}
```


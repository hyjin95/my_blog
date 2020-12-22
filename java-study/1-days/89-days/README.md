---
description: 2020.12.22 - 89일차
---

# 89 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 게시판

### 오라클을 경유하는 경우

* SELECT - 반환 값을 List&lt;&gt;에 담아 유지하고 forward로 응답페이지에 넘겨야한다. - @RequestMapping\("board/boardList.sp"\) : annotation, spring-boot - PropertiesMethodNameResolber : xml,  spring
* INSERT \| UPDATE \| DELETE

  - 트랜잭션의 처리 대상  
  - 결과 페이지가 boardList.jsp로 향하게 된다. : board/boardList.jsp  
  - 커밋과 롤백의 대상

### 경유하지 않는 경우

* SELECT,  조건 검색 - 페이지가 처음 열릴 때 DB를 경유하지 않는다.
* INSERT \| UPDATE \| DELETE - 트랜잭션의 처리 대상 - 결과 페이지가 boardList.jsp로 향하게 된다. : board/boardList.jsp - 커밋과 롤백의 대상

### return Type과 url

* void : webapp하위의 jsp에 접근한다.
* ModelAndView, String : WEB-INF하위의 jsp에 접근한다.
* WEB-INF의 jsp에 접근하려면 반드시 Contorller를 거쳐야 하는 것이다. webapp에 있는 jsp는 url에서 해당 경로로 바로 접근할 수 있다.

## Spring 게시판 : 상세보기

{% page-ref page="spring.md" %}

## Spring 게시판 : 새글 & 덧글 작성하기

### 새글 쓰기

```javascript
<form>
    <input type="hidden" name="bm_no" id="" value="0"/>
    <input type="hidden" name="bm_gorup" id="" value="0"/>
    <input type="hidden" name="bm_step" id="" value="0"/>
</form>
```

* value가 0 이라면 새글 작성하기 업무이다.

### 댓글 쓰기

* 그룹번호는 채번하지 않는다. 
* 선택된 댓글을 작성할 게시물에 대한 그룹번호를 가져야 하기 때문이다.

```javascript
<form>
    <input type="hidden" name="bm_no" id="" value="<%=rMap.get("BM_NO") %>"/>
    <input type="hidden" name="bm_gorup" id="" value="<%=rMap.get("BM_GROUP") %>"/>
    <input type="hidden" name="bm_step" id="" value="<%=rMap.get("BM_STEP") %>"/>
</form>
```

* JS
* &lt;form&gt;전송을 사용한다.
* hidden속성을 활용해 선택한 게시물의 PK,그룹번호, 글 번호를 담아 전송해야한다. value속성으로 값을 가져온다.
* value가 &gt; 0 이라면 댓글 작성하기 업무이다.

```markup
	<update id="updateStep" parameterType="map">
		UPDATE board_master_t
		   SET bm_step = bm_step + 1
		 WHERE bm_group = 35
		   AND bm_step > 1;
	</update>
```

* SQL
* 끼어드는 글이 존재하므로 위와 같은 update구문이 필요하다.
* 배달사고가 잘 발생할 수 있으므로 파라미터가 잘 넘어오는지 확인해야한다.

### 공통부분 : 쿼리

```markup
  <select id="seqBoardNo" parameterType="map" resultType="map">
		SELECT SEQ_BOARD_NO.nextval FROM dual
	</select>
	
	<insert id="boardMInsert" parameterType="map"><!-- 끝에 세미콜론 들어가면 안됨!!! -->
		INSERT INTO board_master_t(bm_no,bm_writer,bm_title,bm_content,bm_email,bm_date,bm_group,bm_pos,bm_step,bm_pw)
       	 VALUES(1,'작성자1','제목1','내용1','test1@hot.com','2020-12-08',0,0,0,'123')
	</insert>	  
	
	<insert id="boardSInsert" parameterType="map"><!-- 끝에 세미콜론 들어가면 안됨!!! -->	   
		INSERT INTO board_sub_t(bm_no, bs_seq, bs_file, bs_size)
		   VALUES(2,2,'test2.txt',10)
	</insert>	  
```

* "seqBoardNo"와 "boardMInsert"는 MDao.java에서, "boardSInsert"는 SDao.java에서 호출된다.
* Logic에서는 MInsert와 SInsert의 결과값\(int\)를 가지고 둘다 insert가 성공\(1\)하면 커밋, 응답제출 해야한다. = 트랜잭션

### 새글과 댓글 구분

* 컬럼  - bm\_no : 둘 다 채번해야 한다. - bm\_group :  새글이라면 둘다 채번하지만 댓글이라면 이미 갖고 있을테니까 필요가없다.

## Android Studio

### menu resource생성

![](../../../.gitbook/assets/2%20%2875%29.png)

* res 우클릭 &gt; New &gt; Android Resource File

![](../../../.gitbook/assets/1%20%2896%29.png)

* Resource type : Menu
* 해당 type으로 리소스 파일을 생성하면 res에 자동으로 menu폴더가 생성된다.


# Debug note

## JDBC Error Note

* 부적합한 식별자, SQLException\
  \- oracle SQL오류, 오타가 있을 수 있다.
* ename은 GROUP BY 표현식이 아닙니다.=JAVA SQLException --ora-000979 에러는 무조건 오라클 쿼리문 에러인 것이다.

## 웹 페이지 Error

* 404 : 경로 오류
* 405 : Not Allowed, 허용불가\
  \- 메서드가 post방식이면 쿼리스트링으로 불러올 수 없다.
* 500 : 내부 서버 오류, 서버 측 파일 오류

### jsp접근시 다운로드되는 현상

```markup
<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
	</dependency>
```

* jsp에 접근시 jsp가 다운로드 되는 현상\
  \- jsp의 mime Type을 읽을 수 없을때에 브라우저는 다운로드 시켜버린다.\
  \- dependency태그를 활용해 jsp문서를 읽을 수 있는 객체를 주입하자.




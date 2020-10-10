# SQLMapEmpDao - mybatis

## MapperConfig

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
	<environment id="development">
	<transactionManager type="JDBC"/>
	<dataSource type="POOLED">
		<property name="driver" value="oracle.jdbc.driver.OracleDriver"/><!--드라이버 클래스 -->
		<property name="url" value="jdbc:oracle:thin:@192.168.123.7:1521:orcl11"/>
		<property name="username" value="scott"/><!--계정이름-->
		<property name="password" value="tiger"/><!--비밀번호-->
	</dataSource>
	</environment>
	
	</environments>
	<mappers>
		<mapper resource="oracle/mybatis/dept.xml"/>
		<mapper resource="oracle/mybatis/emp.xml"/>
	</mappers>
</configuration>
```

* DB연동 sql XML파일, mybatis의 설정 클래스
* 10번 : 사용할 DB제품의 드라이버 클래스 경로
* 11번 : URL - 사용하는 oracle.JDBC의 타입은 thin이다. - ip - port 번호 : oralce Listener의 포트번호는 1521이다. - 호스트이름
* 12,13번 : 계정이름, 비밀번호
* 19번 : &lt;mappers&gt;의 하위 엘리먼트 &lt;mapper&gt;태그 안에 resource속성으로 매퍼파일 경로를 지정한다.  - 매퍼파일이 늘어나면 추가한다.

## EmpVO.java

```java
package com.vo;

public class EmpVO {
	  private double etc_sum = 0.0;
	  private double salesman_sum = 0.0;
	  private double clerk_sum = 0.0;
	  
	public double getEtc_sum() {
		return etc_sum;
	}
	public void setEtc_sum(double etc_sum) {
		this.etc_sum = etc_sum;
	}
	public double getSalesman_sum() {
		return salesman_sum;
	}
	public void setSalesman_sum(double salesman_sum) {
		this.salesman_sum = salesman_sum;
	}
	public double getClerk_sum() {
		return clerk_sum;
	}
	public void setClerk_sum(double clerk_sum) {
		this.clerk_sum = clerk_sum;
	}
}
```

* 출력할 값들을 멤버변수로 하는 db연동 값 관리 VO클래스를 생성한다.
* 멤버변수의 값을 은닉하기 위한 클래스
* getter : 해당 값을 복사해서 반환하므로 무결성이 지켜진다.
* setter : 멤버변수을 변경할수 있는 메서드

## Emp.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.EmpMapper">

 <select id="getJobSumList" parameterType="int" resultType="Map"> 
   SELECT empno, ename, sal
	 FROM emp
 </select><!-- 왜 반환타입은 Map인데 List로 받을까? -->
 
 <select id="getJobSumList2" parameterType="int" resultType="com.vo.EmpVO">
   SELECT SUM(DECODE(job,'CLERK',sal,null)) clerk_sum
         ,SUM(DECODE(job,'SALESMAN',sal,null)) salesman_sum
         ,SUM(DECODE(job,'CLERK',null,'SALESMAN',null,sal)) etc_sum
	FROM emp
 </select>

</mapper>
```

* 서버 접속이 일어나면 MapperConfig.xml의 Mapper인터페이스를 통해 호출되는 SQL문이다. 
* 7-10번 : Sql문의 id는 getJobSumList, 파라미터 타입은 int, 반환타입은 Map이다.
* 12-17번 : 두번째 Sql문의 id는 getJobSumList2이다. - 각 이름별 연봉의 합을 나타내주는 테이블

## SqlMapEmpDao.java

### 선언부

```java
package oracle.mybatis;

import java.io.Reader;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.vo.EmpVO;

public class SqlMapEmpDao {
	//xml문서가 물리적으로 어디에 있는지를 알아야 한다.
	String resource ="oracle/mybatis/MapperConfig.xml";
	//myBatis에서 지원하는 클래스로
	SqlSessionFactory sqlMapper = null;
```

* 17번 : 사용할 xml경로 멤버변수에 생성
* 19번 : 연결통로 SqlSessionFactory 클래스 선언

### getJobSumList - 사원정보

```java
	//쿼리문의 아이디와 메서드 이름은 일치시켜서 통일을 해주는것이 좋다.
	//아이디만 갖고도 메서드를 찾을수 있도록
	public List<Map<String, Object>> getJobSumList(){
		List<Map<String, Object>> empList = null;
		SqlSession session = null;
		try {
			//물리적으로 떨어져있는 소스에서 필요한 정보를 Reader = 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			//build클래스로 sqlSessionFactory를 인스턴스할 수 있다.
			sqlMapper = new SqlSessionFactoryBuilder().build(reader);
			session = sqlMapper.openSession();
			empList = session.selectList("getJobSumList");			
			session.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		if(empList==null) {
			empList = new ArrayList<>();
		}
		return empList;
	}
```

* xml의 sql을 사용할 메서드이다.
* 4번 : 값을 담을 List타입 변수를 선언한다.
* 5번 : sql문 처리 요청객체 Sqls
* 8번 : Reader클래스로 멤버변수의 xml파일을 읽어와 그 안의 값을 사용할 수 있도록 한다.
* 10번 : sqlSessionFactory 인스턴스화, 생성
* 11번 : sqlSession 객체 생성
* 12번 : 만들어둔 List변수에 사용할 xml ID값을 담아 그 반환값을 담는다. - 값이 n개의 row이므로 selectList메서드를 사용한다.
* 17-19번 : 세션 종료
* 12-22 : nullPointException방지 구문

### getJobSumList2 - 부서별 연봉

```java
	public EmpVO getJobSumList2(){
		EmpVO reVO = null;
		SqlSession session = null;
		try {
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader);
			session = sqlMapper.openSession();
			reVO = session.selectOne("getJobSumList2");	
			session.close();			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		if(reVO==null) {
			reVO = new EmpVO();
		}
		return reVO;
	}
```

* 위 메서드와는 다른 xml을 활용할 메서드이다.
* EmpVO클래스를 활용해보자.
* 2번 : EmpVO 인스턴스변수를 선언한다.
* 3번 : sql문 처리 요청 객체 session을 선언한다.
* 4번 : 서버와 연동되는 부분이므로 예외처리를 한다.
* 5번 : Reader클래스로 xml의 값을 읽어온다.
* 6번 : Builder클래스로 SqlSessionFactory연결통로를 생성한다.
* 7번 : openSession메서드로 세션을 생성한다.
* 8번 : Empvo인스턴스 변수에 ID가 getJobSumList2인 sql문의 반환값을 담는다. - selectOne메서드 이므로 하나의 Object만 반환하지만 empVO클래스 안에 나뉘어 담긴다.
* 12-14번 : 세션 종료
* 15-17번 : nullPointException 방지

### main 메서드

```java
	public static void main(String[] args) {
		SqlMapEmpDao eDao = new SqlMapEmpDao();
		List<Map<String, Object>> empList = eDao.getJobSumList();
		System.out.println("empList.size = "+empList.size());
		for(int i=0;i<empList.size();i++) {
			Map rmap = empList.get(i);
			System.out.println(rmap);
			System.out.println(rmap.get("EMPNO"));
			System.out.println(rmap.get("ENAME"));
			System.out.println(rmap.get("SAL"));
		}//n개 row의 값이 다 나온다.
		}
	}
```

* 값을 출력하는 메인 메서드
* 2번 : 인스턴스화
* 3번 : 출력할 getJobSumList메서드의 반환값을 받을 List타입 변수 선언, 생성
* 4번 : 값이 들어왔는지 확인하는 단위테스트용 출력문
* 5번 : List안의 값을 모두 출력해보기 위해 List의 size만큼 for문안에서 출력문을 돌린다.
* 6번 : List안에 n개의 Map타입 객체들이 들어있어 Map타입 변수를 선언해 i번째 방의 Map을 담는다.
* 7번 : i번째 map의 모든 정보가 출력된다. 사원번호+이름+연봉
* 8-10번 : i번째 map의각 get메서드로 꺼낸 값만 출력된다.


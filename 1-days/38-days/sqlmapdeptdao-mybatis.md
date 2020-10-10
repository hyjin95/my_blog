# SQLMapDeptDao - mybatis

## MappingConfig.XML

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
	</mappers>
</configuration>
```

* DB연동 sql XML파일, mybatis의 설정 클래스
* 10번 : 사용할 DB제품의 드라이버 클래스 경로
* 11번 : URL - 사용하는 oracle.JDBC의 타입은 thin이다. - ip - port 번호 : oralce Listener의 포트번호는 1521이다. - 호스트이름
* 12,13번 : 계정이름, 비밀번호
* 19번 : &lt;mappers&gt;의 하위 엘리먼트 &lt;mapper&gt;태그 안에 resource속성으로 매퍼파일 경로를 지정한다.

## dept.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">
 <select id="getDeptList" parameterType="int" resultType="Map">
   select deptno, dname, loc from dept
 </select>
 
 <delete id="deleteDept" parameterType="int">
   delete form dept
 </delete>
</mapper>
```

* 서버 접속이 일어나면 MapperConfig.xml의 Mapper인터페이스를 통해 호출되는 SQL문이다.
* 6-8번 : selesct문은 getDeptLIst라는 id를 갖고, 파라미터 타입은 int, 반환타입은 Map이다.
* 10번 : delete문의 id는 deleteDept이고 파라미터 타입은 int, 삭제sql이므로 반환값은 없다.
* id를 만들때, 사용될 메서드의 이름과 같이 만들어 놓으면 사용하기에 편리하다.

## SqlMapDeptDao.java

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

public class SqlMapDeptDao {
	//xml문서가 물리적으로 어디에 있는지를 알아야 한다.
	String resource ="oracle/mybatis/MapperConfig.xml";
	//myBatis에서 지원하는 클래스
	SqlSessionFactory sqlMapper = null;
```

* import하기 위해서는 필요한 클래스들의 jar파일이 labraries에 존재하는지 확인해야한다.
* 부서의 갯수를 출력하는 Dao클래스
* mybatis 프레임워크를 이용해 sql문을 xml파일로 분리했다.
* 15번 : 이를 이용하기 위해서 xml문서의 물리적인 위치를 멤버변수로 선언, 생성되어야한다.
* 16번 : mybatis에서 제공하는 SqlSessionFactory클래스를 선언한다. - 서버와의 연결통로를 확보해주는 클래스.

### getDeptList 메서드

```java
	public List<Map<String, Object>> getDeptList(){
		List<Map<String, Object>> deptList = null;
		SqlSession session = null;
		try {
			//물리적으로 떨어져있는 소스에서 필요한 정보를 읽어와야한다. Reader<->Writer
			//Reader = 문서를 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			//build클래스로 sqlSessionFactory를 인스턴스할 수 있다.
			sqlMapper = new SqlSessionFactoryBuilder().build(reader);
			session = sqlMapper.openSession();
				deptList = session.selectList("getDeptList");			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		if(deptList==null) {
			deptList = new ArrayList<>();
		}
		return deptList;
	}
```

* Map제네릭 타입을 갖는 List타입 메서드 - List의 방안에 Map이 들어있다.
* 2번 : 반환값으로 사용할 변수 선언
* 3번 : 서버와의 연결에서 개발자의 sql문 처리 요청을 하는 객체인 SqlSession을 선언한다.
* 4번 : xml파일을 통해 서버와의 연결이 이루어지므로 예외처리를 한다.
* 7번 : 문서를 읽는 Reader클래스를 이용해 멤버변수에 선언된 XML파일의 값을 사용할 수 있게한다. - Reader 변수이름 = Resources.getResourceAsReader\(XML파일 경로\)
* 9번 : builder클래스를 이용해 sqlSessionFactory의 인스턴스를 Reader로 생성한다.
* 10번 : sqlSessionFactory가 생성된 다음에 SqlSession변수를 소유주.openSession메서드로 생성한다.
* 11번 : 생성해둔 List 변수에 열린세션중 id가 getDeptList인 sql을 사용해 값을 담는다.
* 14-16번 : 세션 종료
* 17-19번 : 만약 sql결과값이 비어서 deptList에 아무것도 담기지 않았다면 nullPoint가 일어날 수 있으므로 null인 경우에만, ArrayList로 생성해둔다.

### main 메서드

```java
	public static void main(String[] args) {
		SqlMapDeptDao dDao = new SqlMapDeptDao();
		List<Map<String, Object>> deptList = dDao.getDeptList();
		System.out.println("deptList.size = "+deptList.size());//4
	}
}
```

* 값이 제대로 담겼는지 확인하기위한 테스트 메서드
* 2번 : 인스턴스화
* 3번 : getDeptList의 반환값이 List타입이므로 List타입 변수를 하나 선언해, 담는다.
* 4번 : List.size출력


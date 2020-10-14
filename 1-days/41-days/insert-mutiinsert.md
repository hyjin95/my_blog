# insert, mutiInsert 구현

## Insert

```java
package oracle.mybatis;

import java.io.Reader;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.apache.log4j.Logger;

public class SqlMapDeptDao {
	
	//xml문서가 물리적으로 어디에 있는지를 알아야 한다.
	String resource ="oracle/mybatis/MapperConfig.xml";
	//myBatis에서 지원하는 클래스로
	SqlSessionFactory sqlMapper = null;
	
	Logger logger = Logger.getLogger(SqlMapDeptDao.class);
	
	public int deptInsert(int deptno, String dname, String loc){
		//select가 아니라면 커서를 조작하지않는다. 조회결과가 없으므로 list가 필요없다. 반환값이 0 or 1이다.
		int result = 0;//0이면 등록 실패, 1이면 등록 성공
		SqlSession session = null;
		try {
			//물리적으로 떨어져있는 소스에서 필요한 정보를 읽어와야한다. Reader<->Writer
			//Reader = 문서를 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			//build클래스로 sqlSessionFactory를 인스턴스할 수 있다.
			sqlMapper = new SqlSessionFactoryBuilder().build(reader);
			session = sqlMapper.openSession();
			Map<String,Object> pMap = new HashMap<>();
			pMap.put("deptno", deptno);
			pMap.put("dname", dname);
			pMap.put("loc", loc);
			result = session.insert("deptInsert", pMap);	
			session.commit();
			session.close();
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return result;
	}
	
	public static void main(String[] args) {
		SqlMapDeptDao dDao = new SqlMapDeptDao();
		List<Map<String, Object>> deptList = dDao.getDeptList();
		System.out.println("deptList.size = "+deptList.size());
		int result = dDao.deptInsert(50, "개발부", "부산");
		System.out.println("result = "+result);
	}
}
```

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">
 
 <insert id="deptInsert" parameterType="map">
 	insert into dept(detpno, dname, loc) values (#{item.deptno},#{item.dname},#{item.loc}) 
 </insert>
 
</mapper>
```

## multiInsert


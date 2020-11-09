# Update

## SqlMapDeptDao.java

### 선언부

```java
public class SqlMapDeptDao {	
	//xml문서가 물리적으로 어디에 있는지를 알아야 한다.
	String resource ="oracle/mybatis/MapperConfig.xml";
	//myBatis에서 지원하는 클래스로
	SqlSessionFactory sqlMapper = null;
	
	Logger logger = Logger.getLogger(SqlMapDeptDao.class);
	
	MyBatisCommonFactory mcf = new MyBatisCommonFactory();
	
	public SqlMapDeptDao() {
		sqlMapper = mcf.getSqlSessionFactory();
	}
```

### deptUpdate 메서드

```java
public int deptUpdate(DeptVO dVO) {
		int result = 0;
		SqlSession session = sqlMapper.openSession();	
		try {
			dVO= new DeptVO();
			dVO.setDeptno(41);
			dVO.setDname("개발2팀");
			dVO.setLoc("포천시");		
			result = session.update("deptUpdate", dVO);			
			session.commit();
			logger.info("result : "+result);	
		} catch (Exception e) {
			System.out.println(e.toString());
		} finally {
			session.close();
		}
		return result;
	}/////////////////////////////deptDelete////////////////////////////////
```

### dept.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">

 <select id="getDeptList" parameterType="int" resultType="Map">
   select deptno, dname, loc from dept
 </select>
  
 <insert id="deptInsert" parameterType="map">
 	insert into dept(detpno, dname, loc) values (#{item.deptno},#{item.dname},#{item.loc}) 
 </insert>
 
 <insert id="multiDeptInsert" parameterType="list">
 	insert all 
 	<foreach collection = "list" item="item" index="index" separator=" ">
 		into dept(deptno,dname,loc) values(#{item.deptno},#{item.dname},#{item.loc}) 
 	</foreach>
 	select * from dual
 </insert>
 
 <delete id="deptDelete" parameterType="int">
   delete from dept where deptno=#{value} <!-- int이므로 값=value -->
 </delete>
 
 <delete id="deptDelete2" parameterType="com.vo.DeptVO">
   delete from dept where deptno=#{deptno} <!-- DeptVO의 멤버변수(get,set으로 구현된) -->
 </delete>
 
 <delete id="deptDelete3" parameterType="map">
   delete from dept where deptno=#{deptno} <!-- map이므로 key값 -->
 </delete>
 
 <delete id="multiDeptDelete" parameterType="list">
   delete from dept where deptno in 
   <foreach open="(" close=")" collection = "array" item="item" index="index" separator=",">
 		#{item}
   </foreach>
 </delete>
 
 <resultMap id="rdvo" type="com.vo.DeptVO"> </resultMap>

 <parameterMap id="pdvo" type="com.vo.DeptVO"></parameterMap>
 
 <update id="deptUpdate" parameterType="com.vo.DeptVO">
   update dept set dname=#{dname}, loc=#{loc} where deptno=#{deptno}
 </update><!-- 조건이 deptno를 비교한 곳을 update하는 곳이므로 oracle에 해당 deptno가 생성, commit되어있어야한다. -->
 
</mapper>
```


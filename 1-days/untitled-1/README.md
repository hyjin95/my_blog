---
description: 2020.10.15 - 42일차
---

# 42Days - web, scope, sqlSession선언과 생성,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 FrameWork - MyBatis

## 복습

### 루트문서 dtd활용

### dept.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">
 
 <resultMap id="rdvo" type="com.vo.dept"> </resultMap>

 <parameterMap id="pdvo" type="com.vo.dept"></parameterMap>
</mapper>
```

### Configuration.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource = "com/util/Config.properties"/>	
	 
   <typeAliases>
 	  <typeAlias alias="dVO" type="com.vo.DeptVO"/>
   </typeAliases>
</configuration>
```


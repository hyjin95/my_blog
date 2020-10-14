# MyBatisZipCodeSearch - Log4j - zdos3

![Log4j : ZipCode.XML TRACE](../../../.gitbook/assets/log2.png)

### log4j.properties

```markup
#log4j.properties
log4j.rootCategory=info, stdout, file
log4j.debug=false
log4j.appender.stdout=com.p6spy.engine.logging.appender.StdoutLogger
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.ImmediateFlush=true
log4j.appender.stdout.Target=System.err

log4j.appender.stdout.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n


log4j.appender.file.DatePattern = '.'yyyy-MM-dd

#emp.xml이나 dept.xml, zipcode.xml의 namespace이름을 등록한다.
log4j.logger.oracle.mybatis.ZipCodeMapper = TRACE

log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n

log4j.logger.java.sql.Connection=INFO
log4j.logger.java.sql.Statement=INFO
log4j.logger.java.sql.PreparedStatement=INFO
log4j.logger.java.sql.ResultSet=INFO
```

* zipcode.XML이 실행될때마다 추적 로그를 작성한다.




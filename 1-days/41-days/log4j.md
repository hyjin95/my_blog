# Log4j

![logger &quot;&#xC5EC;&#xAE30;&quot;](../../.gitbook/assets/log1.png)

```java
package oracle.mybatis;

import java.io.FileInputStream;
import java.util.Properties;

import org.apache.log4j.Logger;
import org.apache.log4j.PropertyConfigurator;

public class Log4jTest {
	static Logger logger = Logger.getLogger(Log4jTest.class);

	public static void main(String[] args) {
		try {
			FileInputStream fis = new FileInputStream("src//log4j.properties");//properties문서 읽기
			Properties prop = new Properties();
			prop.load(fis);
			PropertyConfigurator.configure(prop);
			logger.info("여기");
		} catch (Exception e) {
			logger.info("Exception : "+e.toString());
		} finally {
			
		}
	}//////////////////////////end of main/////////////////////////
}
```

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
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n

log4j.logger.java.sql.Connection=INFO
log4j.logger.java.sql.Statement=INFO
log4j.logger.java.sql.PreparedStatement=INFO
log4j.logger.java.sql.ResultSet=INFO
```


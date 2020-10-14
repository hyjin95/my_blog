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


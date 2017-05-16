注：[源自Spring官方文档的DEMO](https://spring.io/guides/gs/rest-service/)
<font color="red">IDEA+MAVEN搭建该项目</font>
###编写pom.xml，自动添加依赖
<font color="red">利用MAVEN可以解决jar版本冲突问题</font>
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com</groupId>
  <artifactId>Spring</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>Spring Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.2.RELEASE</version>
  </parent>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.jayway.jsonpath</groupId>
      <artifactId>json-path</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <properties>
    <java.version>1.8</java.version>
  </properties>


  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

  <repositories>
    <repository>
      <id>spring-releases</id>
      <url>https://repo.spring.io/libs-release</url>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <id>spring-releases</id>
      <url>https://repo.spring.io/libs-release</url>
    </pluginRepository>
  </pluginRepositories>
</project>
```


###编写返回数据实体类
类路径：/src/main/java/hello/
<pre>
注释：
1.  id是greeting的唯一标识
2. context是内容
</pre>
```
package hello;

/**
 * Created by abb on 17-5-16.
 */
public class Greeting {

    private final long id;
    private final String context;

    public Greeting(long id, String context) {
        this.id = id;
        this.context = context;
    }

    public long getId() {
        return id;
    }

    public String getContext() {
        return context;
    }

}
```

###编写Controller类
类路径：/src/main/java/hello/
<pre>
注释：
1.  @RestController注解表明这个类是Controller类
2. @RequestMapping注解保证HTTP请求/greeting时能够被映射到greeting方法，可以指明访问方式：@RequestMapping(method=GET)/@RequestMapping(method=PUT)等
3.  @RequestParam绑定请求的参数，可设置默认值。即访问时URL若没有带参数，则参数值为默认值
4. 返回数据是一个Greeting对象，但Spring的 MappingJackson2HttpMessageConverter 会自动将Greeting实例对象转换为JSON数据
</pre>
```
package hello;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.atomic.AtomicLong;

/**
 * Created by abb on 17-5-16.
 * controller
 */
@RestController
public class GreetingController {

   private static final String template = "Helle, %s!";
   private final AtomicLong counter = new AtomicLong();

   @RequestMapping
   public Greeting greeting(@RequestParam(value="name", defaultValue="world!") String name){
      return new Greeting(counter.incrementAndGet(),
              String.format(template,name));
   }

}
```

###编写Application类
类路径：/src/main/java/hello/
<pre>
注释：
1.  @SpringBootApplication注解包括了@Configuration、@EnableAutoConfiguration、@EnableWebMVC、@ComponentScan
2. 想要访问项目必须先启动main方法，用Main方法中的 SpringApplication.run() 方法发布项目.使用这种方法在HTTP运行时将项目动态加载到TOMCAT中，而不是部署一个外部实例
</pre>
```
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * Created by abb on 17-5-16.
 */

@SpringBootApplication
public class Application {
    public static void main(String[] args){
        SpringApplication.run(Application.class, args);
    }
}
```

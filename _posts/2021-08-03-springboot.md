---
layout: post
title:  "SpringBoot 笔记"
date:   2021-08-03 21:44:25 +0800
categories: Spring
---
* 目录
{:toc}

## 原理初探
自动配置：

SpringBoot所有自动配置都是在启动的时候扫描并加载，spring.factories所有的自动配置类都在这里面，但是不一定生效，要判断是否成立，只要导入了对应的start，就有对应的启动器了，有了启动器，自动装配就会生效，然后就配置成功。
1. SpringBoot在启动的时候，从类路径下/META-INF/spring.factories 获取指定的值；
2. 将这些自动配置的类导入容器。自动配置就会生效，帮我们进行自动配置
3. 以前我们需要自动配置的东西，现在SpringBoot帮我们做了！
4. 整个JavaEE，解决方案和自动配置的东西都在spring-boot-autoconfigure这个包下
5. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到Spring容器中。
6. 容器中也会存在非常多的XXXXAutoConfituration的文件，就是这些类给容器中导入了这个场景需要的所有组件，并自动配置。
7. 有了自动配置类，免去了我们手动编写配置类的过程。

自动装配原理：
1. SpringBoot启动会加载大量的自动装配类
2. 我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；
3. 我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要使用的组件存在其中，我们就不需要再手动配置了）
4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；


xxxxAutoConfiguration：自动配置类；给容器中添加组件
xxxxProperties：封装配置文件中相关属性（可以通过properties配置文件中跳转到对应的源码）


## SpringApplication
这个类主要做了以下四件事情：
1. 推断应用的类型是普通的项目还是Web项目。
2. 查找并加载所有可用初始化器，设置到initializers属性中。
3. 找出所有的应用程序监听器，设置到listeners属性中。
4. 推断并设置main方法的定义类，找到运行的主类。


### @ConfigurationProperties 的作用：
```
将配置文件中配置的每一个属性的值，映射到这个组件中。
告诉SpringBoot将苯类中的所有属性和配置文件中的相关配置进行绑定。
参数 prefix = "person" ： 将配置文件中的person下面的所有属性一一对应。
@ConfigurationProperties使用松散绑定，比如yaml中
```

## 配置文件的位置
```
优先级1: file: ./config/       项目根目录config下（注意不是Module下）
优先级2: file: ./              项目根目录下（注意不是Module下）
优先级3: classpath: /config/   
优先级4: classpath: /          
```


pom.xml
* spring-boot-dependencies: 核心依赖在这个父工程中


启动器：
普通应用版本：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
```

Web版本：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```


## 多环境配置

### properties方式

properties文件可以通过实现多环境配置

application-dev.properties文件：
```properties
server.port=8081
```


application-test.properties文件：
```properties
server.port=8082
```

然后在application.properties文件中指定
```properties
#指定当前生效的配置文件
spring.profiles.active=test
```



### yaml方式

```yaml

name: geekhall
server:
  port: 8081
spring:
  profiles:
    active: prod

---
server:
  port: 8082
spring:
  profiles: test

---
server:
  port: 8083
spring:
  profiles: prod
```



## JSR303校验

|Constraint	| 详细信息 |
|:----	|:---- |
|@Null|	被注释的元素必须为 null|
|@NotNull|	被注释的元素必须不为 null|
|@AssertTrue|	被注释的元素必须为 true|
|@AssertFalse|	被注释的元素必须为 false|
|@Min(value)|	被注释的元素必须是一个数字，其值必须大于等于指定的最小值|
|@Max(value)|	被注释的元素必须是一个数字，其值必须小于等于指定的最大值|
|@DecimalMin(value)|	被注释的元素必须是一个数字，其值必须大于等于指定的最小值|
|@DecimalMax(value)	|被注释的元素必须是一个数字，其值必须小于等于指定的最大值|
|@Size(max, min)	|被注释的元素的大小必须在指定的范围内|
|@Digits (integer, fraction)	|被注释的元素必须是一个数字，其值必须在可接|受的范围内
|@Past	|被注释的元素必须是一个过去的日期|
|@Future|	被注释的元素必须是一个将来的日期|
|@Pattern(value)	|被注释的元素必须符合指定的正则表达式|


## 静态资源
### 1. 使用webjars
导入依赖
```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.6.0</version>
</dependency>
```
导入后就可以在：`http://localhost:8080/webjars/jquery/3.6.0/jquery.js`
这里访问到jquery。
`/webjars/`目录对应了`/META-INF/resources/webjars/`
因为`WebMvcAutoConfiguration.java`文件中：
```java
addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
```

### 2. 使用默认位置
* public
* static
* resources

上面位置对应网站根目录：  `localhost:8080/`

## 定制首页

SpringBoot默认首页为静态资源下的index.html
favicon.ico文件放在上面静态资源文件的默认根目录下就可以了


## Thymeleaf
Thymeleaf模版引擎，只需要导入对应的依赖就可以了，
将Html页面放到templates目录下。
```xml
<!-- Thymeleaf -->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>
```

### Thymeleaf语法
* Simple expressions:
  - Variable : ${...}
  - Selection Variable: *{...}
  - Message : #{...}
  - Link URL : @{...}
  - Fragment : ~{...}



## 国际化
resources目录下新建page.properties和page_zh_CN.properties，page_en_US.properties
可以在不同的properties文件中配置多语言支持

page_zh_CN.properties
```properties
login.tip=欢迎
```

page_en_US.properties
```properties
login.tip=Welcome
```

可以在IDEA的左下角的Resource Bundle中可视化编辑。

配置文件中指定目录：
```properties
spring.messages.basename=i18n.index
```

使用：
```html
Index, <span th:text="#{login.tip}"></span>
```




## SpringBoot Data
新建项目时添加jdbc和MySQL-Driver的支持。

配置数据源(application.yml)：

```yml
spring:
  datasource:
    username: mybatis
    password: yy123456
    # 假如时区报错了，就增加一个时区的配置就OK了，serverTimezone=UTC
    url: jdbc:mysql://127.0.0.1:3316/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 配置数据源类型，若不加默认为HikariDataSource
    type: com.alibaba.druid.pool.DruidDataSource
```

测试使用：
```java
@SpringBootTest
class Springboot05DataApplicationTests {

    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {

        // 查看默认数据源：class com.zaxxer.hikari.HikariDataSource
        System.out.println(dataSource.getClass());
        Connection connection = dataSource.getConnection();
        System.out.println(connection);

        connection.close();
    }
}

```

SpringBoot 中有很多XXXXTemplate类，是SpringBoot已经配置好的模板Bean，拿来即用。


注意：
SpringBoot在使用druid引入数据源之后，若使用了filters中有log4j，则需要在pom文件中加入log4j的依赖
```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

注意这里不能配置成下面的这个，否则会报ClassNotFoundException：
```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>2.14.1</version>
</dependency>
```


## SpringBoot整合MyBatis
使用MyBatis Spring Boot Stater
```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>
```

1. 导入包
2. 配置文件
3. mybatis配置
4. 编写Sql
5. service层调用dao层
6. controller调用service层
   

## SpringSecurity & Shiro
解决安全（Security）、认证（Authentication）、授权（Authorization）问题，是Spring-AOP思想的应用，
SpringSecurity是针对Spring项目的安全框架，也是SpringBoot底层安全模块默认的技术选型，可以实现强大的web安全控制，对于安全控制，我们仅仅需要引入spring-boot-starter-security模块，进行少量配置，即可实现强大的安全管理。

记住几个类：
* WebSecurityConfigurerAdapter：自定义Security策略
* AuthenticationManagerBuilder: 自定义认证策略
* @EnableWebSecurity：开启WebSecurity模式

配置：
```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    protected void configure(HttpSecurity http) throws Exception{
        // 首页所有人可以访问，功能页只有对应的人可以访问、
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");
        http.formLogin();
    }

    // SpringSecurity 5.0+增加了很多加密方法
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
//        super.configure(auth);
        auth.inMemoryAuthentication()
                .withUser("moonwhite").password("123456").roles("vip2","vip3")
                .and()
                .withUser("root").password("123456").roles("vip1","vip2","vip3")
                .and()
                .withUser("guest").password("123456").roles("vip1");

    }
} 
```


## Swagger 简介
号称世界上最流行的API框架

RestFul API文档在线自动生成工具，API与定义同步更新

### 使用Swagger
导入依赖
```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

配置类：
```java
@Configuration
@EnableOpenApi
public class SwaggerConfig {
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo()).enable(true)
                .select()
                //apis： 添加swagger接口提取范围
                .apis(RequestHandlerSelectors.basePackage("cn.geekhall"))
                //.apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("Swagger-demo项目接口文档")
                .description("Swagger-demo项目描述")
                .contact(new Contact("MoonWhite", "作者URL", "作者Email"))
                .version("1.0")
                .build();
    }
}

```


Controller代码：
```java
@Api(tags="用户管理")
@RestController
@RequestMapping("/user")
public class UserController {

    @ApiOperation("使用ID获取用户")
    @GetMapping("/{id}")
    public User getUserById(@PathVariable("id") int id) throws Exception{
        return new User();
    }
}

```

访问：http://localhost:8080/swagger-ui/index.html 即可看到生成的接口页面。


## Spring整合Redis
SpringData是和SpringBoot同级的Spring项目，在SpringBoot2.x之后，原来使用的jedis被替换成了lettuce
* jedis ： 采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全，使用jedis pool连接池！更像BIO模式
* lettuce： 采用Netty，实例可以在多个线程中进行共享，不存在线程不安全的情况。可以减少线程数据了，更像NIO模式。

源码分析：
```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class })
public class RedisAutoConfiguration {

	@Bean
	@ConditionalOnMissingBean(name = "redisTemplate") // 我们可以自己定义一个redisTemplate来替换这个默认的
	@ConditionalOnSingleCandidate(RedisConnectionFactory.class) 
	public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        // 默认的RedisTemplate没有过多的设置，redis对象都是需要序列化！
        // 两个范型都是Object，我们使用时需要强制转换为<String, Object>
		RedisTemplate<Object, Object> template = new RedisTemplate<>();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}

    // 由于String是redis中最常使用的类型，所以单独提出来一个Bean
	@Bean
	@ConditionalOnMissingBean
	@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
	public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
		StringRedisTemplate template = new StringRedisTemplate();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}
}
```

SpringBoot 所有的配置类和属性类，都有一个自动配置类 ，
自动配置类都会绑定一个properties配置文件
如：RedisAutoConfiguration 和 RedisProperties



### 配置
导入依赖：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```



```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Component
public class User implements Serializable {
    private String name;
    private int age;
}
```


如果对象没有实现Serializable接口，会抛出SerializationFailedException异常。
```java
    @Test
    public void test() throws JsonProcessingException {   
        User user = new User("极客堂", 3);
        // 真实的开发一般都使用Json来传递对象，不会抛出SerializationFailedException
        // String jsonUser = new ObjectMapper().writeValueAsString(user);
        // redisTemplate.opsForValue().set("user", jsonUser);
        redisTemplate.opsForValue().set("user", user);
        Object user1 = redisTemplate.opsForValue().get("user");
        System.out.println(user1);
    }
```


## 微服务架构的4个核心问题
1. 服务很多，客户端该怎么访问？
2. 这么多服务，服务之间如何通信？
3. 这么多服务，如何治理？
4. 服务挂了怎么办？

解决方案：
1. Spring Cloud NetFlix : 一站式解决方案（2018年12月停止维护）
   API：api网关，zuul组件
   通信：Feign --- HttpClient ： Http通信方式，同步，阻塞
   服务注册发现： Eureka
   熔断机制： Hystrix

2. Spring Cloud Alibaba : 一站式解决方案，更简单。
   

3. Dubbo + ZooKeeper : 半自动，需要整合别人的。
   API： 没有，找第三方组件，或者自己实现
   通信：Dubbo：高性能的基于Java的RPC通信框架
   服务注册发现：ZooKeeper
   熔断机制：没有，借助Hystrix


新概念：服务网格（Server Mesh）istio


## 常见面试题
1. 什么是微服务？
2. 微服务之间是如何独立通讯的？
3. SpringCloud和Dubbo有哪些区别？
4. SpringBoot和SpringCloud，请你谈谈对他们的理解
5. 什么是服务熔断？什么是服务降级？
6. 微服务的优缺点分别是什么？说下你在项目开发中遇到的坑。
7. 你所知道的微服务技术栈有哪些？请列举一二
8. Nacos、Erueka和ZooKeeper都可以提供服务注册与发现的功能，请说说两个的区别。


## 微服务技术栈
|微服务条目|落地技术|
|:---|:---|
|服务开发|SpringBoot,Spring,SpringMVC|
|服务配置与管理|Netflix公司的Archaius，阿里的Diamond等|
|服务注册与发现|Eureka、Consul、ZooKeeper、Nacos等|
|服务调用|Rest、RPC、gRPC|
|服务熔断器|Hystrix、Envoy等|
|负载均衡|Ribbon、Nginx等|
|服务接口调用（客户端调用服务的简化工具）|Feign等|
|消息队列|Kafka、RabbitMQ、ActiveMQ|
|服务配置中心管理|SpringCloudConfig、Chef等|
|服务路由（API网关）|Zuul等|
|服务监控|Zabbix、Nagios、Metrics、Specatator等|
|全链路追踪|Zipkin、Brave、Dapper等|
|服务部署|Docker、OpenStack、Kubernetes|
|数据流操作开发包|SpringCloudStream（封装与Redis、Rabbit、Kafka等发送接收消息）|
|时间消息总线|SpringCloudBus|
|||




## spring.factories

在spring-boot项目中pom文件里面添加的依赖中的bean是如何注册到spring-boot项目的spring容器中的呢？

不难得出spring.factories文件是帮助spring-boot项目包以外的bean（即在pom文件中添加依赖中的bean）注册到spring-boot项目的spring容器的。

由于@ComponentScan注解只能扫描spring-boot项目包内的bean并注册到spring容器中，因此需要@EnableAutoConfiguration注解来注册项目包外的bean。

而spring.factories文件，则是用来记录项目包外需要注册的bean类名。

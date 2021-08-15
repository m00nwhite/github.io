---
git@gitee.com:sjdt/geekhall.cn.gitlayout: post
title:  "Annotation 笔记"
date:   2021-08-05 13:44:25 +0800
categories: Annotation
---

## 原生注解
Java 定义了一套注解，共有 7 个，3 个在 java.lang 中，剩下 4 个在 java.lang.annotation 中。

|注解	| 作用 |
|:----	|:---- |
|@Override |检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。	|
|@Deprecated |标记过时方法。如果使用该方法，会报编译警告。	|
|@SuppressWarnings |指示编译器去忽略注解中声明的警告。	|


作用在其他注解的注解(或者说 元注解)是
|注解	| 作用 |
|:----	|:---- |
|@Retention	|标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。 |
|@Documented	|标记这些注解是否包含在用户文档中。 |
|@Target	|标记这个注解应该是哪种 Java 成员。 |
|@Inherited	|标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类) |


## Spring中的注解
|注解	| 作用 |
|:----	|:---- |
|@SpringBootApplication|表示这是一个SpringBoot的应用，可以自启动|
|@Configuration|表示这是一个配置类|
|@Component|注册Bean，把对象交给Spring管理|
|@Repository|表示存储层Bean，用在持久层的接口上，这个注解是将接口的一个实现类交给spring管理|
|@Service|表示业务层Bean，把对象交给Spring管理|
|@Controller|表示展示层Bean，把对象交给Spring管理|
|@Import|普通类导入到 IoC容器中|

想要让一个普通类接受 Spring 容器管理，有以下方法

* 使用 @Bean 注解
* 使用 @Controller @Service @Repository @Component 注解标注该类，然后再使用 @ComponentScan 扫描包
* @Import 方法


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
‘


## Conditional 扩展注解
|注解	| 作用 |
|:----	|:---- |
|@ConditionalOnJava|系统的java版本是否符合要求|
|@ConditionalOnBean|容器中存在指定Bean|
|@ConditionalOnMissingBean|容器中不存在指定Bean|
|@ConditionalOnExpression|满足SpEL表达式指定|
|@ConditionalOnClass|系统中有指定的类|
|@ConditionalOnMissingClass|系统中没有指定的类|
|@ConditionalOnSingleCandidate|容器中只有一个指定的Bean，或者这个Bean是首选Bean|
|@ConditionalOnProperty|系统中指定的属性是否有指定的值|
|@ConditionalOnResource|类路径下是否存在指定资源文件|
|@ConditionalOnWebApplication|当前是Web环境|
|@ConditionalOnNotWebApplication|当前不是Web环境|
|@ConditionalOnJndi|JNDI存在指定项|

## MyBatis中的注解：

|注解	| 作用 |
|:----	|:---- |
|@Mapper|表示这是一个MyBatis的Mapper类，加了@Mapper注解之后接口在编译时会生成相应的实现类|
|@MapperScan("cn.geekhall.mapper")|扫描某个包下的所有类作为Mapper|



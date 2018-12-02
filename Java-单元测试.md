---
title: 单元测试
date: 2017-05-18 11:24:53
category: Java
tags: 单元测试
---
软件测试分为单元测试、集成测试、功能测试、系统测试。
单元测试
---
测试某个功能正确性，通常情况一个功能模块会关联多个其他功能模块，这时模拟对象就有用，可以模拟其他模块特定操作行为
#### 被测系统（SUT）
#### 测试替身（Test Double）
减少对被测对象的依赖
##### Dummy Objects
	
	测试中必须传入的对象，传入这些对象并不会生产如何作用
##### Test Stub

	测试桩接受SUT内部的间接输入，可以按照我们的要求返回特定的内容给SUT
##### Test Spy
	
	将SUT内部的间接输出传到外部，不负责验证传出数据
##### Mock Object
	
	和Test Spy类似，但负责验证传出的数据

##### Fake Object
	
	和Test Stub类似，不关注SUT内部间接输入输出，仅仅是用来代替一个实际的对象
### 测试夹具（Test Fixture）
在测试之前自动初始化、回收资源。JUnit4中有@before和@After注解的方法，每个测试之前之后都执行一次。但这个缺点是效率低。

JUnit4引入类级别的夹具设置方法：
	
	使用@beforeClass和@afterClass修饰，且方法用public static void 修饰
### 测试用例
JUnit4中测试方法用@Test修饰方法，一个测试用例可以有多个测试方法
### 测试套件（Test Suite）
将多个测试用例组装成一个测试套件，批量运行。使用@RunWith和@SuiteClasses注解空类

	@RunWith(Suite.class) 表示此类为运行器
	@SuiteClasses({}) 把测试用例类作为它的参数
### 断言（Assertiosn）
判断某个语句的结果是否为真，判断是否和预期相符，例如：Assert.assertEquals()方法。可以使用assert关键字，使用它必须指定Java的-ea参数，否则断言将被忽略
集成测试
---
验证功能模块之间集成后的正确性

JUnit4
---
优秀测试框架具备几点：
* 单元测试必须独立于其他的单元测试
* 单元测试中产生的错误必须被记录下来
* 用户能够轻松指定要执行的单元测试

JUnit4采用Java 5的注解而不是子类，反射或命名机制来识别测试，它不是JUnit3.8的扩展版本，而是一个全新的测试框架

#### @Test
注解测试方法，参数：
* expected 抛出异常类型，如果测试方法没有抛出这个异常，测试失败
* timeout 设置超时时间，如果运行时间超过这个值，测试失败

#### 参数化测试
* 使用@RunWith 指定特殊运行器：Parameterized.class
* 声明一个带参数构造参数
* 创建一个@Parameterized.Parameters方法返回多组数据

``` java
@RunWith(Parameterized.class)
public class ParamTest {

    private Integer value;

    public ParamTest(Integer value) {
        this.value = value;
    }

    @Parameterized.Parameters
    public static Collection<Integer[]> data() {
        Integer[][] data = new Integer[][]{{1},{3},{5},{7}};
        return Arrays.asList(data);
    }

    @Test
    public void test() throws Exception {
        assert this.value < 5;
    }
}
```
#### 测试运行器
执行测试方法的，默认测试运行器BlockJUnit4ClassRunner.可以自定义运行器，只要继承org.junit.runner.Runner,使用@Runwith显示地声明

#### Junit4断言
assertArrayEquals方法比较数组是否相等，只有两个数组的元素都相等，则两个数组相等。

#### assertThat断言
assertThat断言是结合Hamcrest新的断言语句。结合Hamcrest提供匹配符，Hamcrest是个测试辅助工具，提供一套通用匹配符Matcher,有个静态类Matchers

模拟利器Mockito
---
是一套通过简单方法对于指定接口或类生成Mock对象的类库

#### Stub对象
提供测试时所需要的测试数据，对各种交互设置相应的回应。when().thenReturn() 设置方法调用的返回值，也可以设置方法何时调用会抛出异常
	
	when(mockUserService.getUserByName("devin")).thenReturn(new User("devin", "123"));
#### Mock对象
验证测试中所依赖对象间的交互是否能够达到预期，verify().methodXxx()语法来验证methodXxx方法是否按照预期进行了调用
``` java
		when(mockUserService.getUserByName("devin")).thenReturn(new User("devin", "123"));

        User devin = mockUserService.getUserByName("devin");
        assertNotNull(devin);

        verify(mockUserService).getUserByName("devin");
```
Mockito 使用
---
#### 创建Mock对象
对于final类，匿名类和Java的基本类型无法进行Mock,创建方式：
* mock(Class) 方法
* @mock注解，注解需要MockitoAnnotations.initMocks方法初始化

#### 设置Mock对象的期望行为及返回值
通过when(mock.someMethod).thenReturn(value),对于static和final修饰的方法无法设定
* when().thenReturn()
* doReturnn().when().methodXxx()  
* doNothing().when().methodXxx()  返回void

#### 验证交互行为
Mock对象一旦建立会自测试动记录自己的交互行为
* verify(mock, atLeastOnce()).methodXxx 方法至少调用一次
* verify(mock, atLeast(1)).methodXxx
* verify(mock, atMost(1)).methodXxx 至多调用一次
* * verify(mock, never()).methodXxx 从未

测试整合之王Unitils
---
构建DbUnit和EasyMock项目之上，与JUnit和TestNG结合，支持数据库测试、支持Mock对象进行测试，并提供与Spring和Hibernate集成

#### 配置文件
* unitils-defaults.properties 默认配置文件，开启所以功能
* unitils.properties 项目级配置文件
* unitils-local.properties 用户级配置文件

### 断言
#### assertReflectionEquals 反射断言
判断两个对象是否相等，判断对象属性值是否一样，不需要重写equals方法

	ReflectionAssert.AssertReflectionEquals()

* IGNORE_DEFAULTS 忽略Java类型默认值
* LENIENT_DATES 忽略Date的值是否相等，比较是不是被设置值或都为null
* LENIENT_ORDER 忽略集合数组中元素顺序

#### assertLenientEquals
忽略顺序忽略默认值的断言

#### assertPropertyXxxEquals 属性断言
只比较对象特定属性assertPropertyReflectionEquals和assertPropertyLenientEquals

Spring Boot
---

@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest
@ActiveProfiles("dev")

### Web Flux Test
WebTestClient 测试WebFlux服务器端点，无论是否有正在运行的服务器。
测试没有正在运行的服务器与Spring MVC中的MockMvc相当，其中使用模拟请求和响应而不是使用套接字通过网络连接

### Maven Test
	
	mvn test -Dtest=[ClassName]
	执行测试类
	mvn test -Dtest=[ClassName]#[MethodName]
	执行测试方法
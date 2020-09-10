---
layout: post
title: "Android单元测试(一)：Mockito框架的使用"
date:   2020-09-10
tags: [geek]
comments: true
author: thomaszeng
---
## Android单元测试(二):Mockito框架的使用
在实际的单元测试中，我们测试的类之间会有或多或少的耦合，导致我们无法顺利的进行测试，这是我们Juin可以使用Mockito,Mockito库能够Mock(我喜欢理解为模拟)对象，替换我们原先依赖的真实对象，这样我们就可以避免外部的影响，只测试本类，得到更准确的结果。

#### 1.四种Mock方式
+ 普通方式
```
public class MockitoTest {

    @Test
    public void testIsNotNull(){
        Person mPerson = mock(Person.class); //<--使用mock方法

        assertNotNull(mPerson);
    }
}
```
+ 注解方式
```
public class MockitoAnnotationsTest {

    @Mock //<--使用@Mock注解
    Person mPerson;

    @Before
    public void setup(){
        MockitoAnnotations.initMocks(this); //<--初始化
    }

    @Test
    public void testIsNotNull(){
        assertNotNull(mPerson);
    }
}
```
+ 运行器方法
```
@RunWith(MockitoJUnitRunner.class) //<--使用MockitoJUnitRunner
public class MockitoJUnitRunnerTest {

    @Mock //<--使用@Mock注解
    Person mPerson;

    @Test
    public void testIsNotNull(){
        assertNotNull(mPerson);
    }
}
```
+ MockitoRule方法
```
public class MockitoRuleTest {

    @Mock //<--使用@Mock注解
    Person mPerson;

    @Rule //<--使用@Rule
    public MockitoRule mockitoRule = MockitoJUnit.rule();

    @Test
    public void testIsNotNull(){
        assertNotNull(mPerson);
    }
}
```
其中后两种方法是结合JUnit框架去实现的。
#### 2.常用的打桩方法
因为Mock出的对象中非void方法都将返回默认值，比如int方法返回0，对象方法将返回null等，而void方法将什么都不做。“打桩”顾名思义就是将我们Mock出的对象进行操作，比如提供模拟的返回值等，给Mock打基础。
+ thenReturn(T value):设置要返回的值
+ thenThrow(Throwable...throwables):设置要抛出的异常
+ thenAnswer(Answer<?> answer):对结果进行拦截
+ doReturn(Object toBeReturned):提前设置要返回的值
+ doThrow(Throwable..toBeThrow):提前设置要抛出的异常
+ doAnswer(Answer answer):提前对结果进行拦截
+ doCallRealMethod():调用某一个方法的真实实现
+ doNothing():设置void方法什么都不做

#### 3.常用的验证方法
前面所说的都是状态测试，但是如果不关心返回结果，而是关心方法是否被正确的参数调用过，这时候应该使用验证方了。从概念上讲，就是和状态测试所不同的“行为测试”了。

Verify(T mock)
+ after(long millis):在给定的时间后进行验证
+ timeout(long millis):验证方法执行是否超时
+ atLeast(int minNumberOfInvocations):至少进行n次验证
+ atMost(int MaxNumberOfInvocations):最多进行n次验证
+ description(String description):验证失败时实处的内容
+ times(int wantedNumberOfInvocations):验证调用方法的次数
+ never():验证江湖没有发生，相当于times(0)
+ only():验证方法只被调用一次，相当于timer(1)

#### 4.常用参数匹配器
+ anyObject():匹配任何对象
+ any(Class<T> type):与anyObjext()一样
+ any():与anyObjext()一样
+ anyBoolean():匹配任何boolean和非空Boolean
+ anyByte():匹配任何byte和非空Byte
+ anyCollection(): 匹配任何非空Collection
+ anyDouble():匹配任何double和非空Double
+ anyFloat():匹配任何float和非空Float
+ anyInt():匹配任何int和非空Integer
+ anyList():匹配任何非空list
+ anyLong():匹配任何long和非空long
+ anyMap():匹配任何非空Map
+ anyString():匹配任何非空String 
+ contains(String substring):参数包含给定的substring字符串
+ argThat(ArgumentMatcher <T> matcher):创建自定义的参数匹配模式

#### 5.其他方法
+ reset(T...mocks):充值Mock
+ spy(Class<T> classYoSpy):实现调用真实对象的实现
+ inOrder(Object...mocks):验证执行顺序
+ @injectMocks注解：自动将模拟对象注入到被注册对象中





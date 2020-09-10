---
layout: post
title: "Android单元测试(三)：PowerMock框架的使用"
date:   2020-09-10
tags: [geek]
comments: true
author: thomaszeng
---
## Android单元测试(三)：PowerMock框架的使用
#### 1.PowerMock选择
powermock主要围绕JUnit测试框架和TestNG测试框架，在安卓中我们常用JUnit+Mockito+PowerMock组合。既然PowerMock是扩展Mockito，所以使用时，我们要找到与Mockito对应的PowerMock支持的版本去使用
这里我们使用的对应版本如下：
```
testCompile "org.mockito:mockito-core:2.8.9"

testCompile "org.powermock:powermock-module-junit4:1.7.3"
testCompile "org.powermock:powermock-module-junit4-rule:1.7.3"
testCompile "org.powermock:powermock-api-mockito2:1.7.3" //注意这里是mockito2
testCompile "org.powermock:powermock-classloading-xstream:1.7.3"
```
#### 2.PowerMock使用
首先我们定义一个Fruit类，Banana类继承他，其中有我们后面需要mock的static、private等方法
```
abstract class Fruit {

    private String fruit = "水果";

    public String getFruit() {
        return fruit;
    }
}
```
```
public class Banana extends Fruit{

    private static String COLOR = "黄色的";

    public Banana() {}

    public static String getColor() {
        return COLOR;
    }

    public String getBananaInfo() {
        return flavor() + getColor();
    }

    private String flavor() {
        return "甜甜的";
    }

    public final boolean isLike() {
        return true;
    }
}
```
##### 2.1Mock静态方法
首先使用PowerMock必须加注解@PrepareForTest和@RunWith(PowerMockRunner.class).注解@PrepareForTest里写的是静态方法所在的类
```
@RunWith(PowerMockRunner.class)
public class PowerMockitoStaticMethodTest {

    @Test
    @PrepareForTest({Banana.class})
    public void testStaticMethod() { 
        PowerMockito.mockStatic(Banana.class); //<-- mock静态类
        Mockito.when(Banana.getColor()).thenReturn("绿色");
        Assert.assertEquals("绿色", Banana.getColor());
    }
}
```
如果我们要更改类的私有static变量
```
@Test
@PrepareForTest({Banana.class})
public void testChangeColor() { 
    Whitebox.setInternalState(Banana.class, "COLOR", "红色的");
    Assert.assertEquals("红色的", Banana.getColor());
}
```
##### 2.2Mock私有方法
```
@RunWith(PowerMockRunner.class)
public class PowerMockitoPrivateMethodTest {

    @Test
    @PrepareForTest({Banana.class})
    public void testPrivateMethod() throws Exception {
        Banana mBanana = PowerMockito.mock(Banana.class);
        PowerMockito.when(mBanana.getBananaInfo()).thenCallRealMethod();
        PowerMockito.when(mBanana, "flavor").thenReturn("苦苦的");
        Assert.assertEquals("苦苦的黄色的", mBanana.getBananaInfo());
        //验证flavor是否调用了一次
        PowerMockito.verifyPrivate(mBanana).invoke("flavor"); 
    }
}
```
我们通过mock私有方法flavor，使得之前的“甜甜的”变为了“苦苦的”。
##### 2.3mock final 方法
使用方法和使用mockito一样，但是我们通过PowerMock，成功修改了isLike方法的返回值。
```
@RunWith(PowerMockRunner.class)
public class PowerMockitoFinalMethodTest {

    @Test
    @PrepareForTest({Banana.class})
    public void testFinalMethod() throws Exception {
        Banana mBanana = PowerMockito.mock(Banana.class);
        PowerMockito.when(mBanana.isLike()).thenReturn(false);
        Assert.assertFalse(mBanana.isLike());
    }
}
```
##### 2.4Mock构造方法
```
@Test
    @PrepareForTest({Banana.class})
    public void testNewClass() throws Exception {
        Banana mBanana = PowerMockito.mock(Banana.class);
        PowerMockito.when(mBanana.getBananaInfo()).thenReturn("大香蕉");
        //如果new新对象，则返回这个上面设置的这个对象
        PowerMockito.whenNew(Banana.class).withNoArguments().thenReturn(mBanana);
        //new新的对象
        Banana newBanana = new Banana();
        Assert.assertEquals("大香蕉", newBanana.getBananaInfo());
    }
```
whenNew 方法的意思是之后 new 这个对象时，返回某个被 Mock 的对象而不是让真的 new 新的对象。如果构造方法有参数，可以在withNoArguments方法中传入。
##### 2.5其他
上面我们有说到使用PowerMock就必须加@RunWith(PowerMockRunner.class)，但是我们毕竟有时会使用多个测试框架，可能@RunWith会占用。这时我们可以使用@Rule。代码如下：
```
@Rule
public PowerMockRule rule = new PowerMockRule();
```
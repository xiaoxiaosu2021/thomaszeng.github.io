---
layout: post
title: "Android单元测试(一)：JUnit框架的使用"
date:   2020-09-10
tags: [geek]
comments: true
author: thomaszeng
---

## Android单元测试(一)：JUnit框架的使用

#### 1.准备工作
我们新建一个项目，模板代码会默认在build文件中添加JUnit的依赖，而单元测试代码是放在src/test/java下面

#### 2.JUnit介绍
JUnit是java最基础的测试框架，主要作用的是断言
Assert类中主要方法如下：
+ assertEquals:断言传入的预期值与实际值是相等的
+ assertNotEquals:断言传入的预期值与实际值是不相等的
+ assertArrayEquals:断言传入的预期数组与实际数组是相等的
+ assertNull:断言传入的对象是空的
+ assertNotNull:断言传入的对象不为空
+ assertTure:断言条件为真
+ assertFalse:断言条件是假
+ assertSame:断言两个对象引用同一个对象，相当于“==”
+ assertNotSame:断言两个对象引用不同的对象，相当于“！=”
+ assertThat:断言实际值是否满足指定的条件

注意：上面的每一个方法，都有对应的重载方法，可以在前面加个String类型参数，便是如果断言失败时的提示

JUnit中常用的注解
+ @Test:表示此方法为测试方法
+ @Before:在每隔测试房钱执行，可做初始化操作
+ @After:在每隔测试方法后执行，可做释放资源操作
+ @Ignore:忽略的测试方法
+ @BeforeClass:在类中所有方法前运行。此注解修饰的方法必须是static void 
+ @AfterClass:在类中最后运行。此注解修饰的方法是static void
+ @RunWith:指定改测试类使用某个运行器
+ @Parameters:指定测试类的测试数据集合
+ @Rule:重新指定测试类方法的行为
+ @FixMethodOrder:指定测试类方法的执行顺序

执行顺序 @BeforeClass->@Before->@Test->@After->@AfterClass

#### Junit用法
我们测试简单的时间转换工具类
```
public class DateUtil {

    /**
     * 英文全称  如：2017-11-01 22:11:00
     */
    public static String FORMAT_YMDHMS = "yyyy-MM-dd HH:mm:ss";

    /**
     * 掉此方法输入所要转换的时间输入例如（"2017-11-01 22:11:00"）返回时间戳
     *
     * @param time
     * @return 时间戳
     */
    public static long dateToStamp(String time) throws ParseException{
        SimpleDateFormat sdr = new SimpleDateFormat(FORMAT_YMDHMS, Locale.CHINA);
        Date date = sdr.parse(time);
        return date.getTime();
    }

    /**
     * 将时间戳转换为时间
     */
    public static String stampToDate(long lt){
        String res;
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(FORMAT_YMDHMS, Locale.CHINA);
        Date date = new Date(lt);
        res = simpleDateFormat.format(date);
        return res;
    }
}
```
测试用例的写法
```
public class DateUtilTest {

    private String time = "2017-10-15 16:00:02";

    private long timeStamp = 1508054402000L;

    private Date mDate;

    @Before
    public void setUp() throws Exception {
        System.out.println("测试开始！");
        mDate = new Date();
        mDate.setTime(timeStamp);
    }

    @After
    public void tearDown() throws Exception {
        System.out.println("测试结束！");
    }

    @Test
    public void stampToDateTest() throws Exception {
        assertEquals(time, DateUtil.stampToDate(timeStamp));
    }

    @Test
    public void dateToStampTest() throws Exception {
        assertNotEquals(4, DateUtil.dateToStamp(time));
    }

    @Test(expected = ParseException.class)
    public void dateToStampTest1() throws Exception{
        DateUtil.dateToStamp("2017-10-15");
    }

    @Test
    @Ignore("test方法不执行\n")
    public void test() {
        System.out.println("-----");
    }
}
```






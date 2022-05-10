## Part01基础加强

### Junit

junit使用：白盒测试

步骤：

1. 定义一个测试类(测试用例)
   + 建议：
     + 测试类名：被测试的类名Test
     + 包名：xxx.xxx.xx.test
2. 定义测试方法：可以独立运行
   + 建议：
   + 方法名：test测试的方法名
   + 返回值：void
   + 参数列表：空参
3. 给方法加@Test

判定结果：

+ 红色：失败
+ 绿色：成功
+ 一般会使用断言操作来处理结果
  + Assert.assertEquals(期望的结果, 运算的结果);

```java
public class CalculatorTest {
    @Test
    public void testadd(){
        //创建计算器对象
        Calculator calculator = new Calculator();
        //调用add方法
        int result = calculator.add(10, 20);
        //断言
        Assert.assertEquals(30,result);
    }
}
```

补充：

+ @Before
  + 修饰的方法会在测试方法之前被自动执行
+ @After
  + 修饰的方法会在测试方法执行之后被自动执行

### 反射

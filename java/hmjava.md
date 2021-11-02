### Set

#### Set集合概述和特点

Set集合特点

+ 不包含重复的元素
+ 没有带索引的方法，所以不能使用普通for循环遍历



存储对象并遍历

 ```java
 public static void main(String[] args) {
         //创建集合对象
         Set<String> set = new HashSet<>();
 
         //添加元素
         set.add("hello");
         set.add("world");
         set.add("java");
         set.add("java");
 
         //遍历
         for(String s :set){
             System.out.println(s);
         }
         System.out.println(set);
     }
 ```

执行结果：

world
java
hello
[world, java, hello]

#### 哈希值

哈希值：是JDK根据对象的**地址**或者**字符串**或者**数字**算出来的int类型的**数值**

Object类中有一个方法可以获取对象的哈希值

+ public int hashCode():返回对象的哈希码值

```java
public static void main(String[] args) {
    //创建学生对象
    Student s1 = new Student("宵宫",18);

    //同一个对象多次调用hashCode()方法返回的哈希值是相同的
    System.out.println(s1.hashCode()); //2129789493
    System.out.println(s1.hashCode()); //2129789493

    Student s2 = new Student("宵宫",18);

    //默认情况下，不同对象的哈希值是不相同的
    //通过方法重写，可以实现不同对象的哈希值是相同的
    System.out.println(s2.hashCode()); //668386784

    System.out.println("hello".hashCode());//99162322
    System.out.println("world".hashCode());//113318802
    System.out.println("java".hashCode());//3254818

    System.out.println("world".hashCode());//113318802

    System.out.println("重地".hashCode());//1179395
    System.out.println("通话".hashCode());//1179395
}
```

对象的哈希值特点：

+ 同一个对象多次调用hashCode()方法返回的哈希值是相同的
+ 默认情况下，不同对象的哈希值是不同的。而重写hashCode()方法，可以实现让不同对象的哈希值相同



#### HashSet集合概述和特点

HashSet集合特点

+ 底层数据结构是哈希表
+ 对集合的迭代顺序不做任何保证，也就是说不保证存储和取出的元素顺序一致
+ 没有带索引的方法，所以不能使用普通for循环遍历
+ 由于是Set集合，所以是不包含重复元素的集合

#### 常见的数据结构之哈希表

哈希表

+ JDK8之前，底层采用**数组+链表**实现，可以说是一个元素为链表的数组
+ JDK8以后，在长度比较长的时候，底层实现了优化

![image-20211028145756685](https://gitee.com/A_Xishuai/img/raw/master/img/image-20211028145756685.png)

#### LinkedHashSet集合概述和特点

LinkedHashSet集合特点

+ 哈希表和链表实现的Set接口，具有可预测的迭代次序
+ 有链表保证元素有序，也就是说元素的存储和取出顺序是一致的
+ 有哈希表保证元素唯一，也就是说没有重复的元素

####  TreeSet集合概述和特点

TreeSet集合特点：

+ 元素有序，这里的顺序不是指存储和取出的顺序，而是按照一定的规则进行排序，具体排序方法取决于构造方法
+ TreeSet()：根据其元素的自然顺序进行排序
+ TreeSet(Comparator comparator): 根据指定的比较器进行排序
+ 没有带索引的方法，所以不能是用普通for循环遍历
+ 由于是Set集合，所以不包含重复的元素


[TOC]

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

```java
public class TreeSetDemo {
    public static void main(String[] args) {
        //创建集合对象
        TreeSet<Integer> ts= new TreeSet<Integer>();

        //添加元素
        ts.add(10);
        ts.add(30);
        ts.add(40);
        ts.add(20);
        //遍历集合
        for (Integer i :ts){
            System.out.println(i);
        }
    }
}
```

##### 自然排序Comparable的使用

+ 存储学生对象并遍历，创建TreeSet集合使用无参构造方法
+ 要求：按照年龄从小到大排序，年龄相同时，按照姓名的字母顺序排序

```java
public static void main(String[] args) {
    //创建集合对象
    TreeSet<Student> ts = new TreeSet<Student>();

    //创建学生对象
    Student s1 = new Student("xiaogong", 18);
    Student s2 = new Student("xinhai", 20);
    Student s3 = new Student("leishen", 500);
    Student s4 = new Student("ganyu", 17);
    Student s5 = new Student("shatang",18);

    //把学生添加到集合
    ts.add(s1);
    ts.add(s2);
    ts.add(s3);
    ts.add(s4);
    ts.add(s5);

    for (Student s : ts) {
        System.out.println(s.getName() + "," + s.getAge());
    }
}
```

```java
@Override
public int compareTo(Student s) {

    //按年龄从小到大排序
    int num = this.age - s.age;
    //年龄相同时，按照姓名的字母顺序排序
    int num2 = num == 0 ? this.name.compareTo(s.name) : num;
    return num2;
}
```

结论

+ 用TreeSet集合存储自定义对象，无参构造方法使用的是**自然排序**对元素进行排序的
+ 自然排序，就是让元素所属的类实现Comparable接口，重写compareTo(To)方法
+ 重写方法是，一定要注意排序规则必须按照要求的主要条件和次要条件来写

##### 比较器排序Comparator的使用

+ 存储学生对象并遍历，创建TreeSet集合使用**带参构造方法**
+ 要求：按年龄从小到大排序，年龄相同时，按照姓名的字母顺序排序

结论

+ 用TreeSet集合存储自定义对象，带参构造方法使用的是**比较器排序**对元素进行排序的
+ 比较器排序，就是**让集合构造方法接收Comparator的实现类对象**，重写Compare(T 01,T 02)方法
+ 重写放法时，一定要注意排序规则必须按照要求的主要条件和次要条件来写

### 泛型

#### 泛型概述

泛型：是JDK5中引入的特性，它提供了编译时类型安全检测机制，该机制允许在编译时检测到非法的类型他的本质是**参数化类型**，也就是说所操作的数据类型被指定为一个参数
一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？
顾名思义，就是**将类型由原来的具体的类型参数化，然后在使用/调用时传入具体的类型**
这种参数类型可以用在类、方法、接口中，分别被称为泛型类、泛型方法、泛型接口

##### 泛型定义格式：

+ <类型>:指定一种类型的格式。这里的类型可以看成是形参
+ <类型1,类型2......>:指定多种类型的格式，多种类型之间用逗号隔开。这里的类型可以看成是形参
+ 将来具体调用时候给定的类型可以看成是实参，并且实参的类型只能是引用数据类型

##### 泛型的好处：

+ 把运行时期的问题提前到了编译期间
+ 避免了强制类型转换

#### 泛型类

泛型类的定义格式：

+ 格式：修饰符class 类名<类型>{}

+ 范例：public class Generic<T>{}

  ​		  此处T可以随便蟹为任意表示，常见的如T、E、K、V等形式的参数通常用于表示泛型

```java
public class Generic<T> {
    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}
```

#### 泛型方法

泛型方法的定义格式：

+ 格式：修饰符<类型>返回值类型方法名(类型 方法名){}
+ 范例：public <T> void show(T t){}

```java
public class Generic {
    public <T> void show(T t) {
        System.out.println(t);
    }
}
```

#### 泛型接口

泛型接口定义格式：

+ 格式：修饰符 interface 接口名 <类型> {}
+ 范例：public interface Generic<T>{}

```java
public interface Generic<T> {
    void show(T t);
}

public class Genericimpl<T> implements Generic<T>{
    @Override
    public void show(T t) {
        System.out.println(t);
    }
}

public class Demo {
    public static void main(String[] args) {
        Generic<String> g1 = new Genericimpl<String>();
        g1.show("甘雨");

        Generic<Integer> g2 = new Genericimpl<Integer>();
        g2.show(30);
    }
}

```



#### 泛型通配符

为了表示各种泛型List的父类，可以使用类型通配符
+ 类型通配符：<?>
+ List<?>：表示元素类型未知List，它的元素可以匹配**任何的类型**
+ 这种带通配符的List仅表示它是各种泛型List的父类，并不能把元素添加到其中

如果说我们不希望List<?>是任何泛型List的父类，只希望它代表某一类泛型List的父类，可以使用类型通配符的上限
+ 类型通配符上线：**<? extends 类型>**
+ List<? extends Number>：它表示的类型是**Number或者其子类型**

除了可以指定类型通配符的上限，我们也可以指定类型通配符的下限
+ 类型通配符下线：**<? super 类型>**
+ List<? super Number>：它表示的类型是**Number或者其父类型**

```java
//类型通配符：<?>
List<?> list1 = new ArrayList<String>();
List<?> list2 = new ArrayList<Number>();
List<?> list3 = new ArrayList<Integer>();

System.out.println("-----------------");
//类型通配符的上限
List<? extends Number> list5 = new ArrayList<Number>();
List<? extends Number> list6 = new ArrayList<Integer>();

System.out.println("-----------------");
//类型通配符的下限
List<? super Number> list7 = new ArrayList<Object>();
List<? super Number> list8 = new ArrayList<Number>();
```

#### 可变参数

可变参数又称参数个数可变，用作方法的参数出现，那么方法参数个数就是可变的了

+ 格式：修饰符 返回值类型 方法名(数据类型...变量名){}
+ 范例：public static int sum(int...a){}

可变参数注意事项

+ 这里的变量其实是一个数组
+ 如果一个方法有多个参数，包含可变参数，可变参数要放在最后

```java
public static void main(String[] args) {
    System.out.println(sum(10,20,30));
}

public static int sum(int... a){
    int sum = 0;
    for (int i:a){
        sum +=i;
    }
    return sum;
}
```

#### 可变参数的使用

Arrays工具类中有一个静态方法：

+ public static<T> List<T> asList(T... a):返回指定数组支持的固定大小的列表
+ 返回的集合不能做增删操作，可以做修改操作

List接口中有一个静态方法：

+ public static<E> List<E> of(E... elements):返回包含任意数量的元素的不可变列表
+ 返回的集合不能做增删改操作

Set接口中有一个静态方法：

+ public static<E> Set<E> of(E... elements)：返回一个包含任意数量元素的不可变集合
+ 再给集合元素的时候，不能给重复的元素
+ 返回的集合不能做增删操作，没有修改的方法

### Map集合概述和使用

#### Map集合概述

+ Interface Map<K,V>    K：键的类型；V：值的类型
+ 将键映射到值的对象；不能包含重复的键；每个键可以映射到最多一个值
+ 举例：学生的学号和姓名

| 学号       | 姓名 |
| ---------- | ---- |
| itheima001 | 宵宫 |
| itheima002 | 新海 |
| itheima003 | 砂糖 |

创建Map集合对象

+ 多态的方式
+ 具体的实现类HashMap

#### Map集合的基本功能

| 方法名                              | 说明                                 |
| ----------------------------------- | ------------------------------------ |
| V put(K key,V value)                | 添加元素                             |
| V remove(Object key)                | 根据键删除键值对元素                 |
| void chear()                        | 移除多有的键值对元素                 |
| boolean containsKey(Object key)     | 判断集合是否包含指定的键             |
| boolean containsValue(object value) | 判断集合是否包含指定的值             |
| boolean isEmpty()                   | 判断集合是否为空                     |
| int seize()                         | 集合的长度，也就是集合中键值对的个数 |

#### Map集合的获取功能

| 方法名                         | 说明                     |
| ------------------------------ | ------------------------ |
| V get(Object key)              | 根据键获取值             |
| Set<K> keySet()                | 获取所有键的集合         |
| Collection<V> values()         | 获取所有值的集合         |
| Set<map.Entry<K,V>> entrySet() | 获取所有键值对对象的集合 |

#### Map集合的遍历

我们存储的元素都是成对出现的，所以我们把Map看成是一个夫妻对的集合

遍历思路1：

+ 把所有的丈夫集中起来
+ 遍历丈夫的集合，获取每一个丈夫
+ 根据丈夫去找对应的妻子

转换为Map集合中的操作：

+ 获取所有键的集合。用KeySet()方法实现
+ 遍历键的集合，获取到每一键。用增强for实现
+ 根据键去找值。用get(Obeject key)方法实现

遍历思路2：

+ 获取所有结婚证的集合
+ 遍历结婚证的的集合，得到每一个接换证
+ 根据结婚证获取丈夫和妻子

转换为Map集合中的操作：

+ 获取键值对对象的集合
  + Set<Map.Entry<K,V>> entrySet()：获取所有键值对对象的集合
+ 遍历键值对对象的集合，得到每一个键值对对象
  + 用增强for实现，得到每一个Map.Entry
+ 根据键值对对象获取键和值
  + 用getkey()得到键
  + 用getValue()得到值

#### 案例：HashMap集合存储学生对象并遍历（1

需求：创建一个HashMap集合，键是学号(String)，值是学生对象(Student)。存储三个键值对元素，并遍历

思路：

1. 定义学生类、

2. 创建HashMap集合对象

3. 创建学生对象

4. 把学生添加到集合

5. 遍历集合

   方法1：键找值

   方法2：键值对对象找键和值

```java
//创建HashMap集合对象
HashMap<String, Student> hm = new HashMap<String, Student>();

//创建学生对象
Student s1 = new Student("宵宫", 18);
Student s2 = new Student("新海", 20);
Student s3 = new Student("砂糖", 16);

//把学生添加到集合
hm.put("tiwate001", s1);
hm.put("tiwate002", s2);
hm.put("tiwate003", s3);

//遍历集合
Set<String> keySet = hm.keySet();
for (String key : keySet) {
    Student value = hm.get(key);
    System.out.println(key + "," + value.getName() + "," + value.getAge());

}
```

#### 案例：HashMap集合存储学生对象并遍历（2

需求：创建一个HashMap集合，键是学生对象(Student)，值是居住地(String)存储多个键值对元素，并遍历

要求保证键的唯一性：如果学生对象的成员变量值相同，我们就认为是同一个对象

思路：

1. 定义学生类

2. 创建HashMap集合对象

3. 创建学生对象

4. 把学生添加到集合

5. 遍历集合

6. 在学生类中创协两个方法

   hashCode()

   equals()

   ```java
   HashMap<Student, String> hm = new HashMap<Student, String>();
   
   Student s1 = new Student("宵宫", 18);
   Student s2 = new Student("砂糖", 19);
   Student s3 = new Student("钟离", 20);
   Student s4 = new Student("钟离", 20);
   
   hm.put(s1, "稻妻");
   hm.put(s2, "蒙德");
   hm.put(s3, "璃月");
   hm.put(s4, "提瓦特");
   
   Set<Student> keySet = hm.keySet();
   
   for (Student key : keySet) {
       String value = hm.get(key);
       System.out.println(key.getName() + "," + key.getAge() + "," + value);
   }
   ```

#### 案例：ArrayList集合存储HashMap元素并遍历

需求：创建一个ArrayList集合，存储三个元素，每一个HashMap的键和值都是String，并遍历

思路：

1. 创建ArrayList集合
2. 创建HashMap集合，并添加键值对元素
3. 把HashMap作为元素添加到ArrayList集合
4. 遍历ArrayList集合

```java
//创建ArrayList集合
ArrayList<HashMap<String, String>> array = new ArrayList<HashMap<String, String>>();

//创建HashMap集合并添加键值对元素
HashMap<String, String> hm1 = new HashMap<String, String>();
hm1.put("孙策", "大乔");
hm1.put("周瑜", "小乔");
//把HashMap作为元素添加到ArrayList集合
array.add(hm1);

HashMap<String, String> hm2 = new HashMap<String, String>();
hm2.put("郭靖", "黄蓉");
hm2.put("杨过", "小龙女");
//把HashMap作为元素添加到ArrayList集合
array.add(hm2);

HashMap<String, String> hm3 = new HashMap<String, String>();
hm3.put("令狐冲", "任盈盈");
hm3.put("林平之", "岳灵珊");
//把HashMap作为元素添加到ArrayList集合
array.add(hm3);

//遍历ArrayList集合
for (HashMap<String, String> hm : array) {
    Set<String> keySet = hm.keySet();
    for (String key : keySet) {
        String value = hm.get(key);
        System.out.println(key + "," + value);
    }
}
```

#### 案例：HashMap集合存储ArrayList元素并遍历

需求：创建一个HashMap集合，存储三个键值对对象，每一个键值对元素的键是String，值是ArrayList

每一个ArrayList的元素是String，并遍历


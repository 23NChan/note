### 动态规划题目特点

1. 计数
   + 有多少种方式走到右下角
   + 有多少种方式选出K个数使得和是Sum
2. 求最大最小值
   + 从左上角走到右下角路径的最大数字和
   + 最长上升子序列长度
3. 求存在性
   + 取石子游戏，先手是否必胜
   + 能不能选出K个数使得和是Sum

### 动态规划组成部分一：确定状态

+ 状态在动态规划中的作用属于定海神针
+ 解动态规划的时候需要开一个数组，数组的每个元素 f[i] 或者f[i] [j]代表什么
  + 类似于解数学题中，x, y, z 代表什么
+ 确定状态需要两个意识
  + **最后一步**
  + **子问题**

### 动态规划组成部分二：转移方程

### 动态规划组成部分三：初始条件和边界情况

### 动态规划组成部分四：计算顺序



#### 练习1

+ 你有三种硬币，分别是面值2元，5元和7元，每种硬币有足够多

+ 买一本书需要27元

+ 如何用最少的硬币组合正好付清

  ​		**求最大最小值动态规划**

```java
public static void main(String[] args) {
   int res= f();
    System.out.println(res);
}

private static int f() {
    //定义硬币种类
    int[] money = {2, 5, 7};
    //定义所需金额
    int Price = 27;
    //创建数组确定状态
    int[] res = new int[Price + 1];
    res[0] = 0;
    for (int i = 1; i < res.length; i++) {
        //定义初始和边界条件
        res[i]=Integer.MAX_VALUE;
        for (int b = 0; b < money.length; b++) {
            //计算顺序
            if (i >= money[b] && res[i - money[b]] != Integer.MAX_VALUE && res[i - money[b]] + 1 < res[i]) {
                res[i] = res[i - money[b]] + 1;
            }
        }
    }
    if (res[27] == Integer.MAX_VALUE) {
        return -1;
    } else {
        return res[Price];
    }

}
```

### 练习2

给定m行n列的网络，有一个机器人从左上角(0,0)出发，每一步可以向下或者向右走一步

问有多少种不同的方式走到右下角

![img](https://gitee.com/A_Xishuai/img/raw/master/img/DPcase2.png)

**计数型动态规划**

走后一步：无论机器人用何种方式到达右下角，总最后挪动的一步**向右**或者**向下**

右下角坐标设为(m-1,n-1)

那么前一步机器人一定在(m-2,n-1)或者(m-1,n-2)

那么，如果机器人有X中方法是从左上角走到(m-2,n-1)，有Y种方式从左上角走到(m-1,n-2),则机器人有X+Y种方式走到(m-1,n-1)

问题转化为，机器人有多少种方式从左上角走到(m-2,n-1)和(m-1,n-2)

```java
public static void main(String[] args) {
    System.out.println(uniquePaths(4,5));
}

private static int uniquePaths(int m,int n){
    int[][] arr=new int[m][n];
    arr[0][0]=1;
    //row
    for (int i=0;i<m;i++){
        //column
        for (int j = 0;j<n;j++){
            if (i==0||j==0){
                arr[i][j]=1; //corner case
            }else {
                //          up          left
                arr[i][j]=arr[i-1][j]+arr[i][j-1];
            }
        }
    }
    return arr[m-1][n-1];
}
```

### 练习3

**存在性动态规划**

有n块石头分别在X轴的0,1,........,n-1位置

有一只青蛙在石头0，想跳到石头n-1

如果青蛙在第i块石头上，他最多可以向右跳距离a[i]

问青蛙能否跳到石头n-1

问青蛙能否跳到石头n-1

+ 例子：
+ 输入：a=[2,3,1,1,4]
+ 输出：True
+ 输入：a=[3,2,1,0,4]
+ 输出：False

+ 最后一步：如果青蛙能跳到最后一块石头n-1，那我们考虑他跳的最后一步
+ 这一步是从石头i跳过来的，i<n-1
+ 这需要两个条件同时满足：
  + 青蛙可以跳到石头i
  + 最后一步不能超过跳跃的最大距离：n-1-i <= a[i]
+ 那么，我们需要知到青蛙能不能跳到石头**i (i<n-1)**
+ 子问题
+ 状态：设f[j]表示青蛙能不能跳到石头j

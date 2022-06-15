[toc]

## 认识时间复杂度

常熟时间的操作：一个操作如果和数据量没有关系，每次都是固定时间完成的操作，叫做常数操作。

时间复杂度为一个算法流程中，常数操作数量的指标。常用O（读作big O）来表示。具体来说，在常熟操作数量的表达式中，**只要高阶项，不要低阶项，也不要高阶项系数**，剩下的部分记为f(N)，那么时间复杂度为O(f(N)).

评价一个算法流程的好坏，先看时间复杂度的指标，然后再分析不同数据样本下的实际运行时间，也就是常数项时间。

### 二分查找

1. O(M*logM)
2. O(N+M)

O(M*logM)+O(N+M)

样本量不确定不能消除N

如果N很小O(N+M)

如果N很大M很小O(M*logM)

### 冒泡排序（淘汰）

时间复杂度O(N^2)

额为空间复杂度O(1)

``` java
    public static void bubbleSort(int[] arr) {
        if (arr == null || arr.length < 2) {
            return;
        }
        for (int end = arr.length - 1; end > 0; end--) {
            for (int i = 0; i < end; i++) {
                if (arr[i] > arr[i + 1]) {
                    swap(arr, i, i + 1);
                }
            }
        }
    }

    public static void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = tmp;
    }
```



### 选择排序（淘汰）

时间复杂度O(N^2)

额为空间复杂度O(1)

``` java
public static void selectionSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    for (int i = 0; i < arr.length - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < arr.length; j++) {
            minIndex = arr[j] < arr[minIndex] ? j : minIndex;
        }
        swap(arr, i, minIndex);
    }
}


//交换
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```

### 插入排序

时间复杂度

+ 最好情况O(N)

+ 平均情况

+ 最坏情况O(N^2)

一律按最差情况估计

空间复杂度O(1)

``` java
public static void insertionSort(int[] arr){
    if(arr ==null || arr.length < 2){
            return;
        }
    for (int i = 1 ; a < arr.length; i++){
        for(int j = i - 1 ; j > = 0 && arr[j] > arr[j+1] ; j--){
            swap(arr,j,j+1);
        }
    }
}

public static void swap(int[] arr, int i,int j){
    arr[i] = arr[i] ^ arr[j];
    arr[j] = arr[i] ^ arr[j];
    arr[i] = arr[i] ^ arr[j];
}
```



### 对数器

对数期的概念和使用

1. 有一个你想要测的方法a
2. 实现一个绝对正确但复杂度不好的方法b
3. 实现一个随机样本生成器
4. 实现比对的方法
5. 把方法a和方法b比对很多次来验证方法a是否正确
6. 如果有一个样本是的比对出错，打印样本分析是哪个方法出错
7. 当样本数量很多时比对测试依然正确，可以确定方法a已经正确 

## 排序

### 归并排序

#### 递归归并

时间复杂度：O(N*logN)

空间复杂度：O(N)	

```java
public static void mergesort(int[] arr) {
    if (arr == null && arr.length < 2) {
        return;
    }
    sortProcess(arr, 0, arr.length - 1);
}

public static void sortProcess(int[] arr, int L, int R) {
    ;
    if (L == R) {
        return;
    }
    int mid = (L + R) / 2;
    sortProcess(arr, L, mid);
    sortProcess(arr, mid + 1, R);
    merge(arr, L, mid, R);
}

public static void merge(int[] arr, int l, int mid, int r) {
    int[] temp = new int[r - l + 1];
    int p1 = l;
    int p2 = mid + 1;
    int i = 0;
    while (p1 <= mid && p2 <= r) {
        temp[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
    }
    while (p1 <= mid) {
        temp[i++] = arr[p1++];
    }
    while (p2 <= r) {
        temp[i++] = arr[p2++];
    }
    for (i = 0;  i< temp.length; i++) {
        arr[l + i] = temp[i];
    }
}
```

#### 小和问题

在一个数组中，每一个数左边比当前数小的数累加起来，叫做这个数组的小和

```java
public static int smallsum(int[] arr) {
    if (arr == null && arr.length < 2) {
        return 0;
    }
    //复制数组保证原数组不会受到影响
    int[] a=new int[arr.length];
    for (int i=0;i<arr.length;i++){
        a[i]=arr[i];
    }
    return mergesort(a, 0, a.length - 1);
}

public static int mergesort(int[] arr, int L, int R) {
    if (R == L || L > R) {
        return 0;
    }
    int mid = L + ((R - L) >> 1);
    return mergesort(arr, L, mid)
            + mergesort(arr, mid + 1, R)
            + merge(arr, L, mid, R);
}

public static int merge(int[] arr, int L, int mid, int R) {
    int p1 = L;
    int p2 = mid + 1;
    int i = 0;
    int[] temp = new int[R - L + 1];
    int res = 0;
    while (p1 <= mid && p2 <= R) {
        res += arr[p1] < arr[p2] ? arr[p1] * (R - p2 + 1) : 0;
        temp[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
    }
    while (p1 <= mid) {
        temp[i++] = arr[p1++];
    }
    while (p2 <= R) {
        temp[i++] = arr[p2++];
    }
    for (i = 0; i < temp.length; i++) {
        arr[L + i] = temp[i];
    }
    return res;
}

public static int nomal(int[] arr) {
    int res = 0;
    for (int b = arr.length - 1; b > 0; b--) {
        for (int j = 0; j < b; j++) {
            res += arr[j] < arr[b] ? arr[j] : 0;
        }
    }
    return res;
}
```

### 快速排序

#### 荷兰国旗问题

给定一个数组arr，和一个数num，请把小于num的数放在数组的左边，等于num的数放在数组的中间，大于num的数放在数组的右边

要求额外空间复杂度O(1)，时间复杂度O(N)

```java
public static int[] partition(int[] arr, int L, int R, int num) {
    int less = L - 1;
    int more = R + 1;
    int cur = L;
    while (cur < more) {
        if (arr[cur] < num) {
            swap(arr, ++less, cur++);
        } else if (arr[cur] > num) {
            swap(arr, --more, cur);
        } else {
            cur++;
        }
    }
    return new int[]{less + 1, more - 1};
}

public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```

#### 随机递归快排

时间复杂度 长期期望O(N*logN)

额外空间复杂度O(logN)



```java
public static void quicksort(int[] arr) {
    if (arr == null || arr.length < 2) {
    return;
    }
        quicksort(arr, 0, arr.length - 1);
    }

    public static void quicksort(int[] arr, int L, int R) {
        if (L < R) {
            //随机取对比数，打乱初始数据结构，减少极限性g
            swap(arr, L + (int) ((R - L ) * Math.random()), R);
            int[] p = partition(arr, L, R);
            quicksort(arr, L, p[0] - 1);
            quicksort(arr, p[1] + 1, R);
        }

    }

    public static int[] partition(int[] arr, int L, int R) {
        int less = L - 1;
        int more = R;
        while (L < more) {
            if (arr[L] < arr[R]) {
                swap(arr, ++less, L++);
            } else if (arr[L] > arr[R]) {
                swap(arr, --more, L);
            } else {
                L++;
            }
        }
        swap(arr, more, R);
        return new int[]{less + 1, more};
    }

    public static void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
```

### 堆排序

时间复杂度O(N*logN)

额外空间复杂度O(1)

堆结构非常重要

1. 堆结构的heap Insert与heap i fy
2. 堆结构的增大和减少
3. 如果知识建立堆的过程，时间复杂度为O(N)
4. 优先级队列结构，就是堆结构

#### 大根堆->完全二叉树

时间复杂度O(N)

额外空间复杂度O(1)

在完全二叉树中，任何一个子树的最大值都是这颗子树的头部

```java
public static void heapSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    for (int i = 0; i < arr.length; i++) {
        heapInsert(arr, i);
    }
}

public static void heapInsert(int[] arr, int index) {
    while (arr[index] > arr[(index - 1) / 2]) {
        swap(arr, index, (index - 1) / 2);
        index =(index-1)/2;
    }
}
```

#### 小根堆->完全二叉树

任何一颗子树的最小值都是这颗子树的头部

```java
public static void heapSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    for (int i = 0; i < arr.length; i++) {
        heapInsert(arr, i);
    }
}

public static void heapInsert(int[] arr, int index) {
    while (arr[index] < arr[(index - 1) / 2]) {
        swap(arr, index, (index - 1) / 2);
        index =(index-1)/2;
    }
}
```

#### 堆排序

```java
public static void heapSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    /**  效率慢舍弃   用作演技heapInsert
    for (int i = 0; i < arr.length; i++) {//O(N)
        heapInsert(arr, i);//O(logN)
    }
     */

    /**
     * 从后往前使每个数下沉效率更快
     */
    for (int i=arr.length-1;i>=0;i--){
        heapify(arr,i,arr.length);
    }


    int heapSize = arr.length;
    swap(arr, 0, --heapSize);
    while (heapSize > 0) {//O(N)
        heapify(arr, 0, heapSize);//O(logN)
        swap(arr, 0, --heapSize);//O(1)
    }
}

public static void heapInsert(int[] arr, int index) {
    while (arr[index] > arr[(index - 1) / 2]) {  //不可用位运算  当index等于0时 错误
        swap(arr, index, (index - 1) >> 1);
        index = (index - 1) >> 1;
    }
}

public static void heapify(int[] arr, int index, int heapSize) {
    int left = (index << 1) + 1;  //左子节点下标
    while (left < heapSize) {//下方还有子节点的时候
        //如果有两个子节点返回大子节点下标
        int largest = left + 1 < heapSize && arr[left + 1] > arr[left] ? left + 1 : left;
        largest = arr[largest] > arr[index] ? largest : index;
        if (largest == index) {
            break;
        }
        swap(arr, largest, index);
        index = largest;
        left = (index << 1) + 1;
    }
}
```

### 桶排序

```java
public static void radixSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    radixSort(arr, 0, arr.length - 1, maxbits(arr));
}

public static int maxbits(int[] arr) {
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < arr.length; i++) {
        max = Math.max(max, arr[i]);
    }
    int res = 0;
    while (max != 0) {
        res++;
        max /= 10;
    }
    return res;
}

public static void radixSort(int[] arr, int l, int r, int digit) {
    final int radix = 10;
    int i = 0, j = 0;
    //有多少个数准备多少个辅助空间
    int[] bucket = new int[r - l + 1];
    for (int d = 1; d <= digit; d++) {
        int[] count = new int[radix];

        for (i = l; i <= r; i++) {
            j = getDigit(arr[i], d);
            count[j]++;
        }
        for (i = 1; i < radix; i++) {
            count[i] = count[i] + count[i - 1];
        }
        for (i = r; i >= l; i--) {
            j = getDigit(arr[i], d);
            bucket[count[j] - 1] = arr[i];
            count[j]--;
        }
        for (i = l, j = 0; i <= r; i++, j++) {
            arr[i] = bucket[j];
        }
    }
}

public static int getDigit(int x, int d) {
    return ((x / ((int) Math.pow(10, d - 1))) % 10);
}
```

## 哈希表

1. 有无伴随数据，是HashMap和HashSet唯一的区别，底层的实现结构是一回事
2. 放入哈希表的东西，如果是基础类型，内部按值传递，内存占用就是这个东西地大小
3. 放入哈希表的东西，如果不是基础类型，内部按引用传递，内存占用是这个东西内存地址的大小

## 无序表

1. 有无伴随数据，是TreeSet和TreeMap唯一的却别，底层的实际结构是一回事
2. 放入有序表的东西，如果不是基础类型，必须提供比较器
3. 红黑树、AVL数、size-balance-tree和跳表等都属于有序表结构，知识底层具体实现不同
4. 不管是什么底层具体实现，只要是有序表，，都有固定的基本功能和固定的时间复杂度

## 链表

**单链表的节点结构**

``` java
Class Node<V>{
    V value;
    Node next;
}
```

以上结构的节点依次连接起来所形成的链叫单链表结构

**双链表的节点结构**

``` java
Class Node<V>{
    V value;
    Node next;
    Node last;
}
```

以上结构的节点依次连接起来所形成的链叫双链表结构

## Case

### Max左-Max右最大绝对值问题

时间复杂度O(N)

额外空间复杂度O(1)

1. 找到全局最大Max
2. 如果Max在左部分，尽量让右部分Max最小，只让右部分包含N-1位置数
3. 如果Max在右部分，尽量让做部分Max最小，只让左部分包含0位置数


```java
public static void main(String[] args) {
    int[] arr = {10, 20, 50, 20, 30, 30, 60, 10,5,70};
    int num = AbsMax(arr);
    System.out.println(num);
}

public static int AbsMax(int[] arr) {
    int max = 0;
    for (int i : arr) {
        max = i > max ? i : max;
    }
    if (max == arr[0]) {
        return max - arr[arr.length - 1];
    } else if (max == arr[arr.length - 1]) {
        return max - arr[0];
    } else {
        return Math.max(max-arr[0],max-arr[arr.length-1]);
    }
}
```

### 子矩阵的最大累加和问题

时间复杂度(N^2*M)

额外空间复杂度(M)

组数组最大累加和：

1. 从左往右依次遍历每个数
2. 把这个数累加到cur 
3. 用max来捕获cur累加后的值，只要最大值
4. if cur<0   --> cur=0,否则不变

假设：

1. 有一个子数组是累加和最大的子数组
2. 在1.的条件下，长度最长
3. 如果子数组开头是i，结尾时j
   + 以i开头的到j结尾的前缀组数组累加和一定不小于零
   + 以i-1为结尾的后缀累加和一定小于零

把矩阵压缩成数组解决问题

```java
public class A02_SubMatrixMaxSum {
    public static void main(String[] args) {
        int[][] matrix={{-90,48,78},{64,-40,64},{-81,-7,66}};
        System.out.println(maxSum(matrix));
    }

    public static int maxSum(int[][] m) {
        if (m == null || m.length == 0 || m[0].length == 0) {
            return 0;
        }
        int max = Integer.MIN_VALUE;
        int cur = 0;
        int[] s = null;
        for (int i = 0; i != m.length; i++) {
            s = new int[m[0].length];
            for (int j = i; j != m.length; j++) {
                cur = 0;
                for (int k = 0; k != s.length; k++) {
                    s[k] += m[j][k];
                    cur += s[k];
                    max = Math.max(cur, max);
                    cur = cur < 0 ? 0 : cur;
                }
            }
        }
        return max;
    }
}
```









## end

[toc]

### 认识时间复杂度

常熟时间的操作：一个操作如果和数据量没有关系，每次都是固定时间完成的操作，叫做常数操作。

时间复杂度为一个算法流程中，常数操作数量的指标。常用O（读作big O）来表示。具体来说，在常熟操作数量的表达式中，**只要高阶项，不要低阶项，也不要高阶项系数**，剩下的部分记为f(N)，那么时间复杂度为O(f(N)).

评价一个算法流程的好坏，先看时间复杂度的指标，然后再分析不同数据样本下的实际运行时间，也就是常数项时间。

#### 二分查找

1. O(M*logM)
2. O(N+M)

O(M*logM)+O(N+M)

样本量不确定不能消除N

如果N很小O(N+M)

如果N很大M很小O(M*logM)

#### 冒泡排序（淘汰）

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



#### 选择排序（淘汰）

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

#### 插入排序

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



#### 对数器

对数期的概念和使用

1. 有一个你想要测的方法a
2. 实现一个绝对正确但复杂度不好的方法b
3. 实现一个随机样本生成器
4. 实现比对的方法
5. 把方法a和方法b比对很多次来验证方法a是否正确
6. 如果有一个样本是的比对出错，打印样本分析是哪个方法出错
7. 当样本数量很多时比对测试依然正确，可以确定方法a已经正确 

### 排序

#### 归并排序

##### 递归归并

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

##### 小和问题

在一个数组中，没一个数左边必当前数小的数累加起来，叫做这个数组的小和

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

#### 快速排序

##### 荷兰国旗问题

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

##### 随机递归快排

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

#### 堆排序

时间复杂度O(N*logN)

额外空间复杂度O(1)

堆结构非常重要

1. 堆结构的heap Insert与heap i fy
2. 堆结构的增大和减少
3. 如果知识建立堆的过程，时间复杂度为O(N)
4. 优先级队列结构，就是堆结构

##### 大根堆->完全二叉树

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

小根堆->完全二叉树

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


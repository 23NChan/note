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
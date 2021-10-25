# 知识点1类型

数据类型的本质作用：合理的利用空间

```c
void text01()
{
	//基本类型 char shourt in long float double
    //char 占1字节空间
    
   	 char ch;
   	 printf("sizeof(ch)=%d\n", sizeof(ch));//1字节
    
     int num;
    printf("sizeof(int)=%d\n",sizeof(num));//4字节
}
```

1 B==8位 b(二进制位)

1 b只能存放0或1

# 知识点2指针变量

编号(地址)：内存中每一个字节分配一个号码

定义一个变量 存放上面的号码 这样的变量叫做指针变量

默认取地址，取空间的起始地址

```c
void test02
{
    int num = 100;
    //取变量的地址用 &
    //&num 代表变量num的起始地址
    printf("%p\n", &num);
    
    //需求：定义一个指针变量 保存num的地址
    //在定义的时候： *说明p是指针变量 而不是普通变量
    int *p;
    printf("sizeof(p)=%d\n",sizeof(p));
    
    //num的地址 与p建立关系
    p = &num;
    
    printf("num = %d\n",num);
    //使用者：*p表示取p保存的地址编号 对应空间的内容
    printf("*p = %d\n", *p);  //100

}
```

```c
void test03{
    int num = 10;
    
    //指针变量两种类型：自身的类型  	 指向的类似
    //自身类型：在指针变量定义的时候 将变量名拖黑 	剩下啥类型	指针变量就是啥类型
    		//p 自身的类型就是int *
    //指向的类型：在指针变量定义的时候 将变量名和离他最近的一个*一起拖黑 	剩下啥类型	指针变量指向的类型就是啥类型
    		//p 指向的类型是int
    
    //指针变量指向类型的作用：决定了指针变量	所取空间内容的宽度 决定了指针变量+1跳过的单位跨度
    int *p = NULL;
    p = &num;
    
    //指针变量的跨度
    printf("&num=%u\n", &num); //7338536
    printf("p=%u\n", p);//7338536
    printf("p+1=%u\n",p+1);//7338540
    
    chasr *p1 = &num;
    printf("p1=%u\n",p1);//7338536
    printf("p1+1=%u\n",p1+1);//7338537
    
    print("*p = %d\n", *p); //num的值
}
```

在linux/windows系统倒着存(小端)



```c
void test04()
{
    int num =0x01020304;
    
    int *p1 = &num;
    
    printf("*p1 = %#x\n", *p1);  //0x1020304
    
    short *p2 = &num;
    
    printf("*p2 = %#x\n", *p3);  //304
    
    char *p3 = &num;
    printf("*p3 = %#x\n", *p3);  //4 
    
    short *p4 = &num;
    p4 = p4 +1; 	//p4 += 1
    printf("*p4 = %#x\n", *p4);  //102
}
```

<img src="https://gitee.com/A_Xishuai/img/raw/master/img/image-20211026000753654.png" alt="image-20211026000753654"  />


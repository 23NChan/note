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

```c
void test02
{
    int num = 100;
    //去变量的地址用 &
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
    int num * 10;
    
}
```


### c数据类型：

​	//占用一个字节，存储单个字节   -128~127   ascii码  存储字符
​	char c = 'a';
​	//两个字节   -32768~32767   存储短整形
​	short s = 10;
​	//四个字节  -2147483648~2147483647   存储整形
​	int i = 100;
​	//四个字节    存储长整形
​	long l = 1000;
​	//四个字节   浮点数  存储浮点数
​	float f = 10.43F;
​	//八个字节   存储浮点数   精度高
​	double d  = 10.5;

#### 运算符：

##### 算术运算符

​	int num1 = 10;
​	int num2 = 20;
​	int result = 0;

```c
result = num1 + num2;  // 30
result = num1 - num2;  // -10
result = num1 * num2;  //  200
result = num1 / num2;  // 0
result = num1 % num2;  // 取余   取模
```

//  int类型只能存储整形  小数点后面的数字直接舍去
	printf("%d\n",result);

##### 	关系运算符

​	//  >    >=   <   <=   !=    ==  
```c 
int num1 = 20;
int	num2 = 30; 
```

在c语言中  真假用 0  和  1表示  
0：表示假   1：表示真
	`printf("%d\n",num1 == num2);
`	

##### 单目运算符

自加和自减   ++   --
变量名   只能有字母，数字，下划线组成   标识符的命名规范   不能用数字开头
	

```c
int i = 20;
int num1 = 10;
int num2 = 20;
int result = num1++;  //  num1 =  11

printf("%d\n",result);  // 10

result = ++num1;   // num1 = 12

result = num1++   +   ++num2;

printf("%d\n",result);
```



### 循环控制语句

#### if

``` c
	int a = 10;
	int b = 20;
	if(a > b)
	{
		printf("a比较大");
	}
	else 
	{
		printf("b比较大");
	}
```



#### for

``` c
//死循环
for(;;)
	{
		printf("1111");
	}
```

#### while

``` c
char s = 127;  // 一个字节  -128~127
	while(s)
	{
		printf("%d\n",s);
		s++;
	}

do 
	{
		printf("AAAAA");
	}while(0);
    
    
```

#### switch

``` c
	int week = 2;
	switch(week)
	{
	case 1:
		printf("星期一");
		break;
	case 2:
		printf("星期二");
		break;
	case 3:
		printf("星期三");
		break;
	case 4:
		printf("星期四");
		break;
	case 5:
		printf("星期五");
		break;
	}
```

#### sizeof()表达式   

括号里面填 数据类型或者变量  返回对应的字节

printf("%d\n",sizeof(int));
printf("%d\n",sizeof(double));

char ch = 'a';
printf("%d\n",sizeof(ch));

int array[] = {2,20,300,400,50020,2002,40,50};
printf("%d\n",sizeof(array));


取地址运算符 ： &
逻辑与：&&
按位与：&
 %p表示输出十六进制的地址

sizeof的作用：判断数据类型或者变量占的内存


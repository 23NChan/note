#### 条件概率

定义：设A,B为两个事件，若P(B)>0，
$$
p(A|B)=\frac{P(AB)}{P(B)}
$$
为事件B发生条件下事件A发生的**条件概率**

“缩减样本空间”

#### 条件的独立性

定义：设A与B是两个事件，如果满足等式
$$
P(AB)=P(a)P(B)
$$
称事件A与B**相互独立**，简称A，B独立

定理：设P(B)>0，则事件A，B相互独立的充要条件是
$$
P(A|B)=P(A)
$$
**独立没有图**

**三个事件的独立性**：称事件A,B,C**两两独立**
$$
P(AB)=P(A)P(B)\\
P(BC)=P(B)P(C)\\
P(AC)=P(A)P(C)
$$
进一步如果满足等式
$$
P(ABC)=P(A)P(B)P(C)
$$
则称事件A,B,C相互独立

#### 全概率公式与贝叶斯公式

（全概率公式自己推，贝叶斯公式不用背）

#### 随机变量

定义：设随机试验的样本空间，X=X(w)是定义在样本空间上的实值单值函数

称X=X(w)为**随机变量**，一般用大写字母X,Y,Z····等表示随机变量，用小写字母x,y,z····表示随机变量的取值（实数）

分类：离散型随机变量、连续型随机变量、非离散非连续随机变量

##### 随机变量的分布函数

定义：设一个X是一个随机变量，x是任意实数，函数
$$
F(x)=P\begin{Bmatrix}
X\leqslant x
\end{Bmatrix}
,-\infty<x<\infty
$$
称为X的**分布函数**，它表示X的取值落在实数x左侧的概率

##### 利用分布函数计算条件概率

已知X的分布函数F(X)，有
$$
P\begin{Bmatrix}
X<=a
\end{Bmatrix}=F(a)\\
P\begin{Bmatrix}
X<a
\end{Bmatrix}=F(a-0)
$$

##### 离散型随机变量及其概率分布

1.**离散型随机变量**：若随机变量全部可能取到的值有限个或可列无限多个，这种随机变量成为**离散型随机变量**

2.**分布率**：设离散型随机变量X所有可能取的值为x1,x2,...,xk,....且X取各个可能值得概率，即事件{X=xk}得概率为
$$
P\begin{Bmatrix}
X=x_k
\end{Bmatrix}=p_k,\space\space\space\space\space\space(k=1,2,....)
$$
称上式为离散型随机变量X得**概率分布**或**分布率**


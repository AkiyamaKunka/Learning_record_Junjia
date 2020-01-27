# C++ knowledge

## 1. Abstruct
Some of the notes is in Chinese with poor language organization, however, I firmly suppose those accumulation has tremendous value which cannot be disgarded.



### C语言笔记

### 1.Static 
仅仅在函数第一次被调用的时候初始化。之后就不会再初始化。

### 2.指针
是地址，让形式指针和实际指针指向同一个数字，然后改变这个指针的指向，就是改变这个绝对物理地址指向的数字，所以即便形式参数消失，地址指向的数字改变，最后输出实际参数的地址不变，但是实际参数的值却改变了。（我真tm聪明）

### 3.二维数组的概念
首先定义一个二维数组
a[I][I];
各个概念
a+i  &*(a+i) 指向第i行的指针
a[i]   *(a+i)   &*(*(a+i)+0)指向第i行第零列的指针
a[I][k] *(*(a+i)+k)第i行第k列的元素

计算a[I][j]相对于首元素偏移的地址数目 = i*列数➕j
Int (*p)[4]是定义一个指针，指向一个含有4个元素的整形数组，把数组当成一个指向的整体来看，如果指针➕1，那么指向下四个元素组成的数组。
Whereas int *p [4] is not , it is a array which has 4 elements , 每个元素都是一个指针类型

[] * 这两个符号使得指针的指向向下一个维度， &使指针的指向向上一个维度
当维度降低了n次，（n是数组的维度）那么指针的指向就是具体元素的值了

### 4.对于外部函数传递数组指针首地址的语法

void  function(int * x)
void  function(int x[] )

### 5.c++中cout输出小数点精度 
#include <iomanip> cout<<precision(小数点后精确的位数）<<num;


### 6.maclloc函数 calloc函数 free函数 realloc函数

### 7.对于指向函数的指针
初始化为
Int *(p) (int ,int )
p=function（给指针赋予函数的开头地址）
调用的时候使用*p(容易忘记）

### 8.赋值
如果拥有一个void *int p指针
想给一个int a赋值
a= *(int *) p

### 9.位运算

左移动
15<<1即把15换成二进制，所有的数字向左移动一个单位

右移动
15>>1
分成
逻辑右移 （最高位默认补0）
- [ ] 算数右移 等价于/2 （无论正负）
定义在不同的编译器中 也不同，故程序鲁棒性会降低


位域是用来减少空间占用的

### 10.二进制整数
10进制先化16进，然后两两位数从小到大以二进制存放


### 11。freopen函数和fopen函数

 
r 只读，不可以写入，不存在时候指针返回NULL

r+ 又读又写，其余同上

w只写 不读， 不存在时候创造文件，（文件原有存在文本被覆盖）

w+又读又写 其余同上
 a（appendix）只w不r 不存在返回null，（写入内容存在文件末尾，不覆盖）

a+ 可r可w，不存在创造文件，可在任意位置写入内容，初指针在文件末尾

ab（在末尾加一个b就是binary 二进制方式）
注意调用函数之后指针是否为空，如果为空记得return -1报错

fclose和 fopen搭配，关闭文件
freopen则不需要


读取文件EOF（end of file）就相当于读取文件时候的最后末尾的文件，如果读到这个关键字就结束读取

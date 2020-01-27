# Algorithm & Data Structures

## 1. Binary Search 

 O(logN+2)

The array has been presorted , so won’t take that much time to do the searching .

```cpp
int binary_search(int flag)
{
    int right=sizeof(arr)/ sizeof(int)-1;
    //cause C++ array star at 0, so we subtract 1;
    int left=0;
    while(left<right){
        int mid=(left+right)/2;
        if(flag==arr[mid])return mid;
        else if (flag>arr[mid]){
            left=mid;
        }
        else right=mid;
    }
    return -1;//not found;
}
```

## 2. Eulclid's Greatest commen divisor
```cpp
long long gcd( long long m, long long n ) {
    while( n != 0 ){ 
    long long rem = m % n; 
		  m = n; 
		  n = rem; 
} 
 return m;  } 
```

## 3. Maximum Sum of a sequence

Maximum	 Sum of a sequence	.
Here are some ways to solve it.

This is the first one, **divide and conquer**.


```cpp

int maxf(int *arr,int left,int right)
{
    if(left==right){
        if( arr[left]>0)
            return arr[left];
        else
            return 0;
    }
    else {
        int boder = (right +left) / 2;
        int max_left = maxf(arr, left, boder  );
        int max_right = maxf(arr, boder+1, right);

        int sum1 = 0;
        int max1 = 0;
        for (int i = boder ; i >=left; --i) {
            sum1 += arr[i];
            if (sum1 > max1);
            max1 = sum1;
        }
        int sum2 = 0;
        int max2 = 0;
        for (int i = boder+1; i <= right; ++i) {
            sum2 += arr[i];
            if (sum2 > max2);
            max2 = sum2;
        }

        int sum_cross = sum1 + sum2;
        int maxtot = sum_cross;
        if (max_left > maxtot)
            maxtot = max_left;
        if (max_right > maxtot)
            maxtot = max_right;
        return maxtot;
    }
}
 ```
 ### A much better second way
 This is a really smart algorithm	which discovered it does not influence the outcome when you find that the subsequence sum from arr[I] to arr[j] is negative, and jump it.

 I don’t know where is the mistake……XD….
Id better not waste time on it.



 ```cpp
int maxf(int *arr)
{
    int maxsum=0;
    int thissum=0;
    for (int i = 0; i <num ; i++){
        thissum += arr[i];
        if(thissum>maxsum)
            maxsum=thissum;
        else if(thissum<0)
            thissum=0;
    }
    return maxsum;
}
```
## 4. Optimal Power

The running time is O(logN)

the key thinking is power(x,n)=power(x^2,n/2), so the computing time is drastically decreased.
```cpp
nt best_power(int x,int n)
{
    if(n==0)return 1;
    if(n==1)return x;
    if(n%2==0) return best_power(x*x,n/2);
    else return best_power(x,n-1)*x;
}
```

## 5.Dynamic Lists (a Data structure)

This is actully a data structure.
Still remember when I was learning this, sitting next to the girl who I have a secret crush on. In a condition where it is esay for me to focus while hard to comprehend.
XD So this includes the input and output of the data structure, via only dual pointer for all of these amazing implementations.

```cpp
#include <cstring>
#include <iostream>
#include <cmath>
#include <algorithm>
#include <string>
#include <vector>
#include <set>
#include <cstdio>
#include <sstream>
#include <map>
#include <stack>
#include <queue>
#include <cstring>
#include <iomanip>
using namespace std;
const int maxn=1000001;
#define LOCAL
#define SIZE sizeof(struct ST)

struct ST
{
    int num;
    float  score;
    struct ST *next ;
};

struct ST* creat_graphic(int n)
{
   struct ST *p1,*p2,*head;
   p1=p2=(struct ST*)malloc(SIZE);
   int o_n=n;
   cin>>p1->num>>p1->score;

   while(n>0)
   {
       if(n==o_n)head=p1;
       else p2->next=p1;
       p2=p1;
       p1=(struct ST*)malloc(SIZE);
       cin>>p1->num>>p1->score;
       n--;
   }
   p2->next=NULL;
   return head;
}

void print_f(struct ST* head)
{
    struct ST *p;
    p=head;
    do{
        cout << p->num <<" "<< p->score<<endl;
        p = p->next;
    }while(p!=NULL);
}

int main() {
#ifdef LOCAL
    freopen("data.in.txt", "r", stdin);
    freopen("data.out.txt", "w", stdout);
#endif
    int n;
    cin>>n;
    print_f(creat_graphic(n));
    return 0;
}
```

## 6. Depth First Search

Anyway, a very easy question which requests to show all of the arranges of an array.

```cpp
//输入一个n位字符串，输出其所有的排列方式。
#include <iostream>
#include <string>

using namespace std;

char a[15];       //储存字符串的n位
int box[15]={0};  //表示第i位上的字符
int used[15]={0}; //判断第i个字符是否已在序列中
int n;

void dfs(int step)  //考虑第step位元素的放置
{
    if(step==n+1)   //越界以后停止搜索，输出每一个位置的值
    {
        for(int i=0;i<n;i++)
            printf("%c",box[i]);
        cout<<endl;
    }
    else
    {
        for(int i=0;i<n;i++)
        {
            if(used[i]==0)  //检测每一个元素是否能够放在第step个位置，若能，从它开始搜索
            {
                box[step-1]=a[i];
                used[i]=1;
                dfs(step+1);
                used[i]=0;  //上面已经对于a[i]放在step位置第情况搜索完了，现在让a[i]不在这个位置
            }
        }
    }
}

int main()
{
    char ch;
    int i=0;
    while(cin>>ch)
    {
        a[i]=ch;
        i++;
    }
    n=i;
    dfs(1);
    return 0;
}
```

## 7. Tree Buiding by Pointers
This is from Ziyi Huang, a fun guy who shared the block of code with me.
Just stored here when needed.
```cpp
#include <iostream>
#include <string>
using namespace std;

//定义节点
struct Node
{
    bool haveValue;
    int value;
    Node *left,*right;
    Node():haveValue(false),left(NULL),right(NULL){};
};

//申请新的节点
Node* newNode()
{
    return new Node();
}

Node* root=newNode();

//对一个指定节点赋值
void addNode(int v,string str)
{
    Node *u=root;
    for(auto iter=str.begin();iter!=str.end();iter++)
    {
        if(*iter=='L')
        {
            if(u->left==NULL) u->left=newNode();
            u=u->left;
        }
        else if(*iter=='R')
        {
            if(u->right==NULL) u->right=newNode();
            u=u->right;
        }
    }
    u->haveValue=true;
    u->value=v;
}

//清除所有节点
void remove(Node *u)
{
    if(!u->haveValue) return;
    if(u->left!=NULL) remove(u->left);
    if(u->right!=NULL) remove(u->right);
    delete u;
}
```

## 8. Quick Sort

A critical algorithm that can sort the array, by using the thingking of Recuision.

```cpp
int part_sort(int *arr,int low,int high)
{
    int& key=arr[high];
    while(low<high)
    {
        while(low<high && arr[low]<=key)
            low++;
        while(low<high && arr[high]>=key)
            high--;
        swap(arr[low],arr[high]);
    }
    swap(arr[low],key);
    return low;
}
//这个是一次排序，将一次的从low到high范围内的数组，把大于original high的都放在右边，小于oh的都放在左边，然后把oh放在中间的axis上，返回axis的位置。


void quick_sort(int * arr,int low,int high)
{
    if(low>=high) return;
    int axis=part_sort(arr,low,high);
    quick_sort(arr,axis+1,high);
    quick_sort(arr,low,axis-1);
}

//这个是排序函数主体，在拥有了一次axis的基础上，再一分为二，排列左右两端的数组，使他们都带有同part sort函数已经排序过数列相同的性质。
```

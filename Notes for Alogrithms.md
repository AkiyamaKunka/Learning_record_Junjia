# Algorithm

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

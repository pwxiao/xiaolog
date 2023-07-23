
学习到卷积，于是用c++实现了一下
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <vector>
#include <queue>
#include <map>
#include <algorithm>
using namespace std; 
void convolve(int a[],int b[],int m,int n,int*p){
    int i,j;
    int k=m+n-1;//卷积后数组长度
    /**卷积计算**/
    for(i=0; i<k; i++,p++)
    {
        for(j=max(0,i+1-n); j<=min(i,m-1); j++)
            *p+=a[j]*b[i-j];
    }
}

int main()
{
    int m=3,n=3;
    int a[3]= {1,2,3},b[3]= {4,5,6};
    int c[9] = {0};
    int *p = c;
    convolve(a,b,3,3,p);
    for(int i = 0;i<5;i++) cout<<c[i]<<" ";
    cout<<endl;
}
```

# c++结构体
## 用vector对结构体排序
学习过程中实现了两种不同方式，有微妙区别，综合来说，第二种更好。

第一种：
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
const int N = 3;
using namespace std;
typedef struct Box{
    int len;
    string sign;
}b;

bool cmp(b a,b c){
    return a.len<c.len;
}

int main(){
    vector<b> test;
    b b;
    for(int i = 0;i<N;i++){
        cin>>b.sign>>b.len;
        test.push_back(b);
    }
    sort(test.begin(),test.end(),cmp);
    for(int i= 0;i<N;i++) cout<<test.at(i).len<<endl;
    return 0;
}
```
第二种：

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;
const int N  = 3;
typedef struct Students{
    int score;
    string name;
}stu;

bool cmp(stu a,stu b){
    return a.score>b.score;
}

int main(){
    vector<stu> a(N);
    for(int i = 0;i<N;i++) cin>>a[i].name>>a[i].score;
    sort(a.begin(),a.end(),cmp);
    for(int i = 0;i<N;i++) cout<<i<<" "<<a[i].name<<" "<<a[i].score<<endl;
    return 0;
}
```


## 利用c++重载实现复数运算

```cpp
#include <iostream>
using namespace std;

typedef struct Complex{
    int r;
    int i;
    Complex(){r = 0;i = 0;}
    //这是构造函数的写法
    Complex(int real,int imag):r(real),i(imag){}
    void show(){
        cout<<r<<"+"<<i<<"i"<<endl;
    }
}Complex;

void printComplex(Complex a){
    cout<<a.r<<"+"<<a.i<<"i"<<endl;
}

 Complex operator + (Complex a,Complex b){
    return Complex(a.r+b.r,a.i+b.i);
}
 Complex operator-(Complex a,Complex b){
    return Complex(a.r-b.r,a.i+b.i);
}

int main(){
    //利用self.show()来输出
    Complex S = Complex(1,2)+Complex(2,3);
    S.show();
    //利用printComplex()输出
    printComplex(Complex(1,2)+Complex(2,3));
    return 0;
}
```

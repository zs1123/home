# 0x00
## 位运算

### $a^b$ [acwing](https://www.acwing.com/problem/content/91/)
### 题目描述

求 a 的 b 次方对 p取模的值。
输入格式

三个整数 a,b,p

,在同一行用空格隔开。
输出格式

输出一个整数，表示a^b mod p的值。
数据范围

1≤a,b,p≤109


#### 样例

```
3 2 7
2
```


----------

### 算法
#### 快速幂  复杂度 $O(logn)$

在二进制表示中，例如($a^{1101}$ 代表 $a^{2^{3}}*a^{2^{2}}*a^{2^{0}}$ )可以预处理出$a^1,a^2,a^4 ...$

循环共进行 $logb$ 次
#### C++ 代码
```c++
#include<iostream>
using namespace std;
int main(){
    
    int a,b,p;
    cin >> a>>b>>p;
    int res = 1%p; 
    
    while(b){
        if(b&1) res = res*1ll*a %p; // 转化long long  防止溢出
        a = a* 1ll*a%p;
        b>>= 1;
    }
    cout << res;
    return 0;
}
```
---
### $a^b$ [acwing](https://www.acwing.com/problem/content/90/)
### 题目描述
求 a 乘 b 对 p

取模的值。
输入格式

第一行输入整数a
，第二行输入整数b，第三行输入整数p

。
输出格式

输出一个整数，表示a*b mod p的值。
数据范围

$1≤a,b,p≤10^{18}$

#### 样例

```
输入
3
4
5
输出
2
```


----------

### 算法
#### 分析
若直接a*b 则会导致溢出，但是a+a 不会溢出，考虑将a连加b次  
计算出  
a  
a+a  
a+a+a+a ...

#### C++ 代码
```c++
#include<iostream>
using namespace std;
using LL = unsigned long long;
int main(){
    
    LL a,b,p;
    cin >> a>>b>>p;
    LL res = 0;
    
    while(b){
        if(b&1) res = (res + a) %p;
        a = (a+a)%p;
        b>>=1;
    }
    cout << res;
    return 0;
}
```
---
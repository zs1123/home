# 模板题

## 0x00排序

### 快速排序
给定你一个长度为n的整数数列。

请你使用快速排序对这个数列按照从小到大进行排序。

并将排好序的数列按顺序输出。

#### 输入格式
输入共两行，第一行包含整数 n。

第二行包含 n 个整数（所有整数均在$1-10^{9}$范围内），表示整个数列。

#### 输出格式
输出共一行，包含 n 个整数，表示排好序的数列。

#### 数据范围
1≤n≤100000m
#### 输入样例：
5
3 1 2 4 5
#### 输出样例：
1 2 3 4 5

```c++
#include<iostream>
using namespace std;
const int M = 1e5 + 10;

int d[M];
void qs(int l,int r){
    if(l >=r )
        return;
    
    int i = l -1,j = r + 1,pivo = d[l + r >> 1];
    
    while(i < j){
        
        while(d[++i] < pivo);
        while(d[--j] > pivo);
        if(i < j) swap(d[i],d[j]);
    }
    qs(l,j);
    qs(j + 1,r);
    
}
int main(){
    int n;
    scanf("%d",&n);
    for(int i = 0;i < n;i ++ ){
        scanf("%d",&d[i]);
    }
    
    qs(0,n-1);
    for(int i = 0;i < n;i++)
        printf("%d ",d[i]);
    
    return 0;
}
```
### 归并排序
```c++
#include<iostream>

using namespace std;
const int M = 1e5 + 10;

int d[M],tmp[M];

void merge_sort(int l,int r){
    if(l >= r) return;
    
    int mid = l + r >> 1;
    
    merge_sort(l,mid), merge_sort(mid + 1,r);
    int k = 0,i = l,j = mid + 1;
    
    while(i <= mid && j <= r){
        if(d[i] <= d[j]) tmp[k++] = d[i++];
        else tmp[k++] = d[j++];
    }
    while(i <= mid) tmp[k++] = d[i++];
    while(j <= r) tmp[k++] = d[j++];
    
    for(i = l,j = 0;j < k;j++,i++) d[i] = tmp[j];

}


int main(){
    int n;
    scanf("%d",&n);
    for(int i = 0;i < n;i ++ ){
        scanf("%d",&d[i]);
    }
    
    merge_sort(0,n-1);    
    for(int i = 0;i < n;i++)
        printf("%d ",d[i]);
    
    return 0;
}
```

## 0x01高精度

### 加法
```c++
vector<int> add(){
    
    vector<int> res;
    int c= 0;
    for(int i = 0;i < A.size() || i <B.size();i++){
        if(i < A.size()) c += A[i];
        if(i < B.size()) c += B[i];
        res.push_back(c % 10);
        c/= 10;
    }
    if(c) res.push_back(c);
    return res;
}


```
### 减法
```c++
vector<int> sub(vector<int>& A,vector<int>& B){
    int c = 0;
    vector<int> res;
    for(int i = 0;i < A.size();i++){
        c = A[i] - c;
        if(i < B.size()) c = c - B[i];
        res.push_back((c + 10) % 10);
        if(c < 0) c = 1; // 有借位
        else c = 0;
    }
    while(res.size() > 1 && res.back()==0) res.pop_back();// 去掉前导 0
    return res;
}
```
### 乘法
```c++
vector<int> mul(vector<int> A,int b){
    int c = 0;
    vector<int> res;
    for(int i = 0;i < A.size();i++){
        c += A[i]*b;
        res.push_back(c % 10);
        c /= 10;
    }  
    if(c) res.push_back(c);
    return res;
}
```
### 除法
```c++
vector<int> div(vector<int> A,int b,int& r){
    
    r = 0;
    vector<int> res;
    for(int i = A.size() -1;i>=0;i--){
        
        r = r * 10 + A[i];
        res.push_back(r / b);
        r = r % b;
    }
    
    reverse(res.begin(),res.end());
    while(res.size() > 1 && res.back() == 0) res.pop_back();
    return res;
}

```
## 0x02 前缀和差分
将$[l,r] + c$ 操作变为$O(1)$
### 差分
输入一个长度为n的整数序列。

接下来输入m个操作，每个操作包含三个整数l, r, c，表示将序列中[l, r]之间的每个数加上c。

请你输出进行完所有操作后的序列。

#### 输入格式
第一行包含两个整数n和m。

第二行包含n个整数，表示整数序列。

接下来m行，每行包含三个整数l，r，c，表示一个操作。

#### 输出格式
共一行，包含n个整数，表示最终序列。

#### 数据范围
$1≤n,m≤100000,$  
$1≤l≤r≤n,$  
$−1000≤c≤1000,$  
$−1000≤整数序列中元素的值≤1000$ 
#### 输入样例：
6 3
1 2 2 1 2 1
1 3 1
3 5 1
1 6 1
#### 输出样例：
3 4 5 3 4 2
```c++
#include<iostream>
using namespace std;

const int M = 1e5 + 10;
int a[M],b[M];
int main(){
    int n,m;
    scanf("%d%d",&n,&m);
    for(int i  =1;i <=n;i++) {
        scanf("%d",&a[i]);
        b[i] = a[i] - a[i-1]; // 计算差分
    }
    while(m--){
        int l,r,c;
        scanf("%d%d%d",&l,&r,&c);
        b[l] += c;
        b[r+1] -= c;
        
    }
    for(int i = 1;i <=n;i++) b[i] +=b[i-1]; // 求前缀和
    for(int i = 1;i <=n;i++) printf("%d ",b[i]);
    return 0;
}
```
### 矩阵差分
输入一个n行m列的整数矩阵，再输入q个操作，每个操作包含五个整数x1, y1, x2, y2, c，其中(x1, y1)和(x2, y2)表示一个子矩阵的左上角坐标和右下角坐标。

每个操作都要将选中的子矩阵中的每个元素的值加上c。

请你将进行完所有操作后的矩阵输出。

#### 输入格式
第一行包含整数n,m,q。

接下来n行，每行包含m个整数，表示整数矩阵。

接下来q行，每行包含5个整数x1, y1, x2, y2, c，表示一个操作。

#### 输出格式
共 n 行，每行 m 个整数，表示所有操作进行完毕后的最终矩阵。

#### 数据范围
$1≤n,m≤1000,$  
$1≤q≤100000,$  
$1≤x1≤x2≤n,$  
$1≤y1≤y2≤m,$  
$−1000≤c≤1000,$  
$−1000≤矩阵内元素的值≤1000$  
#### 输入样例：
3 4 3  
1 2 2 1  
3 2 2 1  
1 1 1 1  
1 1 2 2 1  
1 3 2 3 2  
3 1 3 4 1  
#### 输出样例：
2 3 4 1  
4 3 4 1  
2 2 2 2  
```c++
#include<iostream>
using namespace std;

const int M = 1010;
int a[M][M],b[M][M];
// 插入
void insert(int x1,int y1,int x2,int y2,int c){
    b[x1][y1] += c;
    b[x2 + 1][y1] -= c;
    b[x1][y2+1] -= c;
    b[x2+1][y2+1] += c;
    
}
int main(){
    int n,m,q;
    cin >> n>>m>>q;
    for(int i =1;i <=n;i++)
        for(int j = 1;j <=m;j++)
            scanf("%d",&a[i][j]), insert(i,j,i,j,a[i][j]);
    
    
    while(q--){
        int x1,x2,y1,y2,c;
        cin >> x1>>y1>>x2>>y2>>c;
        insert(x1,y1,x2,y2,c);
    }
    for(int i = 1;i <=n;i++)
        for(int j = 1;j <=m;j++)
            b[i][j] += b[i-1][j] + b[i][j-1] - b[i-1][j-1];// 求前缀和
    for(int i = 1;i <=n;i++){
        
        for(int j = 1;j <=m;j++)
            printf("%d ",b[i][j]);
        printf("\n");
        
    }
    
    return 0;
}

```
---
title: "算法模板"
subtitle: "c++代码模板"
date: '2024-01-28T13:44:26+08:00'
lastmod: '2026-01-27T10:58:26+08:00'
draft: false
categories: ['模板']
tags: ['算法']
---



## 高精度

### 加法

```cpp
//初始条件：a:存储第一个大数(正序),b:存储第二个大数(正序),c:存储结果(逆序)
int a[N],b[N],c[N];//1~N,0号位存储数组长度
void g_add(int* A,int* B,int* C){
	C[0] = 1;//数组长度,从第一位开始 
	int t = 0;//保存进位和结果信息 
	for(int i = A[0],j = B[0];i > 0 || j > 0;--i,--j,++C[0]){
		t += i > 0 ? A[i] : 0;
		t += j > 0 ? B[j] : 0;
		C[C[0]] = t % 10;
		t /= 10;
	}
	C[C[0]] = t ? t : C[--C[0]];//如果最高位进位,则将进位信息保存,否则长度-1(循环导致长度+1)
}
```

### 减法

```cpp
//初始条件：a:存储第一个大数(正序),b:存储第二个大数(正序),c:存储结果(逆序),f:确定正负
int a[N],b[N],c[N],f;//1~N,0号位存储数组长度
int g_cmp(int* A,int* B){//A>=B返回1,否则返回0
	if(A[0] != B[0]) return A[0] > B[0];
	for(int i = 1;i <= A[0];++i) if(A[i] != B[i]) return A[i] > B[i];
	return 1;
}
void g_sub(int* A,int* B,int* C){
	C[0] = 1;
	int t = 0;//存储借位和结果信息 
	for(int i = A[0],j = B[0];i > 0 || j > 0;--i,--j,++C[0]){
		t = i > 0 ? A[i] - t : t;
		t -= j > 0 ? B[j] : 0;
		C[C[0]] = (t + 10) % 10;
		t = t < 0 ? 1 : 0;
	}
	while(C[0] > 1 && !C[--C[0]]);//删除前导0
}
int main(){
    ~~~
    if((f = g_cmp(a,b))) g_sub(a,b,c);
    else g_sub(b,a,c);
    cout << (f ? "" : "-");
    ~~~
    return 0;
}
```

### 乘法

```cpp
//初始条件：a:存储第一个大数(正序),b:存储第二个大数(正序),c:存储结果(逆序)
int a[N],b[N],c[2*N];//1~N,0号位存储数组长度
void g_mul(int* A,int* B,int* C){
	C[0] = A[0] + B[0];//长度多一位,方便判断结果为0的情况
	for(int i = A[0];i > 0;--i) for(int j = B[0];j > 0;--j) C[C[0]-i-j+1] += A[i] * B[j];//逆序放置结果
	for(int i = 1;i < C[0]-1;++i){//处理每一位进位
        C[i+1] += C[i] / 10;
        C[i] %= 10;
	}
	while(C[0] > 1 && !C[--C[0]]);//结果为0时删除多余0
}
```

### 除法

```cpp
//初始条件：a:存储第一个大数(正序),b:存储第二个大数(正序),c:存储结果(逆序),r:存储余数(正序)
int a[N],b[N],c[N],r[N];//1~N,0号位存储数组长度
void g_div(int* A,int* B,int* C,int* R){
	if(!g_cmp(A,B)) {//如果B比A大，则直接商0,余数就是A
		C[0] = 1;
		for(int i = 0;i <= A[i];++i) R[i] = A[i];
		return;
	}
	int T[A[0]+1] = {0};//商的结果
	C[0] = A[0] + 1;//商的位数预处理和被除数相同
	for(int i = A[0];i > 0;--i){
		T[++T[0]] = A[A[0]-i+1];
		while(g_cmp(T,B)){
			++C[i];
			g_sub(T,B,R);
			if(!R[R[0]==1]) T[0] = 0;
			else{
				for(int j = R[0];j > 0;--j) T[R[0]-j+1] = R[j];
				T[0] = R[0];
			}
		}
		while(T[1] == 0 && T[0] > 0) --T[0]; 
	}
	while(C[0] > 1 && !C[--C[0]]);
	for(int i = 0;i <= T[0];++i) R[i] = T[i];
	if(T[0] == 0) R[0]= 1;
}
```

## 数学

### 质数筛法

```cpp
//p:存储质数,idx:p的指针,vis:存储当前元素是否被访问
int p[N],idx,vis[N];
void get_primes(int x){
    for(int i = 2;i <= x;++i){
        if(!vis[i]) p[idx++] = i;
        for(int j = 0;p[j] <= x/i;++j){
            vis[p[j]*i] = 1;
            if(i % p[j] == 0) break;
        }
    }
}
```

### 高斯消元

```cpp
const double eps = 1e-6;//精度判断(是否为0)
double a[N][N];//存储方程组矩阵
int gauss(){
	int r,c;//行,列
	for(c = 0,r = 0;c < n;++c){//系数矩阵第一列开始，遍历到最后一列
		int t = r;//记录当前列绝对值最大值所在行
		for(int i = r;i < n;++i) if(fabs(a[i][c]) > fabs(a[t][c])) t = i;
		if(fabs(a[t][c]) < eps) continue;//如果当前为0,则直接进入下一列,利用eps防止c++小数精度问题
		for(int i = c;i <= n;++i) swap(a[t][i],a[r][i]);//交换当前行与当前列最大值所在的行
		for(int i = n;i >= c;--i) a[r][i] /= a[r][c];//当前行当前列元素置为1,当前行其余元素也要更新,防止影响当前元素,逆序遍历
		for(int i = r+1;i < n;++i)//从下一行开始,更新当前列的下面所有行的值为0,更新当前行下面的所有行
			if(fabs(a[i][c]) > eps)//当前元素不是0
				for(int j = n;j >= c;--j)//逆序计算
					a[i][j] -= a[r][j] * a[i][c];
		++r;//如果当前行列不为0,则计算到这里,行数加一,否则就是当前行的下一列开始计算
	}
	if(r < n){//矩阵不满秩
		for(int i = r;i < n;++i)//判断是否存在系数为0,常s
			if(fabs(a[i][n]) > eps) return 2;//无解
		return 1;//无穷多解
	}
	for(int i = n-1;i >= 0;--i)//最后一行开始
		for(int j = i+1;j < n;++j)//当前行所在对角线的后面所有列
			a[i][n] -= a[i][j] * a[j][n];//存储最后结果(当前行其他列全部消为0)
	return 0;//唯一解
}
```

```cpp
//使用方式
int t = gauss();
		if(t == 0) for(int i = 0;i < n;++i) cout << fixed << setprecision(2) << (fabs(a[i][n]) < eps ? 0 : a[i][n]) << endl;//防止输出-0.00
		else if(t == 1) cout << "无穷多解" << endl;
		else cout << "无解" << endl;
```

### 快速幂

```cpp
int qmi(int a,int k,int p){
    int ret = 1;
    while(k){
        if(k & 1) ret = (LL) ret * a % p;
        a = (LL) a * a % p;
        k >>= 1;
    }
    return ret;
}
```

### 最大公约数

```cpp
int gcd(int x,int y){
    return y ? gcd(y,x%y) : x;
}
```

## 二分查找

```cpp
// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;//最左边的结果
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;//最右边的结果
        else r = mid - 1;
    }
    return l;
}
```

## 字符串

### 读取整行字符

```cpp
//如果读取该行前有读取其他字符或者换行，则需要多加一个getline(cin,s);读取上一行遗留的换行符
getline(cin,s);//会读取一整行包括空格和换行符，但是会自动删去换行符
```

### 分割字符串

```cpp
//构造原始字符串s的字符串流
stringstream ssin(s);
//以delim作为分割符,分割后的字符串存储到t中
string t;
while(getline(ssin,t,'delim')){...}
```

## 图

### 基本操作

```cpp
int h[N],e[N],ne[N],idx;
void add(int a,int b){//a->b
    e[idx] = b;ne[idx] = h[a];h[a] = idx++;
}
memset(h,-1,sizeof h);//初始化头节点为-1
for(int i = h[a];~i;i = ne[i]){...}//遍历邻接表
```

> ***边多就用for循环，邻接矩阵存储。边少就用堆优化，连接表存储。***

### dijkstra

```cpp
int dis[N],vis[N],h[N],ne[2*N],e[2*N],w[2*N],idx;
void add(int a,int b,int c){//a->b,权重为c
    e[idx] = b;ne[idx] = h[a];w[idx] = c;h[a] = idx++;
}
void dijkstra(int u){//计算从u出发，到其他顶点的最短距离
    memset(dis,0x3f,sizeof dis);//dis[i]表示u到i的最短距离
    dis[u] = 0;
    priority_queue<PII,vector<PII>,greater<PII>> heap;
    heap.push({0,u});
    vis[u] = 1;
    while(!heap.empty()){
        auto cur = heap.top().second,len = heap.top().first;
        heap.pop();
        if(vis[cur]) continue;
        vis[cur] = 1;
        for(int i = h[cur];~i;i = ne[i]){
            int j = e[i];
            if(dis[j] > len + w[i]){
                dis[j] = len + w[i];
                heap.push({dis[j],j});
            }
        }
    }
}
```

### floyd

```cpp
int g[N][N];//g[i][j]存储的是原始数组,同时也是最终结果数组,i->j的最短距离
void floyd(int g[][N]){
    for(int k = 1;k <= n;++k){
        for(int i = 1;i <= n;++i){
            for(int j = 1;j <= n;++j){
				g[i][j] = min(g[i][j],g[i][k] + g[k][j]);
            }
        }
    }
}
```

### kruskal

```cpp
int n,m,p[N];//p[]并查集数组
struct Edge{
    int a,b,w;//a和b之间的无向边,权重为w
    bool operator< (const Edge &e) const{
        return w < e.w;
    }
}edges[M];
int find(int x){
    return x != p[x] ? p[x] = find(p[x]) : p[x];
}
int kruskal(){//返回最小生成树的权重之和
    sort(edges,edges + m);//从权重最小的边开始
    for(int i = 1;i <= n;++i) p[i] = i;
    int ret = 0,cnt = 0;//ret存储结果,cnt存储构成树的边的数量
    for(int i = 0;i < m;++i){
        int a = edges[i].a,b = edges[i].b,w = edges[i].w;
        a = find(a),b = find(b);
        if(a != b){//如果a,b所在连通块不连通
            p[a] = b;ret += w;++cnt;//将a,b连通,权重累加,树边+1
        }
    }//最小生成树为顶点数-1,判断是否构成最小生成树
    if(cnt < n - 1) return INF;
    return ret;
}
```

### prim

```cpp
int prim(){
    memset(dis,0x3f,sizeof dis);
    int ret = 0;
    for(int i = 0;i < n;++i){
        int t = -1;
        for(int j = 1;j <= n;++j) if(!vis[j] && (t == -1 || dis[t] > dis[j])) t = j;
        if(i && dis[t] == INF) return INF;
        if(i) ret += dis[t];
        vis[t] = 1;
        for(int j = 1;j <= n;++j) dis[j] = min(dis[j],g[t][j]);
    }
    return ret;
}
```

## 树状数组

```cpp
int tr[N];//从1开始输入
memset(tr,0,sizeof tr);//开始全部置为0
int lowbit(int x){//求x二进制末尾有几个0
    return x & -x;
}
void add(int j,int x){//第j个位置加上x
    for(int i = j;i <= n;i += lowbit(i)) tr[i] += x;
}
int query(int j){//查询1~j区间的和
    int ret = 0;
    for(int i = j;i;i -= lowbit(i)) ret += tr[i];
    return ret;
}
for(int i = 1;i <= n;++i) add(i,a[i]);//初始化树状数组,a[i]为原数组
query(l)-query(r-1)//查询l~r区间的和
```

## 线段树

```cpp
struct node{
    int l,r;
    int sum;//可以替换为其他需要求得条件
}tr[4*N];
void pushup(int u){//回溯时进行的操作
    tr[u].sum = tr[u << 1].sum + tr[u << 1 | 1].sum;
}
void build(int u,int l,int r){//建树
    if(l == r) tr[u] = {l,r,a[u]};//到达叶子节点
    else{
        tr[u] = {l,r,0};//初始化当前节点
        int mid = (l + r) >> 1;
        build(u << 1, l, mid),build(u << 1 | 1, mid+1, r);
        pushup(u);//从叶子节点回溯到根节点，更新需要的值
    }
}
void modify(int u,int j,int x){//将j上的值加上x
    if(tr[u].l == tr[u].r) tr[u].sum += x;//到达叶节点，先更新叶节点
    else{
        int mid = (tr[u].l + tr[u].r) >> 1;
        if(j <= mid) modify(u << 1,j,x);//需要更新的节点在左边
        if(j > mid) modify(u << 1 | 1,j,x);//需要更新的节点在右边
        pushup(u);//回溯时更新路径上的所有节点
    }
}
int query(int u,int l,int r){
    //查询的区间l,r包含了当前节点u的区间，则直接返回当前区间存储的值
    if(tr[u].l >= l && tr[u].r <= r) return tr[u].sum;
    int ret = 0,mid = (tr[u].l + tr[u].r) >> 1;
    if(l <= mid) ret += query(u << 1,l,r);//左半区间需要查询
    if(r > mid) ret += query(u << 1 | 1,l,r);//右半区间需要查询
    return ret;
}
```

## 前缀和

### 一维

```cpp
a[N],s[N];//下标从1开始读入
s[i] = s[i-1] + a[i];//初始化
s[r] - s[l-1] //区间[l,r]的和
```

### 二维

```cpp
a[N][M],s[N][M];//下标从(1,1)开始读入
s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j];//初始化
//左上角(x1,y1)到右下角(x2,y2)的子矩阵和
s[x2][y2] - s[x1-1][y2] - s[x2][y1-1] + s[x1-1][y1-1]
```

## 差分

### 一维

```cpp
a[N],b[N];//定
void insert(int l,int r,int c){//[l,r]区间内全部+c
    b[l] += c;b[r+1] -= c;
}
insert(i,i,a[i]);//初始化差分数组
for(int i = 1;i <= n;++i) b[i] += b[i-1];//恢复数组
```

### 二维

```cpp
a[N][M],b[N][M];
//左上角(x1,y1)到右下角(x2,y2)全部加上c
void insert(int x1,int y1,int x2,int y2,int c){
    b[x1][y1] += c;
    b[x1][y2+1] -= c;
    b[x2+1][y1] -= c;
    b[x2+1][y2+1] += c;
}
insert(i,j,i,j,a[i][j]);//初始化差分数组
b[i][j] += b[i-1][j] + b[i][j-1] - b[i-1][j-1];//恢复数组
```

## KMP

```cpp
c s[N];
cin >> s+1;
//s[]原始串(长串),p[]模式串(短串),n是s的长度,m是p的长度
for (int i = 2, j = 0; i <= m; i ++){//求next数组
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
}
for (int i = 1, j = 0; i <= n; i ++ ){//匹配
    while (j && s[i] != p[j + 1]) j = ne[j];
    if (s[i] == p[j + 1]) j ++ ;
    if (j == m)
    {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
```

## STL

### priority_queue

```cpp
push(),pop(),top(),empty(),size(),emplace(),swap()
```

### queue

```cpp
front(),pop(),push(),back(),empty(),size(),emplace(),swap()
```

### deque

```cpp
pop_back(),pop_front(),push_back(),push_front(),empty(),size(),back(),front()
at(),begin(),emplace(),emplace_back(),emplace_front(),end(),erase(),insert(),rbegin(),rend()
```

### map

```cpp
insert(),find(),empty(),size(),clear(),count(),lower_bound(),upper_bound()
begin(),emplace(),end(),erase(),equal_range(),rbegin(),rend()
```

### unordered_map

```cpp
insert(),find(),empty(),size(),clear(),count()
begin(),emplace(),end(),erase(),reserve(),swap(),rehash(),hash_function()
```

### set

```cpp
insert(),find(),empty(),size(),clear(),count(),lower_bound(),upper_bound()
begin(),emplace(),end(),erase(),equal_range(),rbegin(),rend()
```

### unordered_set

```cpp
insert(),find(),empty(),size(),clear(),count()
begin(),emplace(),end(),erase(),reserve(),swap(),rehash(),hash_function()
```

### stack

```cpp
push(),pop(),top(),size(),empty(),emplace()
```

# Java

## 快读快写模板

```java
import java.io.*;

static BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
static StreamTokenizer st = new StreamTokenizer(reader);
static PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));//写

public static void main( String[] args ) throws IOException {
    // TODO
    
    pw.flush();
}

static int ri() throws IOException{//读int
    st.nextToken();
    return (int) st.nval;
}
static String rs() throws IOException{//读S
    st.nextToken();
    return st.sval;
}
```

## 排序

### 快排

```java
static void qs(int[] a, int low, int high) {
    if(low >= high) return;
    int i = low - 1, j = high + 1, t = a[low + high >> 1];
    while(i < j){
        do i++; while (a[i] < t);
        do j--; while (a[j] > t);
        if(i < j) {
            int x = a[i];
            a[i] = a[j];
            a[j] = x;
        }
    }
    qs(a, low, j);
    qs(a ,j + 1, high);
}
```


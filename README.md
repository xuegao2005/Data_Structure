# 链表

**数组模拟链表**



## 单链表

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

// head指向链表的头节点（指针）
// e[i]表示下标为i的点的值（数值）
// ne[i]表示下标为i的点的next指针是多少（指针）
// idx表示当前要插入的点的下标（指针）
int head, e[N], ne[N], idx;

// 初始化
void init()
{
  head = -1;
  idx = 0;
}

// 将新值x插入头节点
void add_to_head(int x)
{
  e[idx] = x; // 把新值x存下来
  ne[idx] = head; 
  head = idx ++ ;
}

// 将x插入下标为k的点的后面
void add(int k, int x)
{
  e[idx] = x;
  ne[idx] = ne[k];
  ne[k] = idx ++ ;
}

// 将下标是k的点后面的点删掉
void remove(int k)
{
  ne[k] = ne[ne[k]];
}

int main()
{

}
```



![image-20250605140013168](https://github.com/xuegao2005/Data_Structure/blob/main/image-20250605140013168.png)

![image-20250605145414113](https://github.com/xuegao2005/Data_Structure/blob/main/image-20250605145414113.png)

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int head, e[N], ne[N], idx;

// 初始化
void init()
{
  head = -1;
  idx = 0;
}

// 将x插入头节点
void add_to_head(int x)
{
  e[idx] = x; // 把新值x存下来
  ne[idx] = head; 
  head = idx ++ ;
}

// 将x插入下标为k的点的后面
void add(int k, int x)
{
  e[idx] = x;
  ne[idx] = ne[k];
  ne[k] = idx ++ ;
}

// 将下标是k的点后面的点删掉
void remove(int k)
{
  ne[k] = ne[ne[k]];
}

int main()
{
  int m;
  cin >> m;

  init();

  while(m -- )
  {
    int k, x;
    char op;
    
    cin >> op;
    if (op == 'H')
    {
      cin >> x;
      add_to_head(x);
    }
    else if (op == 'D')
    {
      cin >> k;
      if (!k) head = ne[head];
      remove(k - 1);
    }
    else
    {
      cin >> k >> x;
      add(k - 1, x);
    }
  }

  for (int i = head; i != -1; i = ne[i]) cout << e[i] << ' ';
  cout << endl;

  return 0;
}
```



## 双链表

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int head, e[N], l[N], r[N], idx;

void init()
{
  r[0] = 1, l[1] = 0;
  idx = 2;
}

// 在 下标为k的数 的右边插入 下标idx值为x的数
void add(int k, int x)
{
  e[idx] = x; // 赋值
  r[idx] = r[k]; // 1.
  l[idx] = k; // 2.(可与1调换)
  l[r[k]] = idx; // 3.
  r[k] = idx; // 4.(不可与3调换)
}

// 在 下标为k的数 的左边插入 下标idx值为x的数
// add(l[k], x)

// 删除下标为k的点
void remove(int k)
{
  r[l[k]] = r[k];
  l[r[k]] = l[k];
}

int main()
{
  
}
```



# 栈



```cpp
#include <iostream>

using namespace std;

const int N = 100010;

// tt是栈顶，初始为0
int stk[N], tt;

// 栈顶插入x
stk[ ++ tt ] = x;

// 栈顶元素弹出
t --;

// 判空
if(tt > 0) not empty
else empty

// 栈顶
stk[tt];
```

![image-20250605165206583](https://github.com/xuegao2005/Data_Structure/blob/main/image-20250605165206583.png)

```cpp
#include <iostream>

using namespace std;

const int N = 10010;

int stk[N], tt;
int main()
{
  int n;
  scanf("%d", &n);
  for (int i = 0 ; i < n; i ++ )
  {
    int x; 
    scanf("%d", &x);
    while(tt && stk[tt] >= x) tt -- ;
    if(tt) printf("%d ", stk[tt]);
    else printf("-1 ");
    
    stk[ ++ tt] = x;
  }
  cout << endl;
  return 0;
}
```

*scanf printf 比 cin cout 快10倍*



# 队列



```cpp
#include <iostream>

using namespace std;

const int N = 100010;

// 队尾插入元素，tt初始为-1，队头弹出元素
int q[N], hh, tt = -1;

// 队尾插入元素x
q[ ++ tt] = x;

// 弹出队头元素
h ++ ;

// 判空
if (hh < tt) not empty
else empty

// 取出队头元素
q[hh]
  
// 取出队尾元素
q[tt]
```



## 单调队列

**求滑动窗口的最大值最小值**

![image-20250710153053800](../assets/image-20250710153053800.png)

![image-20250710153112327](../assets/image-20250710153112327.png)

![image-20250710153124627](../assets/image-20250710153124627.png)

```cpp
#include <iostream>

using namespace std;

const int N = 10010;

int n, k;
// 单调队列q[N]存的是下标，队列保持严格递增
int q[N], a[N]; 

int main()
{
  scanf("%d%d", &n, &k);
  
  for (int i = 0 ; i < n ; i ++ ) scanf("%d", &a[i]);
  
  int hh = 0, tt = -1;
  for (int i = 0 ; i < n ; i ++ )
  {
    // 如果队头元素的下标q[hh]小于左边界，说明该元素已不在窗口内，需要从队头移除（hh++）
    if(hh <= tt && q[hh] < i - k + 1) hh ++ ;
    // 当新元素a[i]小于等于队尾元素a[q[tt]]时，队尾元素不可能是未来任何窗口的最小值（因为a[i]更小且位置更靠后），因此将队尾元素移除（tt--），重复此过程直到队尾元素小于a[i]
    while(hh <= tt && a[q[tt]] >= a[i]) tt -- ;
    // 将当前元素的下标i加入队尾，此时队列仍保持递增特性
    q[ ++ tt ] = i;
    // i = k - 1 是第一个窗口，此时队头下标q[hh]对应的a[q[hh]]就是当前窗口的最小值（因为队列递增，队头最小）
    if(i >= k - 1) printf("%d ", a[q[hh]]);
  }
  cout << endl;
  return 0;
}
```



# KMP算法

![image-20250710171453922](../assets/image-20250710171453922.png)

```cpp
#include <iostream>

using namespace std;

const int N = 10010, M = 100010;

int n, m;
char p[N], s[M];
int ne[N];

int main()
{
  cin >> n >> p + 1 >> m >> s + 1;
  
  // 求next数组
  // j 记录当前最长的 “前缀和后缀相等” 的长度
  for (int i = 2, j = 0; i <= n; i ++ )
  {
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
  }
  
  // kmp 匹配过程
  // 用 i 遍历文本 s，用 j 记录当前匹配到模式的哪个位置
  for (int i = 1, j = 0; i <= m; i ++ )
  {
    // 如果 s[i] 和 p[j+1] 不相等，就利用 ne 数组 “回退” j
    while(j && s[i] != p[j + 1]) j = ne[j];
    if(s[i] == p[j + 1]) j ++ ;
    if(j == n)
    {
      // 匹配成功 题目下标是从0开始则输出i - n, 从1开始则为i - n + 1
      printf("%d ", i - n);
      // 继续寻找下一个匹配
      j = ne[j];
    }
  }
  cout << endl;
  return 0;
}
```

# Trie树 字典树

![image-20250711134101587](../assets/image-20250711134101587.png)

![image-20250711134144145](../assets/image-20250711134144145.png)

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

// son[p][u]：节点p的第u个字符子节点（u=0~25对应a~z）
// cnt[p]：节点p作为单词结尾的次数
// idx：当前用到的节点编号（0是根节点）
int son[N][26], cnt[N], idx;
char str[N];

void insert(char str[])
{
  // 根节点开始
  int p = 0 ; 
  for (int i = 0 ; str[i] ; i ++ ) // 遍历字符串每个字符，结尾是0
  {
    // 将字符转为0~25的数字（a=0, b=1, ...）
    int u = str[i] - 'a'; 
    // 如果当前节点p没有u这个子节点，创建新节点，编号为 idx + 1
    if (!son[p][u]) son[p][u] = ++ idx ;
    // 移动到子节点
    p = son[p][u];
  }
  // 标记单词结尾（可能有重复单词，所以计数+1）
  cnt[p] ++ ;
}

int query(char str[])
{
	int p = 0 ;
  for (int i = 0 ; str[i]; i ++ )
  {
    int u = str[i] - 'a' ; 
    // 路径中断，说明单词不存在
    if (!son[p][u]) return 0;
    p = son[p][u];
  }
  // 返回单词出现次数（可能为0，表示不是完整单词）
  return cnt[p];
}

int main()
{
 	int n;
  scanf("%d", &n);
  while(n -- )
  {
    // 操作类型（'I'或'Q'）
    char op[2];
    scanf("%s%s", op, str);
    if (op[0] == 'I') insert(str);
    else printf("%d\n", query(str));
  }

  return 0;
}
```

# 并查集

![image-20250711150527059](../assets/image-20250711150527059.png)

![image-20250711152736928](../assets/image-20250711152736928.png)

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int p[N];

// 返回祖宗节点 + 路径压缩
int find(int x)
{
  // 如果x不是根节点，递归找到根节点，并把x的父节点直接设为根节点
  if (p[x] != x) p[x] = find(p[x]);
  return p[x];
}

int main()
{
	scanf("%d%d", &n, &m);
  
  // 初始化：每个元素自成一个集合，父节点是自己
  for (int i = 0 ; i <= n; i ++ ) p[i] = i;
  
  while (m -- )
  {
    char op[2];
    int a, b; 
    scanf("%s%d%d", op, &a, &b);
    
    // 合并，把a的根节点的父节点设为b的根节点
    if (op[0] == 'M') p[find(a)] = find(b);
		// 查询，a 和 b 是否在一个集合中
    else
    {
      if (find(a) == find(b)) puts("Yes");
      else puts("No");
    }
  }
  return 0;
}
```

变形

![image-20250711160529291](../assets/image-20250711160529291.png)

![image-20250711170613892](../assets/image-20250711170613892.png)

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int p[N];
int size[N]; // 每个集合的元素个数，规定只有根节点的size有意义

// 返回祖宗节点 + 路径压缩
int find(int x)
{
  // 如果x不是根节点，递归找到根节点，并把x的父节点直接设为根节点
  if (p[x] != x) p[x] = find(p[x]);
  return p[x];
}

int main()
{
	scanf("%d%d", &n, &m);
  
  // 初始化：每个元素自成一个集合，父节点是自己
  for (int i = 0 ; i <= n; i ++ ) 
  {
    p[i] = i;
    size[i] = 1;
  }
  
  while (m -- )
  {
    char op[2];
    int a, b; 
    scanf("%s", op);
    
    // 合并
    if (op[0] == 'C') 
    {
      scanf("%d%d", &a, &b);
      // 如果a和b在同一集合，跳过下面步骤
      if (find(a) == find(b)) continue;
      // 合并a和b所在的集合，并更新size
      size[find(b)] += size[find(a)];
      p[find(a)] = find(b);
    }
    
		// 查询是否连通
    else if (op[1] == '1')
    {
      scanf("%d%d", &a, &b);
      if (find(a) == find(b)) puts("Yes");
      else puts("No");
    }
    
    // 查询集合大小
    else
    {
      scanf("%d", &a);
      return size[find(a)];
    }
  }
  return 0;
}
```

# 堆

![image-20250714112453547](../assets/image-20250714112453547.png)

![image-20250714112458048](../assets/image-20250714112458048.png)

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n, m;
int h[N], size;

void down(int u)
{
  // t 表示三个点的最小点
  int t = u;
  // 左儿子存在 且 左儿子更小，则更新最小点
  if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
  if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
	// 如果根节点不是最小值，则最小值换位置到根节点
  if (h[u] != h[t])
  {
    swap(h[u], h[t]);
    down(t);
  }
}

int main()
{
  scanf("%d%d", &n, &m);
  for (int i = 1 ; i <= n; i ++ ) scanf("%d", &h[i]);
  size = n; 
  
  for (int i = n; i ; i -- ) down(i);
  
  while(m -- )
  {
    printf("%d ", h[1]);
    // 删除堆顶元素
    h[1] = h[size]; 
    size --;
    down(1);
  }
  
  puts("");
  return 0;
}
```

![image-20250714151631621](../assets/image-20250714151631621.png)

![image-20250714151649463](../assets/image-20250714151649463.png)

![image-20250714152425775](../assets/image-20250714152425775.png)

![image-20250714152804014](../assets/image-20250714152804014.png)



# 哈希表

![image-20250721105307395](../assets/image-20250721105307395.png)

![image-20250721105412884](../assets/image-20250721105412884.png)

## 拉链法

有insert函数和find函数

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 100003;

int h[N], e[N], ne[N], idx;

void insert(int x)
{
  int k = (x % N + N) % N;
  
  e[idx] = x;
  ne[idx] = h[k];
  h[k] = idx ++ ;
}

bool find(int x)
{
  int k = (x % N + N) % N;
  for (int i = h[k]; i != -1; i = ne[i])
    if (e[i] == x)
      return true;
  
  return false;
}

int main()
{
  int n;
  scanf("%d", &n);
  
  memset(h, -1, sizeof h);
  
  while(n -- )
  {
    char op[2];
    int x;
    scanf("%s%d", op, &x);
    
    if (*op == 'I') insert(x);
    else 
    {
      if (find(x)) puts("Yes");
      else puts("No");
		}
  }
  return 0;
}
```

## 开放寻址法

只有一个find函数，类比上厕所

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 200003, null = 0x3f3f3f3f;

int h[N];

int find(int x)
{
  int k = (x % N + N) % N;
  
  while (h[k] != null && h[k] != x)
  {
    k ++ ;
    if ( k == N) k =0 ; 
  }
  
  return k;
}

int main()
{
  int n;
  scanf("%d", &n);
  
  memset(h, 0x3f, sizeof h);
  
  while(n -- )
  {
    char op[2];
    int x;
    scanf("%s%d", op, &x);
    
    int k = find(x);
    if (*op == 'I') h[k] = x;
    else 
    {
      if (h[k] != null) puts("Yes");
      else puts("No");
		}
  }
  return 0;
}
```

两个方法任选



## 字符串前缀哈希法

字符串哈希方式

![image-20250722104541671](../assets/image-20250722104541671.png)

![image-20250722104559830](../assets/image-20250722104559830.png)

```cpp
#include<iostream>

using namespace std;

typedef unsigned long long ULL;

const int N = 100010, P = 131;

int n, m;
char str[N];
ULL h[N], p[N];

ULL get(int l, int r)
{
  return h[r] - h[l - 1] * p[r - l + 1];
}

int main()
{
  scanf("%d%d%s", &n, &m, str + 1);
  
  p[0] = 1;
  for (int i = 1; i <= n; i ++ )
  {
    p[i] = p[i - 1] * P;
    h[i] = h[i - 1] * P + str[i];
  }
  
  while (m -- )
  {
    int l1, r1, l2, r2;
    scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
    
    if ( get(l1, r1) == get(l2, r2) ) puts("Yes");
    else puts("No");
  }
  
  return 0;
}
```




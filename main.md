# ACM 竞赛模板

## C++ 基础模板

```cpp
#include <bits/stdc++.h>
#pragma GCC optimize("O2")
#pragma GCC target("avx")   // 开启 AVX 指令集，提高效率
using namespace std;

// 常用宏定义
#define ll long long
#define ull unsigned long long
#define INF 0x3f3f3f3f
#define LLINF 0x3f3f3f3f3f3f3f3f
#define mod 1000000007      // 常用模数
#define endl '\n'

// 快速读入（处理 1e6+ 数据）
inline int read() {
    int x = 0, f = 1; char ch = getchar();
    while (ch < '0' || ch > '9') { if (ch == '-') f = -1; ch = getchar(); }
    while (ch >= '0' && ch <= '9') { x = x * 10 + ch - '0'; ch = getchar(); }
    return x * f;
}

// 快速输出（处理大量输出）
inline void write(int x) {
    if (x < 0) { putchar('-'); x = -x; }
    if (x > 9) write(x / 10);
    putchar(x % 10 + '0');
}

// 快速读入 long long
inline long long readLL() {
    long long x = 0, f = 1; char ch = getchar();
    while (ch < '0' || ch > '9') { if (ch == '-') f = -1; ch = getchar(); }
    while (ch >= '0' && ch <= '9') { x = x * 10 + ch - '0'; ch = getchar(); }
    return x * f;
}
// 快速输出 long long
inline void writeLL(long long x) {
    if (x < 0) { putchar('-'); x = -x; }
    if (x > 9) writeLL(x / 10);
    putchar(x % 10 + '0');
}

// cin/cout 优化
inline void init_io() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
}

int main() {
    init_io();      // 若使用 cin/cout，调用此函数；若使用 read/write，可注释

    // ===== 输入区 =====
    int n = read(); // 示例：快速读入

    // ===== 逻辑处理区 =====

    // ===== 输出区 =====
    write(n);       // 示例：快速输出

    return 0;
}
```

## C++ 新标准核心技巧

### 范围 for 循环

> 简化容器遍历语法，避免手动操作迭代器，支持 `const` 引用避免拷贝

```cpp
vector<int> vec = {1, 2, 3, 4, 5};
// 修改元素
for (auto &x : vec) x *= 2;
// 只读访问
for (const auto &x : vec) cout << x << ' ';

// map 遍历
map<int, string> mp = {{1, "a"}, {2, "b"}};
for (const auto &[key, value] : mp)
    cout << key << ": " << value << endl;
```

### std::to_string

> 将数值转换为字符串，比 `stringstream` 更简洁

```cpp
int i = 42;
double d = 3.14159;
float f = 2.718f;
long long ll = 123456789012345LL;

string s1 = to_string(i);      // "42"
string s2 = to_string(d);      // "3.141590"
string s3 = to_string(f);      // "2.718000"
string s4 = to_string(ll);     // "123456789012345"
string result = "Value: " + to_string(i) + ", Pi: " + to_string(d);
```

### std::stoi / std::stoll / std::stod

> 字符串转数值，支持异常处理、进制转换和范围检查

```cpp
string str1 = "123", str2 = "1010", str3 = "3.14", str4 = "FF";

int x   = stoi(str1);                     // 123
int y   = stoi(str2, nullptr, 2);         // 二进制转十进制：10
long long z = stoll("123456789012");      // 123456789012
double d   = stod(str3);                   // 3.14
int hex   = stoi(str4, nullptr, 16);      // 十六进制转十进制：255

// 带异常处理
try {
    int num = stoi("abc");               // 抛出 std::invalid_argument
} catch (const exception &e) {
    cout << "Error: " << e.what() << endl;
}
```

### std::gcd

> 计算最大公约数，自动取绝对值

```cpp
#include <numeric>
int a = 12, b = 18;
int g1 = gcd(a, b);            // 6
int g2 = gcd(-12, 18);         // 6
long long g3 = gcd(24LL, 36LL); // 12

// 分数约分
int num = 15, den = 25;
int common = gcd(num, den);
num /= common; den /= common;   // 3 / 5

// 多数求 gcd
int g = gcd(gcd(a, b), 24);    // 6
```

### std::lcm

> 计算最小公倍数，防止中间结果溢出

```cpp
#include <numeric>
int a = 12, b = 18;
int l1 = lcm(a, b);            // 36
int l2 = lcm(-12, 18);         // 36
long long l3 = lcm(24LL, 36LL); // 72

// 多数求 lcm
int l = lcm(lcm(a, b), 24);    // 72

// 大数防溢出
int x = 1000000000, y = 1000000000;
int res = lcm(x, y);
```

### std::bit_count / std::popcount

> 统计二进制中 `1` 的个数

```cpp
#include <bit>
unsigned int n1 = 5;   // 101
unsigned int n2 = 7;   // 111
unsigned long long n3 = 15; // 1111

int c1 = bit_count(n1); // 2
int c2 = bit_count(n2); // 3
int c3 = bit_count(n3); // 4

bool isPow2 = bit_count(n1) == 1;
int hamming = bit_count(a ^ b);
int parity = bit_count(n1) % 2;
```

### std::views::iota

> 生成惰性整数序列，节省内存

```cpp
#include <ranges>
#include <algorithm>

// 有界序列
for (int i : views::iota(1, 5)) cout << i << ' '; // 1 2 3 4

// 有限序列（配合 take）
auto first10 = views::iota(0) | views::take(10);
for (int i : first10) cout << i << ' '; // 0 … 9

// 与算法配合
vector<int> v;
ranges::copy(views::iota(1, 6), back_inserter(v));
```

## STL 高频库函数

### `std::stack`

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | `stack<T> s;` | 创建空栈 |
| 访问 | `s.top();` | **返回栈顶元素的引用**（常用） |
| 容量 | `s.empty();` | 判断是否为空 |
|      | `s.size();` | 返回元素个数 |
| 修改 | `s.push(val);` | 将 `val` 压入栈顶（常用） |
|      | `s.emplace(args…);` | 栈顶原地构造（C++11） |
|      | `s.pop();` | **弹出栈顶元素**（不返回值） |

### `std::queue`

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | `queue<T> q;` | 创建空队列 |
| 访问 | `q.front();` | **返回队首元素的引用**（常用） |
|      | `q.back();` | 返回队尾元素的引用 |
| 容量 | `q.empty();` | 判断是否为空 |
|      | `q.size();` | 返回元素个数 |
| 修改 | `q.push(val);` | 将 `val` 入队（常用） |
|      | `q.emplace(args…);` | 队尾原地构造（C++11） |
|      | `q.pop();` | **弹出队首元素**（不返回值） |

### `std::priority_queue`

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | `priority_queue<T> pq;` | 大顶堆 |
|      | `priority_queue<T, vector<T>, greater<T>> pq;` | 小顶堆 |
| 访问 | `pq.top();` | **返回最高优先级元素的引用**（常用） |
| 容量 | `pq.empty();` | 判断是否为空 |
|      | `pq.size();` | 返回元素个数 |
| 修改 | `pq.push(val);` | 插入并调整堆结构（常用） |
|      | `pq.emplace(args…);` | 原地构造并插入（C++11） |
|      | `pq.pop();` | **移除最高优先级元素**（不返回值） |

### 序列容器

**`vector`、`deque`、`list`、`forward_list`、`array`、`string`**

#### 通用接口

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | `C c;` | 默认构造 |
|      | `C c(n, val);` | 包含 `n` 个 `val` 的容器 |
|      | `C c(begin, end);` | 用迭代器范围构造 |
|      | `C c{1,2,3};` | 列表初始化（C++11） |
| 赋值 | `c = {1,2,3};` | 列表赋值 |
|      | `c.assign(n, val);` | 赋值 `n` 个 `val` |
|      | `c.assign(begin, end);` | 用迭代器范围赋值 |
| 访问 | `c[i]` | **下标访问（不检查越界）** |
|      | `c.at(i)` | **带边界检查** |
|      | `c.front()`、`c.back()` | 返回首/尾元素引用（`forward_list` 没有 `back`） |
|      | `c.data()` | 返回底层数组指针（`array`、`vector`、`string`） |
| 迭代 | `c.begin()/c.end()` | 正向迭代器 |
|      | `c.rbegin()/c.rend()` | 反向迭代器（`forward_list` 没有） |
| 容量 | `c.empty()`、`c.size()` | 判断是否为空、返回元素个数 |
|      | `c.capacity()`、`c.reserve(n)` | 仅对 `vector`、`string` 有效 |
|      | `c.shrink_to_fit()` | 请求释放多余容量（`vector`、`deque`、`string`） |
| 修改 | `c.clear()` | **清空容器** |
|      | `c.insert(pos, val)` | 在 `pos` 前插入 |
|      | `c.emplace(pos, args…)` | 在 `pos` 前原地构造（C++11） |
|      | `c.erase(pos)`、`c.erase(b,e)` | 删除单个或范围 |
|      | `c.push_back(val)` | **在尾部添加**（`forward_list` 没有） |
|      | `c.emplace_back(args…)` | 尾部原地构造（C++11） |
|      | `c.pop_back()` | **删除尾部元素**（`forward_list` 没有） |
|      | `c.resize(n)` | 改变容器大小 |

#### 特殊接口

##### `deque`

| 操作 | 描述 |
|---|---|
| `push_front(val)` / `emplace_front(args …)` | 在 **头部** 插入一个新元素（拷贝或原位构造）。 |
| `pop_front()` | 删除 **第一个** 元素。 |

##### `list`

| 操作 | 描述 |
|---|---|
| `push_front(val)` / `emplace_front(args …)` | 在 **头部** 插入新元素。 |
| `pop_front()` | 删除 **第一个** 元素。 |
| `c.remove(val)` | 删除 **所有等于 `val`** 的节点。 |
| `c.remove_if(pred)` | 删除 **满足谓词 `pred`** 的所有节点。 |
| `c.sort()` | 对链表 **就地排序**（默认使用 `<`，可自定义比较器）。 |
| `c.merge(other)` | 合并 **两个已排序** 的链表 `c` 与 `other`，结果仍保持有序。 |
| `c.splice(pos, other)` | 把 `other` 中的所有元素 **移动到 `pos` 前**，不产生拷贝或构造。 |

##### `forward_list`

| 操作 | 描述 |
|---|---|
| `push_front(val)` / `emplace_front(args …)` | 在 **头部** 插入新元素。 |
| `pop_front()` | 删除 **第一个** 元素。 |
| `c.remove(val)` | 删除 **所有等于 `val`** 的节点。 |
| `c.remove_if(pred)` | 删除 **满足谓词 `pred`** 的所有节点。 |
| `c.sort()` | 对单向链表 **就地排序**（同 `list`）。 |
| `c.merge(other)` | 合并 **两个已排序** 的 `forward_list`，保持有序。 |
| `c.splice(pos, other)` | 把 `other` 中的所有元素 **移动到 `pos` 前**（`forward_list` 实际上提供 `splice_after`，此处按照题目给出的统一写法）。 |
| `c.before_begin()` | 返回指向 **第一个元素之前** 的迭代器（相当于“哑结点”）。 |
| `insert_after(it, val)` / `emplace_after(it, args …)` | 在迭代器 `it` **之后** 插入元素（拷贝或原位构造）。 |
| `erase_after(it)` | 删除 **紧随 `it` 之后** 的元素。 |

### 关联容器与无序关联容器

**`set`、`map`、`multiset`、`multimap`** 以及对应的 `unordered_` 版本

#### 通用接口

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | 与序列容器类似 | |
| 容量 | `empty()`、`size()` | |
| 修改 | `clear()` | 清空容器 |
|      | `insert(val)` / `insert({k,v})` | **插入元素** |
|      | `emplace(args…)` | 原地构造并插入 |
|      | `erase(key)`、`erase(it)` | **按键或迭代器删除** |
| 查找 | `c.find(key)` | **查找键，返回迭代器**（极其常用） |
|      | `c.count(key)` | 返回键出现次数 |
|      | `c.lower_bound(key)` | 返回 **≥ key** 的迭代器（有序容器） |
|      | `c.upper_bound(key)` | 返回 **> key** 的迭代器（有序容器） |
|      | `c.equal_range(key)` | 返回匹配键的范围（pair） |

#### 具体容器特性

| 容器类型 | 操作 | 描述 |
|----------|------|------|
| `map` | `map[key]` | **若 key 不存在则插入默认值并返回引用**（最常用） |
|      | `map.at(key)` | 存在返回引用，不存在抛异常 |
| 有序容器 | `c.begin()` | 返回按键排序后的首元素 |
| 无序容器 | `c.bucket_count()` | 桶的数量 |
|      | `c.load_factor()` | 负载因子 |
|      | `c.rehash(n)` | 至少 `n` 桶 |
|      | `c.reserve(n)` | 预留空间，可容纳至少 `n` 元素 |

### `std::pair`

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | `pair<T1,T2> p;` | 默认构造，`first` 与 `second` 值初始化 |
|      | `pair<T1,T2> p(v1,v2);` | 用 `v1`,`v2` 初始化 |
|      | `p = {v1,v2};` | 列表初始化（C++11） |
| 访问 | `p.first`、`p.second` | **访问两成员** |
| 比较 | `p1 == p2`、`p1 < p2` … | 按字典序比较 |
| 辅助 | `tie(a,b) = p;` | 解包到变量（需 `<tuple>`） |

### `std::bitset`

| 类别 | 操作 | 描述 |
|------|------|------|
| 构造 | `bitset<N> b;` | 默认全 0 |
|      | `bitset<N> b("1010");` | 从字符串初始化（左侧为高位） |
| 访问 | `b[pos]` | 访问第 `pos` 位（不检查） |
|      | `b.test(pos)` | 带边界检查的访问 |
|      | `b.count()` | **返回 1 的个数** |
|      | `b.all()`、`b.any()`、`b.none()` | 全部为 1、至少有 1、全部为 0 |
| 容量 | `b.size()` | 位数 `N` |
| 位操作 | `b.set()`、`b.set(pos,val)` | 设置全部或单个位 |
|      | `b.reset()`、`b.reset(pos)` | 清零全部或单个位 |
|      | `b.flip()`、`b.flip(pos)` | 翻转全部或单个位 |
| 转换 | `b.to_string()`、`b.to_ulong()`、`b.to_ullong()` | 转为字符串或整数（越界抛异常） |
| 运算符 | `& \| ^ ~ << >>` | 按位运算与位移 |
| IO | `cin >> b`、`cout << b` | 支持输入输出（读取 0/1） |

## 高频算法库

### 自定义比较

```cpp
// 方式 1：greater<int>
priority_queue<int, vector<int>, greater<int>> minHeap1;
minHeap1.push(5); minHeap1.push(2); minHeap1.push(8);
cout << minHeap1.top();   // 2

// 方式 2：自定义比较函数
auto cmp = [](int l, int r) { return l > r; };
priority_queue<int, vector<int>, decltype(cmp)> minHeap2(cmp);
```

### sort

> 基本排序与常用变体

```cpp
vector<int> v = {5,2,8,1,9,3,7,4,6};

// 默认升序
sort(v.begin(), v.end());

// 降序
sort(v.begin(), v.end(), greater<int>());

// 自定义规则（按个位数）
sort(v.begin(), v.end(),
     [](int a, int b){ return a % 10 < b % 10; });

// 稳定排序
stable_sort(v.begin(), v.end());

// 前 k 小元素有序
partial_sort(v.begin(), v.begin() + 5, v.end());
```

### lower_bound / upper_bound

> 二分查找（序列必须有序）

```cpp
vector<int> a = {1,2,3,3,3,4,5,6,7};

auto lb = lower_bound(a.begin(), a.end(), 3); // 第一个 >= 3
auto ub = upper_bound(a.begin(), a.end(), 3); // 第一个 > 3

int cnt = ub - lb;           // 3 个 3
bool exists = binary_search(a.begin(), a.end(), 3);

// 插入保持有序
a.insert(lower_bound(a.begin(), a.end(), 3), 3);
```

#### nth_element / unique

```cpp
// O(n) 均摊复杂度求第 k 小元素
vector<int> nums = {5, 2, 8, 1, 9, 3, 7, 4, 6};
int k = 4; // 找第 4 小的元素（下标为 3）
nth_element(nums.begin(), nums.begin() + k, nums.end());
// 调用后，nums[k] 就是第 k 小的数，nums[0..k-1] 都小于等于它

// 去重（常用于离散化）
vector<int> arr = {1, 2, 2, 3, 3, 3, 4};
sort(all(arr)); // unique 前必须排序
auto last = unique(all(arr));
arr.erase(last, arr.end());
// arr 现在是 {1, 2, 3, 4}
```

* **`nth_element`**：求第 k 大/小元素时，用 `O(n)` 的 `nth_element` 代替 `O(n log n)` 的 `sort`，可以避免 TLE。
* **`unique`**：是**离散化**的标准步骤之一，几乎所有需要坐标压缩的题目都会用到。

### 其他常用算法

```cpp
vector<int> v = {1,2,3,4,5};

// 反转
reverse(v.begin(), v.end());

// 最大/最小
int mx = *max_element(v.begin(), v.end());
int mn = *min_element(v.begin(), v.end());

// 累加、乘积
int sum = accumulate(v.begin(), v.end(), 0);
double prod = accumulate(v.begin(), v.end(), 1.0, multiplies<double>());

// 填充
fill(v.begin(), v.end(), 0);
fill_n(v.begin(), 3, 1);

// 复制
vector<int> dst(v.size());
copy(v.begin(), v.end(), dst.begin());
copy_if(v.begin(), v.end(), dst.begin(),
        [](int x){ return x % 2 == 0; });

// 查找
auto it = find(v.begin(), v.end(), 3);
auto it2 = find_if(v.begin(), v.end(),
                   [](int x){ return x > 3; });

// 计数
int evenCnt = count_if(v.begin(), v.end(),
                       [](int x){ return x % 2 == 0; });
```

## 错误检查

### DEBUG

```cpp
// DEBUG 宏，只在本地编译时生效
#ifdef LOCAL
#define DEBUG(x) cerr << #x << " = " << (x) << endl
#else
#define DEBUG(x)
#endif

// 使用示例
int my_var = 42;
DEBUG(my_var);        // 本地会输出 "my_var = 42"，提交到 OJ 则这行代码无效
DEBUG(vec.size());
```

### 环境差异导致的错误

* `scanf` / `printf` 在 Linux 下使用 `%I64d` 可能输出错误。

### 常见会导致 CE 的错误

* 拼写错误：`int mian()` → `int main()`。
* `struct` / `class` 后漏写分号。
* 数组过大、使用不被 OJ 允许的函数（如多线程）。
* 参数类型不匹配、函数声明未定义。
* `goto` / `switch` 跨越未初始化的局部变量。

### 会产生 Warning 的错误

* `=` 与 `==` 混用。
* 运算符优先级导致的逻辑错误。
* `static` 使用不当。
* `scanf` 漏写 `&`。
* 格式化符与参数类型不匹配。
* 位运算与逻辑运算混用未加括号，例如 `(x >> j) & 3 == 2`。
* 整型字面量溢出。
* 未初始化局部变量。
* 变量遮蔽（`-Wshadow` 可检查）。
* 重载运算符后导致的输出错误，例如 `cout << (1 << 1);`。

### 只会导致 WA、RE、TLE、MLE 的错误（编译器不可直接发现）

> 常见的逻辑错误，供对照检查。

* **WA**：未清空容器、负数读取漏判、类型宽度不足导致溢出、编号偏移（0/1）错误、混用 `cin`/`scanf`、宏展开缺括号、hash 使用有符号数等。
* **RE**：除零、数组越界、未定义行为、非法指针、重复 `delete`、非法 `new[]` 部分释放等。
* **TLE**：递归边界缺失、死循环、BFS 未标记、频繁调用高复杂度函数、宏实现的 `min/max` 影响常数。
* **MLE**：数组/容器过大、循环中无限插入、未释放不再使用的内存。

## 技巧

### 循环宏

```cpp
#define f(i,l,r) for (int i = (l), __ = (r); i < __; ++i)
#define _rep(i,a,b) for (int i = (a); i <= (b); ++i)
```

使用示例：

```cpp
f(i,0,N) { /*...*/ }
_rep(j,1,10) { /*...*/ }
```

### 内存池

在大量小对象分配时，使用预分配的内存池可以显著提升性能并避免碎片：

```cpp
// 静态数组池示例
int* newarr(int sz) {
    static int pool[MAXN], *p = pool;
    int* ret = p;
    p += sz;
    return ret;
}

// 线段树节点池
struct Node { /*...*/ };
Node* newnode() {
    static Node pool[MAXN << 1], *p = pool - 1;
    return ++p;
}
```

> 大多数 OI 题目可以在开始时一次性申请足够的内存，再在程序中复用。

### 自定义哈希

```cpp
// 针对 C++11/14/17 的 unordered_map 防 TLE 技巧
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};

// 使用示例:
// unordered_map<long long, int, custom_hash> mp;
```

### `__int128`

当题目数值范围超过 `long long` (约 9e18) 但又没到需要高精度的程度时，`__int128` 是一个完美的解决方案。

```cpp
// __int128 的使用
using i128 = __int128;

// __int128 的读写需要自己实现
void print_i128(i128 x) {
    if (x < 0) { putchar('-'); x = -x; }
    if (x > 9) print_i128(x / 10);
    putchar(x % 10 + '0');
}

// 示例
i128 a = 1000000000000000000LL; // 注意 LL 后缀
i128 b = a * a;
print_i128(b); // 输出 10^36
```

## 算法

好的，这是一份为你精心准备的 ACM 算法竞赛模板。

模板按照你的要求，涵盖了从基础技巧、搜索到动态规划等多个方面，并严格遵循你指定的格式。为了更好的组织结构和可用性，我将内容分为三大类：**基础算法与技巧**、**搜索**、**动态规划**。这样划分更加符合算法的内在逻辑。

---

### 基础算法与技巧

#### 计数排序 (Counting Sort)

##### 适用场景

* **时空复杂度**：时间复杂度 O(n + k)，空间复杂度 O(k)，其中 n 是待排序元素数量，k 是元素的数值范围。
* **适用问题**：适用于待排序数据是整数，且范围 k 不比 n 大很多的情况。它是一种非比较排序，稳定。

##### 核心代码

```cpp
#include <vector>
#include <algorithm>

// 对非负整数数组进行计数排序
// arr: 待排序数组
// maxValue: 数组中的最大值
void countingSort(std::vector<int>& arr, int maxValue) {
    if (arr.empty()) return;

    // - 创建计数数组，大小为 maxValue + 1
    std::vector<int> count(maxValue + 1, 0);

    // 2. 统计每个元素出现的次数
    for (int x : arr) {
        count[x]++;
    }

    // 3. (可选，用于稳定排序) 将计数数组修改为存储元素最终位置的前缀和
    // for (int i = 1; i <= maxValue; ++i) {
    //     count[i] += count[i - 1];
    // }

    // 4. 回填原数组
    int arr_idx = 0;
    for (int i = 0; i <= maxValue; ++i) {
        while (count[i] > 0) {
            arr[arr_idx++] = i;
            count[i]--;
        }
    }
}
```

##### 使用补充

* **稳定性**：上面给出的简版实现是不稳定的。要实现稳定排序，需要额外 O(n) 的空间，并从后向前遍历原数组来放置元素（如注释掉的第3步配合一个从后到前的回填循环）。

2. **负数处理**：如果数组中包含负数，可以通过添加一个偏移量（offset）将所有数转换成非负数，排序后再转换回去。
3. **核心思想**：空间换时间。

#### 基数排序 (Radix Sort)

##### 适用场景

* **时空复杂度**：时间复杂度 O(d * (n + k))，空间复杂度 O(n + k)，其中 d 是数字的最大位数，k 是基数（通常为10），n 是元素数量。
* **适用问题**：适用于整数排序，特别是当数值很大但位数不多时。可以处理比计数排序更大的范围。

##### 核心代码

```cpp
#include <vector>
#include <algorithm>

// 获取数字 x 的第 d 位 (d=1 表示个位)
int getDigit(int x, int d) {
    return (x / d) % 10;
}

// 基于计数排序的子过程，对数组按照某一位进行排序
void countSortForRadix(std::vector<int>& arr, int d) {
    int n = arr.size();
    if (n == 0) return;

    std::vector<int> output(n);
    std::vector<int> count(10, 0); // 基数是10

    for (int i = 0; i < n; i++) {
        count[getDigit(arr[i], d)]++;
    }

    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    for (int i = n - 1; i >= 0; i--) {
        int digit = getDigit(arr[i], d);
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    arr = output;
}

void radixSort(std::vector<int>& arr) {
    if (arr.empty()) return;

    int maxVal = *std::max_element(arr.begin(), arr.end());

    // 从最低位(个位)到最高位，依次进行排序
    for (int d = 1; maxVal / d > 0; d *= 10) {
        countSortForRadix(arr, d);
    }
}
```

##### 使用补充

* **依赖稳定排序**：基数排序的正确性依赖于其内部使用的排序算法是稳定的。计数排序是稳定的，因此是理想的选择。

2. **LSD vs MSD**：代码中实现的是 LSD（Least Significant Digit first）基数排序，这是最常见的。
3. **适用范围**：不仅限于整数，也可用于字符串等可按“位”比较的数据。

#### 桶排序 (Bucket Sort)

##### 适用场景

* **时空复杂度**：平均时间复杂度 O(n + k)，最坏时间复杂度 O(n^2)。空间复杂度 O(n + k)，k 是桶的数量。
* **适用问题**：适用于待排序数据能均匀、独立地分布在一个确定范围内的情况。

##### 核心代码

```cpp
#include <vector>
#include <algorithm>
#include <cmath>

void bucketSort(std::vector<float>& arr) {
    int n = arr.size();
    if (n == 0) return;

    // - 创建桶
    std::vector<std::vector<float>> buckets(n);

    // 2. 将元素放入对应的桶中
    // 假设 arr 中的元素在 [0, 1) 范围
    for (int i = 0; i < n; i++) {
        int bucketIndex = n * arr[i];
        buckets[bucketIndex].push_back(arr[i]);
    }

    // 3. 对每个桶内部进行排序 (可使用插入排序等)
    for (int i = 0; i < n; i++) {
        std::sort(buckets[i].begin(), buckets[i].end());
    }

    // 4. 合并所有桶
    int index = 0;
    for (int i = 0; i < n; i++) {
        for (float val : buckets[i]) {
            arr[index++] = val;
        }
    }
}
```

##### 使用补充

* **数据分布**：性能严重依赖于输入数据的分布。如果所有数据都落入同一个桶，算法退化为 O(n^2)。

2. **映射函数**：将元素值映射到桶索引的函数是关键。对于 `[minVal, maxVal]` 范围的整数，映射函数可以是 `(arr[i] - minVal) * (num_buckets - 1) / (maxVal - minVal)`。
3. **通用性**：是一种思想框架，内部排序可以用任何其他排序算法。

#### 锦标赛排序 (Tournament Sort / Tree Sort)

##### 适用场景

* **时空复杂度**：完全排序时间复杂度 O(n log n)，选出前 k 个最小元素时间复杂度 O(n + k log n)。空间复杂度 O(n)。
* **适用问题**：在流数据中找最小值/最大值，或者找前 k 个最值。其思想和堆排序非常相似。

##### 核心代码

```cpp
#include <vector>
#include <queue>
#include <functional> // for std::greater

// 使用优先队列(最小堆)来模拟锦标赛排序，找出前 k 小的元素
std::vector<int> tournamentSelect(const std::vector<int>& data, int k) {
    if (data.empty() || k == 0) return {};
  
    // 最小堆
    std::priority_queue<int, std::vector<int>, std::greater<int>> pq;
    for (int val : data) {
        pq.push(val);
    }

    std::vector<int> result;
    k = std::min(k, (int)data.size());
    for (int i = 0; i < k; ++i) {
        result.push_back(pq.top());
        pq.pop();
    }
    return result;
}
```

##### 使用补充

* **与堆排序关系**：锦标赛排序的“淘汰赛”过程本质上就是建堆的过程。选出最小的元素后，将其“替换”并重新比赛的过程，就是堆的 `pop` 和 `push` 操作。

2. **实现**：在竞赛中，除非题目有特殊要求，一般直接使用 `std::priority_queue` 来实现其思想，而不是手动建一棵“赢者树”。

#### Tim 排序 (Tim Sort)

##### 适用场景

* **时空复杂度**：最坏和平均时间复杂度 O(n log n)，最优时间复杂度 O(n)。空间复杂度在最坏情况下 O(n)，通常为 O(log n) 或更小。
* **适用问题**：一种混合、稳定的排序算法，对真实世界中包含大量“有序片段”（run）的数据表现极好。是 Python 和 Java 的默认排序算法。

##### 核心代码

```cpp
// TimSort 的完整实现非常复杂，不适合在比赛中手写。
// 下面是其核心思想的伪代码/概念描述。

/*
- 寻找连续的升序或降序片段 (natural runs)。
   - 如果是降序，则原地反转。
   - 如果 run 的长度太短，使用插入排序将其扩展到最小长度 (minrun)。

2. 将这些 runs 存入一个栈中。

3. 反复合并栈顶的 runs，直到栈满足特定条件（维持平衡，避免合并代价过大的 runs）。
   - 合并过程类似于归并排序的 merge 操作。
   - 合并策略是智能的，会利用 runs 的长度来优化。

// 在 C++ 中，我们通常依赖 `std::stable_sort`，其底层实现可能就是 Timsort 或类似的归并排序变体。
*/
```

##### 使用补充

* **竞赛中的角色**：主要用于理解其思想——即混合排序的优势，以及对部分有序数据的优化。竞赛中直接用 `std::sort` (Introsort) 或 `std::stable_sort` (通常是归并排序变体)。

2. **优势**：结合了插入排序（对小数组和有序数组高效）和归并排序（最坏情况 O(n log n)）的优点。

#### 前缀和 & 差分 (Prefix Sum & Difference Array)

##### 适用场景

* **前缀和**：对**静态**数组进行频繁的区间和查询。预处理 O(n)，查询 O(1)。
* **差分**：对数组进行频繁的**区间增减**操作，最后统一查询。单次区间修改 O(1)，查询单点值需要 O(n)（通过前缀和复原）。

##### 核心代码

```cpp
// --- 1D 前缀和 ---
std::vector<long long> prefixSum;
void buildPrefixSum(const std::vector<int>& arr) {
    int n = arr.size();
    prefixSum.assign(n + 1, 0);
    for (int i = 0; i < n; ++i) {
        prefixSum[i + 1] = prefixSum[i] + arr[i];
    }
}
// 查询 [l, r] (闭区间, 0-indexed) 的和
long long querySum(int l, int r) {
    return prefixSum[r + 1] - prefixSum[l];
}

// --- 1D 差分 ---
std::vector<int> diff;
// 构建差分数组
void buildDifference(const std::vector<int>& arr) {
    int n = arr.size();
    diff.assign(n, 0);
    diff[0] = arr[0];
    for (int i = 1; i < n; ++i) {
        diff[i] = arr[i] - arr[i - 1];
    }
}
// 将 [l, r] (闭区间, 0-indexed) 的所有元素加上 val
void updateRange(int l, int r, int val) {
    diff[l] += val;
    if (r + 1 < diff.size()) {
        diff[r + 1] -= val;
    }
}
// 由差分数组复原原数组
std::vector<int> restoreFromDiff() {
    int n = diff.size();
    std::vector<int> restoredArr(n);
    restoredArr[0] = diff[0];
    for(int i = 1; i < n; ++i) {
        restoredArr[i] = restoredArr[i-1] + diff[i];
    }
    return restoredArr;
}
```

##### 使用补充

* **逆运算**：差分和前缀和是互为逆运算。对差分数组求前缀和，可以得到原数组。

2. **二维**：前缀和与差分可以扩展到二维。
    * 二维前缀和：`S[i][j] = S[i-1][j] + S[i][j-1] - S[i-1][j-1] + A[i][j]`
    * 二维差分：对 `(x1, y1)` 到 `(x2, y2)` 的矩形加 `c`，等价于 `d[x1][y1]+=c`, `d[x2+1][y1]-=c`, `d[x1][y2+1]-=c`, `d[x2+1][y2+1]+=c`。

#### 二分 (Binary Search)

##### 适用场景

* **时空复杂度**：时间复杂度 O(log N)，N 为搜索范围大小。
* **适用问题**：
    * 在**有序序列**中查找特定值/边界。
    2. **二分答案**：在一个有单调性的值域上查找满足条件的最小/最大答案。

##### 核心代码

```cpp
// 模板1: 查找第一个 >= target 的位置 (类似 std::lower_bound)
// 在 [left, right] 区间内搜索
int binary_search_lower_bound(const std::vector<int>& arr, int target) {
    int left = 0, right = arr.size(); // 搜索区间 [left, right)
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] >= target) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left; // left 是第一个 >= target 的位置
}

// 模板2: 二分答案, 查找满足 check(x) 的最小值
// 假设答案范围是 [low, high]
bool check(int x) {
    // 判断 x 是否是一个合法的/可能的答案
    // 这个函数具有单调性：如果 check(x) 为 true, 则所有 y > x, check(y) 也为 true
    return true; 
}

int binary_search_answer() {
    int low = 0, high = 1e9; // 答案的可能范围
    int ans = high;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (check(mid)) {
            ans = mid;
        }
    }
    return ans;
}
```

##### 使用补充

* **边界条件**：二分最容易出错的地方是 `l` 和 `r` 的更新方式以及循环条件 (`<` 还是 `<=`)。推荐记忆固定、可靠的模板，如上所示的左闭右开区间模板。

2. **单调性**：二分答案的核心是找到问题的单调性，并构造 `check` 函数。`check` 函数的返回值决定了搜索区间的收缩方向。
3. **浮点数二分**：对于浮点数，循环条件可以改为固定次数（如100次）或 `r - l > eps`。

#### 倍增 (Binary Lifting)

##### 适用场景

* **时空复杂度**：预处理 O(n log n)，单次查询 O(log n)。
* **适用问题**：处理树上或序列上的“静态”查询，如LCA（最近公共祖先）、树上路径问题、k级祖先等。

##### 核心代码

```cpp
// 倍增求 LCA (Lowest Common Ancestor)
const int MAXN = 100005;
const int LOGN = 17; // ceil(log2(MAXN))

std::vector<int> adj[MAXN];
int depth[MAXN];
int parent[MAXN][LOGN]; // parent[u][j] 是 u 的第 2^j 个祖先

void dfs_lca(int u, int p, int d) {
    depth[u] = d;
    parent[u][0] = p;
    for (int v : adj[u]) {
        if (v != p) {
            dfs_lca(v, u, d + 1);
        }
    }
}

void build_lca(int root, int n) {
    dfs_lca(root, -1, 0);
    for (int j = 1; j < LOGN; ++j) {
        for (int i = 1; i <= n; ++i) {
            if (parent[i][j - 1] != -1) {
                parent[i][j] = parent[parent[i][j - 1]][j - 1];
            } else {
                parent[i][j] = -1;
            }
        }
    }
}

int lca(int u, int v) {
    if (depth[u] < depth[v]) std::swap(u, v);

    // - 将 u, v 提升到同一深度
    for (int j = LOGN - 1; j >= 0; --j) {
        if (depth[u] - (1 << j) >= depth[v]) {
            u = parent[u][j];
        }
    }

    if (u == v) return u;

    // 2. 同时向上跳，直到他们的父节点相同
    for (int j = LOGN - 1; j >= 0; --j) {
        if (parent[u][j] != -1 && parent[u][j] != parent[v][j]) {
            u = parent[u][j];
            v = parent[v][j];
        }
    }
    return parent[u][0];
}
```

##### 使用补充

* **预处理**：核心是 `parent[u][j] = parent[parent[u][j-1]][j-1]` 这个递推式，它高效地计算出所有节点的 `2^j` 级祖先。

2. **应用扩展**：倍增表不仅可以存祖先，还可以存路径上的其他信息，如最大/小权值，从而解答树上路径的查询。
3. **ST表**：倍增思想也用于构建ST表，解决RMQ（区间最值查询）问题。

#### 构造 (Constructive Algorithms)

##### 适用场景

* **复杂度不定**：取决于具体问题。
* **适用问题**：不要求找到最优解，而是要求给出一个符合特定条件的具体方案或例子。

##### 核心代码

构造题没有统一的核心代码，它是一种思维方式。但有一些常见的策略：

```cpp
// 示例：构造一个 0 到 n-1 的排列 p，使得 p[i] != i
std::vector<int> construct_derangement(int n) {
    if (n == 1) return {}; // 无解

    std::vector<int> p(n);
    for (int i = 0; i < n; ++i) {
        p[i] = i;
    }

    // 一个简单的构造: 循环移位
    for (int i = 0; i < n - 1; ++i) {
        std::swap(p[i], p[i+1]);
    }
  
    // checker (for verification, not part of construction)
    // if p[n-1] == n-1 after rotation, swap with n-2
    // A simpler construction: p[i] = (i+1)%n works for all n>1
  
    std::vector<int> result(n);
    for(int i = 0; i < n; ++i) {
        result[i] = (i + 1) % n;
    }
    return result;
}
```

##### 使用补充

* **常用思路**：
    * **从特殊到一般**：先考虑 n=1, 2, 3 等小规模情况，寻找规律。
    * **数学性质**：利用奇偶性、模运算、质数、对称性等。
    * **归纳/递归**：假设 `n-1` 的解已知，如何构造 `n` 的解。
    * **贪心**：每一步都做出局部最优或最“安全”的选择。
    * **随机化**：在某些情况下，随机方案有很大概率满足条件。
    * **模式化**：构造棋盘染色、循环移位、对称填充等模式。

2. **关键**：大胆猜测，小心验证。

### 搜索

#### DFS（深度优先搜索）

##### 适用场景

* **时空复杂度**：图遍历 O(V+E)，状态空间搜索可能指数级。空间复杂度 O(H)，H 为最大搜索深度。
* **适用问题**：遍历图/树，寻找路径，检测环，拓扑排序，解决隐式的图/树问题（如排列、组合、子集）。

##### 核心代码

```cpp
// 图的 DFS
const int MAXN = 100005;
std::vector<int> adj[MAXN];
bool visited[MAXN];

void dfs(int u) {
    visited[u] = true;
    // process node u
    std::cout << u << " "; 

    for (int v : adj[u]) {
        if (!visited[v]) {
            dfs(v);
        }
    }
}

// 示例：生成全排列
void generate_permutations(std::vector<int>& nums, int start, std::vector<std::vector<int>>& result) {
    if (start == nums.size()) {
        result.push_back(nums);
        return;
    }
    for (int i = start; i < nums.size(); ++i) {
        std::swap(nums[start], nums[i]);
        generate_permutations(nums, start + 1, result);
        std::swap(nums[start], nums[i]); // 回溯
    }
}
```

##### 使用补充

* **栈溢出**：递归深度过大时可能导致栈溢出，此时需要手写栈来模拟递归。

2. **状态**：在图遍历中，节点通常有“未访问”、“访问中”、“已访问”三种状态，用于判环等复杂场景。
3. **应用**：是回溯法、记忆化搜索等高级搜索技术的基础。

#### BFS（广度优先搜索）

##### 适用场景

* **时空复杂度**：O(V+E)。空间复杂度 O(W)，W 为图的最大宽度。
* **适用问题**：在**无权图**中寻找**最短路径**，层序遍历。

##### 核心代码

```cpp
#include <queue>
#include <vector>

// 图的 BFS，返回从 start 到各点的最短距离
std::vector<int> bfs(int start, int n) {
    std::vector<int> dist(n + 1, -1);
    std::queue<int> q;

    q.push(start);
    dist[start] = 0;
  
    while (!q.empty()) {
        int u = q.front();
        q.pop();

        for (int v : adj[u]) { // adj is the adjacency list
            if (dist[v] == -1) { // not visited yet
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
    return dist;
}
```

##### 使用补充

* **队列**：BFS 的核心数据结构是队列，保证了按层次的遍历顺序。

2. **状态记录**：必须有一个 `visited` 数组或 `dist` 数组来防止节点重复入队，避免死循环和效率低下。
3. **多源BFS**：可以将所有源点同时入队，并将其初始距离设为0，可以用来解决“所有A点到最近的B点的距离”这类问题。

#### 双向搜索 (Bidirectional Search)

##### 适用场景

* **时空复杂度**：时间复杂度 O(b^(d/2))，空间复杂度 O(b^(d/2))，b是分支因子，d是解的深度。
* **适用问题**：当起点和终点都明确，且状态空间巨大时。它从起点和终点同时开始搜索，期望在中间相遇。

##### 核心代码

```cpp
#include <queue>
#include <map>
#include <string>

// 假设状态是 string，通过 op(state) 得到下一状态
// 返回最短转换步数
int bidirectional_bfs(std::string start, std::string end) {
    if (start == end) return 0;
  
    std::queue<std::string> q_s, q_e;
    std::map<std::string, int> dist_s, dist_e;

    q_s.push(start);
    dist_s[start] = 0;
    q_e.push(end);
    dist_e[end] = 0;

    while (!q_s.empty() && !q_e.empty()) {
        // 扩展较小的一端
        if (q_s.size() <= q_e.size()) {
            std::string u = q_s.front(); q_s.pop();
            // for v in neighbours of u
            // std::string v = op(u); 
            // if (dist_s.find(v) == dist_s.end()) {
            //     dist_s[v] = dist_s[u] + 1;
            //     if (dist_e.count(v)) {
            //         return dist_s[v] + dist_e[v];
            //     }
            //     q_s.push(v);
            // }
        } else {
            std::string u = q_e.front(); q_e.pop();
            // for v in neighbours of u (inverse operation)
            // std::string v = op_inv(u);
            // if (dist_e.find(v) == dist_e.end()) {
            //     dist_e[v] = dist_e[u] + 1;
            //     if (dist_s.count(v)) {
            //         return dist_s[v] + dist_e[v];
            //     }
            //     q_e.push(v);
            // }
        }
    }
    return -1; // No path found
}

```

##### 使用补充

* **相遇判断**：扩展一个节点后，需要检查它是否已经被另一方的搜索访问过。

2. **空间换时间**：双向搜索的优势在于大大减少了搜索的节点数，但代价是需要存储两边的 `visited` 信息，内存开销更大。
3. **操作可逆**：如果问题的操作不可逆，从终点向起点的搜索需要定义“逆操作”。

#### 启发式搜索 (Heuristic Search)

##### 适用场景

* **时空复杂度**：高度依赖于启发函数的质量。最优情况下接近 O(V+E)，最差退化为指数级。
* **适用问题**：在加权图中寻找最短路径，特别是当图巨大或隐式（如游戏地图、谜题状态空间）时。

##### 核心代码

见下面的 `A*` 算法，`A*` 是最著名的一种启发式搜索。

##### 使用补充

* **启发函数 h(n)**：估计从节点 `n` 到目标节点的“未来代价”。一个好的启发函数应该 **易于计算** 且 **信息量足**。
* **贪心最佳优先搜索 (Greedy Best-First Search)**：只用 `h(n)` 作为评估标准，速度快但不保证最优解。
* **A\***：同时考虑 `g(n)` (从起点到 `n` 的已知代价) 和 `h(n)` (未来预估代价)，即 `f(n) = g(n) + h(n)`。

#### A*

##### 适用场景

* 同启发式搜索，是其最优性实现。
* **适用问题**：在加权图中寻找最优路径，且存在一个“可采纳”的启发函数。

##### 核心代码

```cpp
#include <queue>
#include <vector>
#include <cmath> // For heuristic calculation

struct Node {
    int id;
    int g; // cost from start
    int h; // heuristic cost to end
  
    int f() const { return g + h; }

    bool operator>(const Node& other) const {
        return f() > other.f(); // For min-priority_queue
    }
};

// 启发函数 (例如，网格中的曼哈顿距离)
int heuristic(int node_id, int goal_id) {
    // int x1 = ..., y1 = ...;
    // int x2 = ..., y2 = ...;
    // return abs(x1 - x2) + abs(y1 - y2);
    return 0; // Trivial heuristic, degenerates to Dijkstra
}

// A* 算法
// graph[u] = {{v1, cost1}, {v2, cost2}, ...}
int a_star(int start_id, int goal_id, const std::vector<std::pair<int, int>> graph[]) {
    std::priority_queue<Node, std::vector<Node>, std::greater<Node>> pq;
    std::vector<int> g_cost(100005, -1);

    pq.push({start_id, 0, heuristic(start_id, goal_id)});
    g_cost[start_id] = 0;

    while (!pq.empty()) {
        Node current = pq.top();
        pq.pop();

        if (current.id == goal_id) {
            return current.g;
        }

        // 如果找到了更优路径到达 current.id, 则忽略此节点
        if (current.g > g_cost[current.id] && g_cost[current.id] != -1) {
            continue;
        }

        for (auto const& [neighbor_id, edge_cost] : graph[current.id]) {
            int new_g = current.g + edge_cost;
            if (g_cost[neighbor_id] == -1 || new_g < g_cost[neighbor_id]) {
                g_cost[neighbor_id] = new_g;
                pq.push({neighbor_id, new_g, heuristic(neighbor_id, goal_id)});
            }
        }
    }
    return -1; // Goal not reachable
}

```

##### 使用补充

* **可采纳性 (Admissibility)**： `h(n)` 必须小于或等于从 `n` 到终点的实际最短代价。这是A*算法保证最优解的关键。如果 `h(n)` 高估了代价，A* 可能找到次优解。

2. **一致性 (Consistency)**： `h(n) <= cost(n, n') + h(n')`。一致的启发函数一定是可采纳的。它能保证一个节点被扩展时，到达它的路径就是最优路径，类似 Dijkstra。
3. **与 Dijkstra 的关系**：如果 `h(n)` 恒为0，A*算法退化为 Dijkstra 算法。

#### 迭代加深搜索 (Iterative Deepening DFS, IDDFS)

##### 适用场景

* **时空复杂度**：时间复杂度 O(b^d)，空间复杂度 O(d)，其中 b 是分支因子，d 是解的深度。
* **适用问题**：当搜索树深度未知或很大，但解的深度相对较浅时。它结合了DFS的空间优势和BFS能找到最短路（按层数）的优势。适用于答案步数/层数有要求，但状态空间会爆炸的问题。

##### 核心代码

```cpp
#include <vector>

bool found = false;
int max_depth;

// DLS: Depth Limited Search
void dls(int current_node, int target_node, int depth) {
    if (found) return;
    if (depth > max_depth) return;
    if (current_node == target_node) {
        found = true;
        return;
    }

    // for (int neighbor : adj[current_node]) {
    //     if (!visited[neighbor]) { // 路径上判重
    //         visited[neighbor] = true;
    //         dls(neighbor, target_node, depth + 1);
    //         visited[neighbor] = false; // 回溯
    //     }
    // }
}

// IDDFS 主函数
void iddfs(int start_node, int target_node) {
    for (max_depth = 0; ; ++max_depth) {
        found = false;
        // clear visited states if necessary
        dls(start_node, target_node, 0);
        if (found) {
            // std::cout << "Found at depth " << max_depth << std::endl;
            return;
        }
        // 如果 max_depth 很大仍未找到，可能无解，可以设置一个上限
    }
}
```

##### 使用补充

* **重复搜索**：IDDFS 会重复搜索上层节点，但由于大部分节点都在搜索树的底层，所以重复搜索的代价相对较小，时间复杂度与BFS同级。
* **适用性**：非常适合于那些分支因子很小，或者解的深度不大的问题。如果解很深，IDDFS的效率会降低。
* **与BFS对比**：当BFS因为队列过大而内存超限 (MLE) 时，IDDFS是一个绝佳的替代品。

#### IDA\* (Iterative Deepening A\*)

##### 适用场景

* **时空复杂度**：时间复杂度依赖于启发函数质量，空间复杂度 O(d)，d是解的深度。
* **适用问题**：A\* 算法的迭代加深版本。当状态空间巨大，A\* 的优先队列会消耗过多内存时，IDA\* 是一个很好的选择。它能找到最优解，且空间开销极小。

##### 核心代码

```cpp
#include <algorithm>

int threshold; // 当前的 f(n) 代价上限
int next_threshold; // 下一次迭代的最小代价上限
bool found;

// 启发函数 h(n)，估计从 n 到终点的代价
int heuristic(int node) { /* ... */ return 0; }

// state 可以是节点编号，也可以是更复杂的状态结构
void ida_dfs(int g_cost, int current_state) {
    if (found) return;

    int h_cost = heuristic(current_state);
    int f_cost = g_cost + h_cost;

    if (f_cost > threshold) {
        next_threshold = std::min(next_threshold, f_cost);
        return;
    }

    if (is_goal(current_state)) {
        found = true;
        return;
    }

    // for (auto& next_move : possible_moves(current_state)) {
    //     int edge_cost = get_cost(next_move);
    //     int next_state = apply_move(current_state, next_move);
    //     ida_dfs(g_cost + edge_cost, next_state);
    //     if (found) return;
    // }
}

void ida_star(int start_state) {
    threshold = heuristic(start_state);
    while (!found) {
        next_threshold = 1e9; // Infinity
        // clear visited path if needed
        ida_dfs(0, start_state);
      
        if (found) break; // Solution found
        if (next_threshold == 1e9) break; // No solution exists

        threshold = next_threshold;
    }
}
```

##### 使用补充

* **阈值 (threshold)**：IDA\* 的核心是 `threshold`。它不是搜索深度，而是代价函数 `f(n) = g(n) + h(n)` 的上限。
* **下一次阈值**：每次 `ida_dfs` 搜索失败后，`next_threshold` 记录了所有“刚好”超过当前阈值的 `f_cost` 中的最小值。这保证了下一次迭代能探索到新的节点。
* **优点**：结合了 A\* 的最优性和 IDDFS 的低内存消耗。

#### 回溯法 (Backtracking)

##### 适用场景

* **时空复杂度**：指数级，取决于搜索空间大小和剪枝效率。
* **适用问题**：寻找一个问题的所有解或任意一个解，如排列、组合、子集、N皇后、数独等。本质是带有**剪枝**的深度优先搜索。

##### 核心代码

```cpp
// N 皇后问题示例
std::vector<std::vector<std::string>> solutions;
std::vector<std::string> board;
int n;

bool is_valid(int row, int col) {
    // 检查列
    for (int i = 0; i < row; ++i) if (board[i][col] == 'Q') return false;
    // 检查左上对角线
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j) if (board[i][j] == 'Q') return false;
    // 检查右上对角线
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; --i, ++j) if (board[i][j] == 'Q') return false;
    return true;
}

void solve(int row) {
    // - 终止条件
    if (row == n) {
        solutions.push_back(board);
        return;
    }

    // 2. 遍历选择
    for (int col = 0; col < n; ++col) {
        if (is_valid(row, col)) {
            // 2a. 做出选择
            board[row][col] = 'Q';
            // 2b. 递归
            solve(row + 1);
            // 2c. 撤销选择 (回溯)
            board[row][col] = '.';
        }
    }
}

void solveNQueens(int size) {
    n = size;
    board.assign(n, std::string(n, '.'));
    solve(0);
}
```

##### 使用补充

* **三要素**：**选择 (Choice)**、**约束 (Constraint)**、**目标 (Goal)**。代码中的 `for` 循环是选择，`is_valid` 是约束，`row == n` 是目标。
* **框架**：`solve(参数)` -> `判断终止` -> `循环遍历所有选择` -> `剪枝(判断合法性)` -> `做出选择` -> `solve(下一层)` -> `撤销选择`。
* **与DFS的关系**：回溯法是DFS在解决特定问题时的一种具体实现范式。

#### Dancing Links (DLX)

##### 适用场景

* **时空复杂度**：对于精确覆盖问题，其效率远超普通回溯，但仍然是指数级的。
* **适用问题**：专门用于高效解决**精确覆盖问题 (Exact Cover)**。许多谜题问题（如数独、多米诺骨牌覆盖）都可以转化为精确覆盖问题。

##### 核心代码

```cpp
// DLX 的实现非常精巧且固定，通常作为模板使用。
// 核心是构建一个十字循环双向链表来表示 0-1 矩阵。
namespace DLX {
    const int MAX_NODES = 1e5; // 根据问题规模调整
    int L[MAX_NODES], R[MAX_NODES], U[MAX_NODES], D[MAX_NODES];
    int col[MAX_NODES], row[MAX_NODES];
    int H[MAX_COLS], S[MAX_COLS]; // H 头指针, S 每列大小
    int ans[MAX_ROWS], node_count;

    void init(int m) { // m是列数
        for (int i = 0; i <= m; ++i) {
            L[i] = i - 1, R[i] = i + 1;
            U[i] = D[i] = i; S[i] = 0;
        }
        L[0] = m, R[m] = 0;
        node_count = m + 1;
        memset(H, -1, sizeof(H));
    }

    void link(int r, int c) {
        S[c]++; col[node_count] = c; row[node_count] = r;
        U[node_count] = c; D[node_count] = D[c];
        U[D[c]] = node_count; D[c] = node_count;
        if (H[r] < 0) H[r] = L[node_count] = R[node_count] = node_count;
        else {
            R[node_count] = R[H[r]]; L[R[H[r]]] = node_count;
            L[node_count] = H[r]; R[H[r]] = node_count;
        }
        node_count++;
    }

    void remove(int c) {
        L[R[c]] = L[c]; R[L[c]] = R[c];
        for (int i = D[c]; i != c; i = D[i])
            for (int j = R[i]; j != i; j = R[j]) {
                U[D[j]] = U[j]; D[U[j]] = D[j]; --S[col[j]];
            }
    }

    void resume(int c) {
        for (int i = U[c]; i != c; i = U[i])
            for (int j = L[i]; j != i; j = L[j])
                U[D[j]] = D[U[j]] = j, ++S[col[j]];
        L[R[c]] = R[L[c]] = c;
    }

    bool dance(int k) {
        if (R[0] == 0) return true; // 所有列被覆盖
        int c = R[0];
        for (int i = R[0]; i != 0; i = R[i]) if (S[i] < S[c]) c = i;
        remove(c);
        for (int i = D[c]; i != c; i = D[i]) {
            ans[k] = row[i];
            for (int j = R[i]; j != i; j = R[j]) remove(col[j]);
            if (dance(k + 1)) return true;
            for (int j = L[i]; j != i; j = L[j]) resume(col[j]);
        }
        resume(c);
        return false;
    }
}
```

##### 使用补充

* **精确覆盖**：给定一个0-1矩阵，选出一些行，使得这些行构成的子矩阵每列恰好有1个1。
* **建模**：解决问题的关键是如何将原问题建模成一个0-1矩阵。
* **启发式**：`dance`函数中选择`S[i]`最小的列`c`进行覆盖是一个重要的启发式优化，它能显著减少搜索分支。

#### Alpha‑Beta 剪枝 (Alpha‑Beta Pruning)

##### 适用场景

* **时空复杂度**：最优情况下为O(b^(d/2))，是Minimax的平方根。
* **适用问题**：在二人零和博弈中（如井字棋、象棋）优化 Minimax 算法，大幅削减搜索树的分支。

##### 核心代码

```cpp
const int INF = 1e9;

// player: 1 for maximizing, -1 for minimizing
int minimax(int depth, int player, int alpha, int beta) {
    // 评估当前局面的分数
    if (depth == 0 || is_game_over()) {
        return evaluate_board();
    }

    if (player == 1) { // Maximizing player
        int max_eval = -INF;
        // for (auto& move : legal_moves()) {
        //     make_move(move);
        //     int eval = minimax(depth - 1, -1, alpha, beta);
        //     undo_move(move);
        //     max_eval = std::max(max_eval, eval);
        //     alpha = std::max(alpha, eval);
        //     if (beta <= alpha) {
        //         break; // Beta cut-off
        //     }
        // }
        return max_eval;
    } else { // Minimizing player
        int min_eval = INF;
        // for (auto& move : legal_moves()) {
        //     make_move(move);
        //     int eval = minimax(depth - 1, 1, alpha, beta);
        //     undo_move(move);
        //     min_eval = std::min(min_eval, eval);
        //     beta = std::min(beta, eval);
        //     if (beta <= alpha) {
        //         break; // Alpha cut-off
        //     }
        // }
        return min_eval;
    }
}

// Initial call
// minimax(MAX_DEPTH, 1, -INF, INF);
```

##### 使用补充

* **Alpha**： maximizing player 已经确保能得到的**最低**分数。
* **Beta**： minimizing player 已经确保能得到的**最高**分数。
* **剪枝条件**：`beta <= alpha`。这意味着 minimizing player 找到一个走法比 maximizing player 已经拥有的最好选择还要差，所以 maximizing player 绝不会让局面走到这里，后续的搜索可以被剪掉。
* **搜索顺序**：剪枝的效率高度依赖于走法的搜索顺序。如果能先搜到最好的走法，剪枝效率会最大化。

#### 搜索优化

##### 适用场景

* 适用于任何搜索算法，旨在通过各种技巧缩小搜索空间或加速搜索过程。

##### 核心代码

无统一代码，是方法论的集合。

* **可行性剪枝**：在搜索过程中，如果当前状态无论如何都无法达到合法解，则立即返回。（例如，背包问题中当前物品重量已超过容量）。
* **最优性剪枝**：如果当前状态加上一个乐观的估计（启发函数）后，仍然不如已找到的最优解，则立即返回。（例如，旅行商问题中，当前路径长度+到终点的最短可能距离 > 已知最短回路）。
* **搜索顺序优化**：优先搜索最有可能产生解或剪枝的分支。例如：
    * 在DLX中选择节点最少的列。
    * 在图的搜索中，优先扩展度数较小的节点。
* **对称性破除**：对于对称的解，只搜索其中一个。例如N皇后问题，第一个皇后只需放在棋盘的前半部分，因为后半部分是对称的。
* **状态压缩/哈希**：用一个整数（位运算）、字符串或哈希值来表示复杂的状态，方便存入`visited`集合或`memo`表。
* **预处理**：在搜索前进行一些计算，减少搜索中的重复工作。

##### 使用补充

这些优化技巧是提高搜索程序效率、防止超时的关键。一道题能否通过，往往取决于是否使用了有效的剪枝和优化策略。

#### 记忆化搜索 (Memoization)

##### 适用场景

* **时空复杂度**：时间复杂度等于 **状态数 × 转移复杂度**，空间复杂度等于 **状态数**。
* **适用问题**：当递归/搜索过程中，存在大量**重叠子问题**时。它是自顶向下实现动态规划的一种方式。

##### 核心代码

```cpp
#include <cstring> // for memset

const int MAXN = 101;
long long memo[MAXN]; // 记忆化数组

// 斐波那契数列示例
long long fib(int n) {
    // - 基线条件
    if (n <= 1) return n;

    // 2. 检查备忘录
    if (memo[n] != -1) {
        return memo[n];
    }
  
    // 3. 递归计算
    long long result = fib(n - 1) + fib(n - 2);

    // 4. 存入备忘录并返回
    return memo[n] = result;
}

int main() {
    // 初始化备忘录为-1
    memset(memo, -1, sizeof(memo));
    // long long ans = fib(50);
    return 0;
}
```

##### 使用补充

* **与DP的关系**：记忆化搜索是“自顶向下”的DP，而通常用循环实现的DP是“自底向上”的。两者在解决相同问题时，本质和复杂度是一样的。
* **优点**：
    * **直观**：代码结构和暴力递归几乎一样，易于思考和编写。
    * **自动跳过无效状态**：只计算从初始状态可达的状态，对于状态空间稀疏的问题更高效。
* **初始化**：记忆化数组必须初始化为一个特殊值（如-1），以区分“已计算但结果是0”和“未计算”两种情况。

---

### 动态规划

#### 背包 DP (Knapsack DP)

##### 适用场景

* **时空复杂度**：
    * 0/1背包: O(NW), 空间优化后 O(W)
    * 完全背包: O(NW), 空间优化后 O(W)
    * 多重背包: O(NW * log(count)) 或 O(NW), 取决于二进制优化还是单调队列优化。
* **适用问题**：给定一组物品，每个物品有自己的重量(或代价)和价值，在一个有限的背包容量内，如何选择物品使得总价值最大/方案数最多。

##### 核心代码

```cpp
// N: 物品数量, W: 背包容量
// w[i]: 第i个物品的重量, v[i]: 第i个物品的价值
int w[N], v[N];
int dp[W+1];

// 0/1 背包 (每个物品只能选一次)
// 空间优化: O(W)
void knapsack01() {
    memset(dp, 0, sizeof(dp));
    for (int i = 1; i <= N; ++i) {
        for (int j = W; j >= w[i]; --j) { // 必须逆序
            dp[j] = std::max(dp[j], dp[j - w[i]] + v[i]);
        }
    }
    // dp[W] is the answer
}

// 完全背包 (每个物品可以选无限次)
// 空间优化: O(W)
void knapsackComplete() {
    memset(dp, 0, sizeof(dp));
    for (int i = 1; i <= N; ++i) {
        for (int j = w[i]; j <= W; ++j) { // 必须正序
            dp[j] = std::max(dp[j], dp[j - w[i]] + v[i]);
        }
    }
    // dp[W] is the answer
}

// 多重背包 (第i个物品最多选c[i]次)
// 二进制拆分优化: O(NW log(c_max))
void knapsackMultiple() {
    // 将多重背包问题转换为0/1背包问题
    std::vector<std::pair<int, int>> items;
    for (int i = 1; i <= N; ++i) {
        int count = c[i];
        for (int k = 1; k <= count; k *= 2) {
            items.push_back({w[i] * k, v[i] * k});
            count -= k;
        }
        if (count > 0) {
            items.push_back({w[i] * count, v[i] * count});
        }
    }
    // 然后对 items 跑一遍 0/1 背包
    // ...
}
```

##### 使用补充

* **滚动数组/一维优化**：核心在于内外层循环的定义和内层循环的顺序。0/1背包的逆序保证了`dp[j-w[i]]`是上一轮（`i-1`）的状态；完全背包的正序保证了`dp[j-w[i]]`是本轮（`i`）的状态，实现了物品的重复选取。
* **初始化**：如果要求**恰好装满**，则 `dp[0]` 初始化为0，其余初始化为 `-INF`。如果可以不装满，则全部初始化为0。
* **变种**：求方案数（`dp[j] += dp[j - w[i]]`）、求最优价值下的最小重量等。

#### 区间 DP (Interval DP)

##### 适用场景

* **时空复杂度**：通常是 O(n^3) 或 O(n^2)。空间复杂度 O(n^2)。
* **适用问题**：问题可以分解为求解子区间的性质，并且大区间的解可以由小区间的解合并而来。形式如：求整个序列 `[1, n]` 的最优解，需要先知道 `[1, k]` 和 `[k+1, n]` 等子区间的解。

##### 核心代码

```cpp
#include <algorithm>

const int MAXN = 505;
int arr[MAXN];
int dp[MAXN][MAXN];

// 经典问题：石子合并
void intervalDP(int n) {
    // 初始化: dp[i][i] = 0 (长度为1的区间)
    // for (int i = 1; i <= n; ++i) dp[i][i] = 0;

    // - 枚举区间长度 len
    for (int len = 2; len <= n; ++len) {
        // 2. 枚举区间左端点 i
        for (int i = 1; i <= n - len + 1; ++i) {
            // 3. 计算右端点 j
            int j = i + len - 1;
          
            dp[i][j] = 1e9; // 初始化为无穷大
            int sum_cost = /* calculate cost for merging interval [i, j], e.g., sum of arr[i..j] */;

            // 4. 枚举分割点 k
            for (int k = i; k < j; ++k) {
                dp[i][j] = std::min(dp[i][j], dp[i][k] + dp[k+1][j] + sum_cost);
            }
        }
    }
    // The answer is dp[1][n]
}
```

##### 使用补充

* **状态定义**：`dp[i][j]` 通常表示区间 `[i, j]` 的最优解。
* **转移方程**：`dp[i][j] = max/min(dp[i][k] + dp[k+1][j] + cost(i, j, k))`，其中 `k` 是 `[i, j]` 之间的分割点。
* **循环顺序**：最外层必须是**区间长度**从小到大，这样在计算大区间 `dp[i][j]` 时，其依赖的子区间 `dp[i][k]` 和 `dp[k+1][j]` 都已经被计算出来了。

#### DAG 上的 DP (DP on Directed Acyclic Graph)

##### 适用场景

* **时空复杂度**：时间 O(V+E)，空间 O(V)，V是节点数，E是边数。
* **适用问题**：当问题的状态转移关系可以构成一个有向无环图时。例如，在一个有向无环图中寻找最长路/最短路、统计路径数量等。许多DP问题都可以建模成DAG上的DP。

##### 核心代码

通常有两种实现方式：记忆化搜索（自顶向下）和拓扑排序（自底向上）。记忆化搜索更直观。

```cpp
#include <vector>
#include <algorithm>

const int MAXN = 100005;
std::vector<std::pair<int, int>> adj[MAXN]; // {neighbor, weight}
int dp[MAXN]; // dp[i] 表示从节点 i 出发的最长路径长度
bool visited[MAXN];

// 记忆化搜索求解 DAG 最长路
int solve_dag(int u) {
    if (visited[u]) {
        return dp[u];
    }

    visited[u] = true;
    dp[u] = 0; // 如果 u 是终点，路径长度为0

    for (auto const& [v, weight] : adj[u]) {
        dp[u] = std::max(dp[u], solve_dag(v) + weight);
    }

    return dp[u];
}

// 使用前需要初始化 visited 和 dp 数组
// e.g., memset(visited, false, sizeof(visited));
// for (int i = 1; i <= n; ++i) if (!visited[i]) solve_dag(i);
// 最终答案是所有 dp[i] 的最大值
```

##### 使用补充

* **建模**：关键在于将问题抽象成图。DP的状态是图的节点，状态之间的转移是图的有向边。
* **拓扑排序**：另一种方法是先对图进行拓扑排序，然后按照拓扑序的逆序进行DP。这样可以保证在计算一个节点 `u` 时，它所有后继节点 `v` 的DP值都已经被计算出来。
* **应用**：很多线性DP（如最长递增子序列）可以看作是在一个隐式DAG上的最长路问题。

#### 树形 DP (Tree DP)

##### 适用场景

* **时空复杂度**：通常是 O(N) 或 O(N * K)，其中 N 是树的节点数，K 是附加状态的维度（如背包容量）。
* **适用问题**：在树形结构上进行动态规划。问题的解可以通过递归地组合子树的解来获得。

##### 核心代码

通常通过一次或两次DFS来完成。第一次DFS从叶子到根（后序遍历）更新信息，第二次DFS（可选）从根到叶子传递信息。

```cpp
#include <vector>
#include <algorithm>

const int MAXN = 100005;
std::vector<int> adj[MAXN];
int dp[MAXN][2]; // dp[u][0]: u不选的最大权重; dp[u][1]: u选的最大权重

// 经典问题：树的最大权独立集
// weight[u] 是节点 u 的权重
void tree_dp(int u, int parent) {
    dp[u][1] = weight[u]; // 选择 u，则初始权重为 weight[u]
    dp[u][0] = 0;         // 不选择 u，则初始权重为 0

    for (int v : adj[u]) {
        if (v == parent) continue;
      
        tree_dp(v, u); // 先递归计算子节点

        // 状态转移
        dp[u][1] += dp[v][0]; // 如果选了 u，则其子节点 v 都不能选
        dp[u][0] += std::max(dp[v][0], dp[v][1]); // 如果不选 u，则其子节点 v 可选可不选，取大的
    }
}

// Initial call: tree_dp(root, -1);
// Answer: std::max(dp[root][0], dp[root][1]);
```

##### 使用补充

* **核心思想**：一个节点的DP值，由其所有子节点的DP值转移而来。
* **换根DP**：对于需要求“以每个节点为根的答案”这类问题，通常需要两次DFS。第一次求出以某个固定点（如1）为根的答案，第二次DFS在换根的过程中，利用父节点的信息 O(1) 地修正答案。
* **树上背包**：将背包问题和树形DP结合，`dp[u][j]` 表示在以`u`为根的子树中，用容量为`j`的背包能获得的最大价值。合并子树信息时类似分组背包。

#### 状压 DP (State Compression DP)

##### 适用场景

* **时空复杂度**：通常是 O(N^2 *2^N) 或 O(M* N * 3^M) 等指数级形式。
* **适用问题**：当DP的一维状态是某个集合的子集，且集合大小很小（通常 N <= 20）时，可以用一个整数的二进制位来表示这个集合状态。

##### 核心代码

```cpp
#include <algorithm>
#include <cstring>

const int MAXN = 20;
const int INF = 1e9;

int n;
int dist[MAXN][MAXN]; // 城市间的距离
int dp[1 << MAXN][MAXN]; // dp[mask][i]: 访问过 mask 集合中的城市，当前在城市 i 的最短路程

// 旅行商问题 (TSP)
int tsp() {
    memset(dp, 0x3f, sizeof(dp));
    dp[1][0] = 0; // 从城市 0 出发，初始状态是 {0}

    // - 枚举所有状态 mask
    for (int mask = 1; mask < (1 << n); ++mask) {
        // 2. 枚举当前状态 mask 中的终点 i
        for (int i = 0; i < n; ++i) {
            if (mask & (1 << i)) { // 如果城市 i 在集合中
                // 3. 枚举上一个城市 j，尝试从 j 转移到 i
                for (int j = 0; j < n; ++j) {
                    if (j != i && (mask & (1 << j))) {
                        int prev_mask = mask ^ (1 << i); // 从 mask 中去掉 i 的状态
                        dp[mask][i] = std::min(dp[mask][i], dp[prev_mask][j] + dist[j][i]);
                    }
                }
            }
        }
    }

    int ans = INF;
    for (int i = 0; i < n; ++i) {
        ans = std::min(ans, dp[(1 << n) - 1][i] + dist[i][0]); // 最后回到城市 0
    }
    return ans;
}
```

##### 使用补充

* **位运算**：熟练掌握位运算是使用状压DP的基础。
    * `mask | (1 << i)`: 将第 `i` 个元素加入集合。
    * `mask & (1 << i)`: 检查第 `i` 个元素是否在集合中。
    * `mask ^ (1 << i)`: 从集合中移除第 `i` 个元素（如果存在）。
* **轮廓线DP**：是状压DP在棋盘问题上的一种应用，通常状态压缩的是棋盘的一条“轮廓线”上的信息，如插头DP。

#### 数位 DP (Digit DP)

##### 适用场景

* **时空复杂度**：O(位数 * 状态数)，通常是 log10(N) 级别。
* **适用问题**：统计在某个区间 `[L, R]` 内，满足特定“数位相关”性质的数的个数。

##### 核心代码

通常用记忆化搜索实现，将问题转化为 `count(R) - count(L-1)`。

```cpp
#include <vector>
#include <string>
#include <cstring>

long long dp[20][STATE_SIZE]; // pos, state
int digits[20];

// dfs 函数求解 [0, n] 中满足条件的数的个数
// pos: 当前处理到第几位 (从高到低)
// state: 自定义状态，记录之前数位的信息 (e.g., 数位和)
// is_limit: 当前位是否受 n 的限制 (e.g., n=567, 前两位是56, 则第3位最多是7)
// is_lead_zero: 是否是前导零
long long dfs(int pos, int state, bool is_limit, bool is_lead_zero) {
    // 终止条件
    if (pos == 0) {
        return is_lead_zero ? 0 : check_validity(state); // check_validity 根据题意判断
    }
    // 记忆化
    if (!is_limit && !is_lead_zero && dp[pos][state] != -1) {
        return dp[pos][state];
    }
  
    long long ans = 0;
    int upper_bound = is_limit ? digits[pos] : 9;

    for (int i = 0; i <= upper_bound; ++i) {
        // ... 根据 i 计算新的 state ...
        // int next_state = update_state(state, i);
        // ... 判断是否满足题目条件，如不能出现 "49" ...
        // if (prev_digit == 4 && i == 9) continue;
      
        ans += dfs(pos - 1, next_state, is_limit && (i == upper_bound), is_lead_zero && (i == 0));
    }

    // 存入记忆化数组
    if (!is_limit && !is_lead_zero) {
        dp[pos][state] = ans;
    }
    return ans;
}

long long solve(long long n) {
    if (n < 0) return 0;
    int len = 0;
    while (n) {
        digits[++len] = n % 10;
        n /= 10;
    }
    memset(dp, -1, sizeof(dp));
    return dfs(len, 0, true, true);
}

// long long final_ans = solve(R) - solve(L - 1);
```

##### 使用补充

* **模板性强**：数位DP的代码结构非常固定，关键在于定义好 `state` 和 `dfs` 函数中的各种判断。
* `is_limit` **是核心**：这个布尔值决定了当前位的上限。只有当上一位也取到了上限时，当前位才会继续受限。
* `is_lead_zero` **处理**：用于处理像“不能包含数字X”但数字0可以作为前导零的情况。当放置第一个非零数字后，`is_lead_zero` 变为 `false`。

#### 插头 DP (Plug DP)

##### 适用场景

* **时空复杂度**：非常高，如 O(N *M* 4^M)。
* **适用问题**：在网格图上求解路径、回路、连通性相关的问题，如寻找哈密顿回路数量、棋盘完美覆盖等。是状压DP的终极形态。

##### 核心代码

插头DP实现极为复杂，这里仅描述其思想，不给出完整可运行模板。

```cpp
/*
概念描述:
dp[i][j][mask]: 表示处理到格子 (i, j)，轮廓线状态为 mask 时的方案数。

- 轮廓线 (Contour Line):
   - 将已处理格子和未处理格子分开的一条线。
   - DP从上到下，从左到右进行，轮廓线也随之移动。
   - 轮廓线穿过 m+1 个格子边界，每个边界上可以有"插头"。

2. 插头 (Plug):
   - 表示轮廓线上某个位置的连通性。
   - 例如，0表示无插头，1表示有插头。
   - 对于需要区分连通分量的问题（如哈密顿回路），需要用更复杂的方式表示插头，如括号表示法(1表示左括号，2表示右括号)。

3. 状态压缩 (mask):
   - mask 是一个整数，用 k-进制 (k为插头状态数) 来编码轮廓线上 m+1 个位置的插头状态。
   - 例如，k=3 (0:无, 1:左, 2:右)，m=10，mask 就是一个3进制数。

4. 转移:
   - 从 dp[i][j-1][mask] 转移到 dp[i][j][new_mask]。
   - 转移的核心是根据 (i, j) 左边和上边的插头状态，以及 (i, j) 本身是否放置障碍，来决定 (i, j) 右边和下边的插头应该是什么。
   - 这个转移的讨论非常复杂，需要分情况（左、上插头状态的组合）进行。

5. 实现:
   - 通常使用滚动数组来优化空间。
   - 状态数量巨大，常用哈希表或 map 来存储非零的 DP 值。
*/
```

##### 使用补充

* **高难度**：插头DP是竞赛中最难的算法之一，需要对状压DP和图论连通性有深刻理解。
* **关键**：
    * **状态表示**：如何用一个整数 `mask` 有效地表示轮廓线的连通性状态。最小表示法是保证唯一性的关键。
    * **分类讨论**：对当前格子和其相邻插头的各种组合进行细致的分类讨论，写出正确的状态转移。

#### 计数 DP

##### 适用场景

* **时空复杂度**：依赖于具体问题。
* **适用问题**：求解满足特定条件的“方案数”，而不是最优值。

##### 核心代码

计数DP没有固定的代码，它是一种思想，应用在各种DP类型上。其核心是将“求最优值”的 `max/min` 操作改为“求和”的 `+` 操作。

```cpp
// 示例：路径计数
// dp[i][j] 表示从起点到格子 (i, j) 的路径数量
void count_paths(int m, int n) {
    int dp[m][n];
  
    // 初始化 dp 数组
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (is_obstacle(i, j)) {
                dp[i][j] = 0;
            } else if (i == 0 && j == 0) {
                dp[i][j] = 1;
            } else {
                int up = (i > 0) ? dp[i-1][j] : 0;
                int left = (j > 0) ? dp[i][j-1] : 0;
                dp[i][j] = (up + left) % MOD; // 注意取模
            }
        }
    }
    // return dp[m-1][n-1];
}
```

##### 使用补充

* **加法原理**：如果到达一个状态有多种独立的方式，那么总方案数是这些方式的方案数之和。
* **乘法原理**：如果一个任务可以分解为 K 个独立的步骤，每个步骤有 `n_k` 种方法，那么总方案数是 `n_1 * n_2 * ... * n_K`。
* **取模**：计数问题答案通常很大，需要对一个大质数（如 10^9 + 7）取模。

#### 动态 DP

##### 适用场景

* **时空复杂度**：单次修改和查询的复杂度通常为 O(log N) 或 O(log^2 N)，预处理 O(N)。
* **适用问题**：在经典的DP问题上增加了“单点或区间修改”操作。主要用于树上的动态规划问题。

##### 核心代码

以“树上最大权独立集带单点修改”为例。核心思想是将树链剖分和数据结构（如线段树）与DP结合。DP的转移方程需要能表示成矩阵乘法的形式。

```cpp
/*
概念描述:

- 重定义DP转移：
   将树形DP的转移写成矩阵形式。对于最大权独立集：
   dp[u][0] = sum(max(dp[v][0], dp[v][1]))  for v in light_children(u)
   dp[u][1] = weight[u] + sum(dp[v][0])     for v in light_children(u)

   我们可以定义一个广义矩阵乘法：C = A * B
   C[i][j] = max(A[i][k] + B[k][j]) for k in {0, 1}

   对于每个节点 u，可以构造一个 2x2 矩阵 G[u]，表示考虑了 u 的轻子树和 u 本身之后，从 u 向上转移的矩阵。
   [ f[u][0] ]   [ sum(max(dp[v][0],dp[v][1]))  sum(max(dp[v][0],dp[v][1])) ] [ g[son][0] ]
   [ f[u][1] ] = [      weight[u]+sum(dp[v][0])          -INF               ] * [ g[son][1] ]
   其中 f[u] 是 u 的 dp 值，g[son]是其重儿子的 dp 值。

2. 树链剖分：
   - 对树进行树链剖分，将树拆成若干条重链。
   - 每条重链上的节点在DFS序中是连续的。

3. 线段树维护矩阵乘积：
   - 用线段树维护每条重链上的矩阵连乘积。
   - 线段树的每个节点存储一个区间内的矩阵乘积。

4. 修改操作 (update(u, new_weight)):
   - 修改 weight[u]。这只会影响 G[u] 和 u 到根路径上所有重链顶端的父亲节点的 G 矩阵。
   - 在线段树上更新 G[u]，然后沿着 u 到根的路径，逐条重链更新答案。每跳一次重链，父节点的 G 矩阵中轻儿子的贡献会改变，需要更新。

5. 查询操作 (query()):
   - 答案就是根节点所在重链的线段树上区间 [1, size[chain_root]] 查询到的矩阵的结果。
*/
```

##### 使用补充

* **前置知识**：需要熟练掌握树形DP、矩阵乘法、树链剖分和线段树。
* **广义矩阵乘法**：DP转移方程必须能表达成 `dp[i] = op1(dp[j], dp[k], ...)` 的形式，并且这个 `op1` 可以和另一个 `op2` 结合，满足结合律。常见形式是 `(max, +)`。

#### 概率 DP

##### 适用场景

* **时空复杂度**：依赖于具体问题。
* **适用问题**：求解与概率或期望值相关的问题。

##### 核心代码

概率DP的状态定义和转移与普通DP类似，但转移时乘的是概率，求的是期望值。

```cpp
// 示例：一个游戏，有n个格子，从1号开始。每次等概率扔骰子（1-6点），
// 前进相应步数。求到达n号格子的期望投掷次数。
double dp[N]; // dp[i] 表示从 i 到 n 的期望次数

void probability_dp() {
    dp[n] = 0;
    for (int i = n - 1; i >= 1; --i) {
        dp[i] = 1; // 本次投掷算1次
        for (int j = 1; j <= 6; ++j) {
            int next_pos = std::min(i + j, n);
            dp[i] += (1.0 / 6.0) * dp[next_pos];
        }
    }
    // return dp[1];
}

// 如果状态转移有环，则需要解方程组 (高斯消元)。
// dp[i] = 1 + (1/k) * sum(dp[next_j])
// 将所有与 dp[i] 相关的项移到一边，然后解方程。
```

##### 使用补充

* **顺推 vs 逆推**：
    * 如果起点确定，终点不确定，通常用顺推（从前往后）。`dp[i]` 表示到达状态 `i` 的概率。
    * 如果终点确定，起点不确定或求从某点出发的期望，通常用逆推（从后往前）。`dp[i]` 表示从状态 `i` 到达终点的期望代价。逆推可以更好地处理吸收态（终止状态）和避免环。
* **期望的线性性**：`E(X+Y) = E(X) + E(Y)`。总期望等于各部分期望之和。这是期望DP的核心数学依据。
* **有环的期望DP**：如果状态转移有环，直接递推会无限循环。此时需要将DP方程看作线性方程组，使用高斯消元来求解。

#### DP 套 DP

##### 适用场景

* **时空复杂度**：非常高，通常是外层DP的状态数乘以内层DP的状态数。
* **适用问题**：当一个DP问题的状态需要通过另一个DP的结果来描述时。通常是“计算有多少种...使得...的最优解为/方案数为K”。

##### 核心代码

没有模板。这是一种高度抽象的思维方式。

```cpp
/*
概念描述:
问题：统计满足某种条件的输入 A 的数量，而这个条件本身需要通过一个 DP (我们称之为内层DP) 来判定。

例子：构造一个 01 字符串，使得其最长公共子序列 (LCS) 长度为 K。

- 外层DP:
   - 通常是数位DP或序列DP，用来构造输入 A。
   - `dp_outer(i, state_inner)`: 表示构造到输入 A 的第 i 位，此时内层DP的状态为 `state_inner` 时的方案数。

2. 内层DP的状态 (state_inner):
   - 这是最关键的一步。我们需要找到一种方法来表示内层DP的所有必要信息。
   - 对于 LCS 问题，如果我们正在匹配两个字符串 A 和 B (B是固定的)，`dp_inner[i][j]` 是 A的前i位和B的前j位的LCS。
   - `dp_inner[i][j]` 只依赖于 `dp_inner[i-1][j]`, `dp_inner[i-1][j-1]`, `dp_inner[i][j-1]`。
   - 当我们构造 A 的下一位时，`dp_inner[i]` 这一整行都可以从 `dp_inner[i-1]` 这一行推出来。
   - 所以，`state_inner` 就是内层 DP 的一整行 `dp_inner[i-1]` 的状态。
   - `dp_inner[i][j]` 和 `dp_inner[i][j-1]` 的差值最大为 1，所以可以用一个 bitmask 来压缩这一行的差分数组，这就是 `state_inner`。

3. 转移:
   `dp_outer(i, state_inner)` -> `dp_outer(i+1, new_state_inner)`
   - 我们尝试在 A 的第 i 位填 0 或 1。
   - 对于每种填法，我们模拟内层DP的转移，从 `state_inner` 计算出 `new_state_inner`。
   - 然后更新外层DP的值：`dp_outer(i+1, new_state_inner) += dp_outer(i, state_inner)`。
*/
```

##### 使用补充

* **目的**：将一个判别性问题（判断输入是否满足条件）的求解过程（内层DP）作为状态，来解决一个计数问题（外层DP）。
* **自动机**：内层DP的状态和转移可以看作一个自动机。外层DP是在这个自动机上行走的方案数。
* **高难度**：这是DP中最难的技巧之一，要求对内外两个DP都有深刻的理解，并能找到压缩内层DP状态的关键。

---

### DP 优化

#### DP 优化简介

##### 适用场景

* **目的**: 降低动态规划算法的时间或空间复杂度。
* **常见优化对象**: 状态转移过程中的循环。一个朴素的 `O(N^2)` DP，其转移方程 `dp[i] = optimize(dp[j])` (for `j` in range) 中的 `optimize` 过程（通常是 `max` 或 `min`），如果每次都暴力循环查找，总复杂度就是 `O(N^2)`。DP优化的目标就是将这个 `optimize` 过程加速到 `O(log N)` 或 `O(1)`。

##### 核心代码

无，是方法论。

##### 使用补充

优化技巧通常是针对特定形式的转移方程。识别出转移方程的结构，是选择正确优化方法的第一步。

#### 单调队列/单调栈优化

##### 适用场景

* **时空复杂度**: 将 `O(N^2)` 优化到 `O(N)`。
* **适用问题**: 转移方程形如 `dp[i] = max/min(dp[j]) + val[i]`，其中 `j` 的取值范围是一个固定大小的滑动窗口 `[i-k, i-1]`。

##### 核心代码

```cpp
#include <deque>

// dp[i] = max(dp[j]) for j in [i-k, i-1]
void monotonic_queue_opt(int n, int k) {
    int dp[n];
    std::deque<int> q; // 存储索引，其对应的 dp 值单调递减

    for (int i = 0; i < n; ++i) {
        // - 移除超出窗口范围的队首
        if (!q.empty() && q.front() < i - k) {
            q.pop_front();
        }

        // 2. 计算当前 dp 值，队首就是最优决策点
        dp[i] = (q.empty() ? 0 : dp[q.front()]) + value[i];
      
        // 3. 维护队列单调性，从队尾移除不如当前决策的元素
        while (!q.empty() && dp[q.back()] <= dp[i]) {
            q.pop_back();
        }
      
        // 4. 当前决策入队
        q.push_back(i);
    }
}
```

##### 使用补充

* **队列内容**：队列中存储的是**决策点**的**索引**，而不是值。
* **队首**：队首永远是当前窗口内的最优决策点。
* **单调栈**：思想类似，常用于解决“找到每个元素左/右边第一个比它大/小的元素”这类问题，也是 `O(N)`。

#### 斜率优化

##### 适用场景

* **时空复杂度**: `O(N)` 或 `O(N log N)`。
* **适用问题**: 转移方程 `dp[i] = min(dp[j] + cost(i, j))` 经过变形后，可以化为 `y = kx + b` 的形式，其中 `y` 和 `x` 是只与 `j` 相关的项，`k` 是只与 `i` 相关的项，`b` 是 `dp[i]`。

##### 核心代码

以 `dp[i] = min(dp[j] + (s[i]-s[j])^2)` 为例，其中 `s` 是前缀和。
展开得 `dp[i] = dp[j] + s[i]^2 - 2*s[i]*s[j] + s[j]^2`
移项得 `(dp[j] + s[j]^2) = (2*s[i])*s[j] + (dp[i] - s[i]^2)`
令 `y(j) = dp[j] + s[j]^2`，`x(j) = s[j]`，斜率 `k(i) = 2*s[i]`，截距 `b = dp[i] - s[i]^2`。
问题变成：对于每个 `i`，找到一个 `j`，使得经过点 `(x(j), y(j))` 且斜率为 `k(i)` 的直线的截距 `b` 最小。
这等价于用斜率为`k(i)`的直线去切一个下凸包。

```cpp
#include <deque>

long long Y(int j) { /* ... */ }
long long X(int j) { /* ... */ }
double slope(int j1, int j2) { return (double)(Y(j2) - Y(j1)) / (X(j2) - X(j1)); }

void slope_optimization(int n) {
    std::deque<int> q; // 存决策点索引，维护下凸包
    q.push_back(0);

    for (int i = 1; i <= n; ++i) {
        // - (如果斜率 k(i) 单调) 从队首弹出不再最优的决策
        while (q.size() >= 2 && slope(q[0], q[1]) < k(i)) {
            q.pop_front();
        }
      
        // 2. 队首即为当前最优决策点 j
        int j = q.front();
        dp[i] = /* calculate dp[i] using j */;

        // 3. (维护凸包) 从队尾弹出不满足凸性的点
        while (q.size() >= 2 && slope(q[q.size()-2], q.back()) > slope(q.back(), i)) {
            q.pop_back();
        }

        // 4. 当前点 i 入队，成为新的决策点
        q.push_back(i);
    }
}
```

##### 使用补充

* **单调性**：如果 `x(j)` 和 `k(i)` 都单调，可以用双端队列 `O(N)` 维护凸包。否则，需要在凸包上二分查找切点 `O(N log N)`，或者用平衡树/`std::set`维护。
* **推导**：关键步骤是将DP方程整理成 `b(i) = y(j) - k(i)x(j)` 的形式，然后判断是求最大截距（上凸包）还是最小截距（下凸包）。

#### 四边形不等式优化

##### 适用场景

* **时空复杂度**: 将 `O(N^3)` 优化到 `O(N^2)`。
* **适用问题**: 针对 `dp[i][j] = min(dp[i][k] + dp[k+1][j]) + w(i, j)` (区间DP) 或 `dp[i][j] = min(dp[i-1][k]) + w(k, j)` 这样的二维DP。

##### 核心代码

核心是利用决策单调性。令 `p[i][j]` 为 `dp[i][j]` 的最优决策点。
如果 `w(i, j)` 满足四边形不等式 `w(a,c)+w(b,d) <= w(a,d)+w(b,c)` for `a<=b<=c<=d`，则决策点 `p` 满足 `p[i][j-1] <= p[i][j] <= p[i+1][j]`。

```cpp
void quadrangle_inequality_opt(int n) {
    // for dp[i][j] = min(dp[i][k] + dp[k+1][j]) + w(i, j)
    int p[n+1][n+1];
  
    // 初始化
    for (int i = 1; i <= n; ++i) {
        dp[i][i] = 0;
        p[i][i] = i;
    }

    for (int len = 2; len <= n; ++len) {
        for (int i = 1; i <= n - len + 1; ++i) {
            int j = i + len - 1;
            int lower_k = p[i][j-1];
            int upper_k = p[i+1][j];
            dp[i][j] = 1e18;

            for (int k = lower_k; k <= upper_k; ++k) {
                long long val = dp[i][k] + dp[k+1][j] + w(i, j);
                if (val < dp[i][j]) {
                    dp[i][j] = val;
                    p[i][j] = k;
                }
            }
        }
    }
}
```

##### 使用补充

* **证明**：证明 `w` 满足四边形不等式是应用此优化的前提，有时需要数学直觉或打表观察。`w(i, j)` 单调也是一个常见条件。
* **1D/1D DP**：对于 `dp[i] = min(dp[j] + w(j, i))` 形式，决策单调性也可以用来分治求解，将 `O(N^2)` 优化到 `O(N log N)`。

#### Slope Trick

##### 适用场景

* **时空复杂度**: `O(N log N)` 或 `O(N)`。
* **适用问题**: 用于维护形如 `f(x) = g(x) + |x - a|` 或 `f(x) = min_{y<=x} g(y)` 的 DP 过程。其中，`g(x)` 是一个凸函数。此技巧直接维护 DP 函数 `f(x)` 本身（的导数），而不是 DP 值。

##### 核心代码

Slope Trick相当高级，其核心是使用优先队列来维护凸函数斜率发生变化的点。

```cpp
/*
概念描述:
- 我们不存储 dp[i][x] 的所有值，而是存储 dp[i] 这个凸函数的形状。
- 一个凸函数可以被其导数的“拐点”唯一确定。
- 我们用两个优先队列（一个大根堆 L，一个小根堆 R）来存储这些拐点。
- L 存储斜率 <= 0 的部分的所有拐点，R 存储斜率 >= 0 的部分的所有拐点。
- L 的堆顶是斜率从负数变为 0 的位置，R 的堆顶是斜率从 0 变为正数的位置。
- 整个函数的最小值在 [L.top(), R.top()] 区间内取到。

DP 转移：
- f(x) -> f(x) + c: 只需给全局最小值加上 c。
2. f(x) -> f(x) + a*x: 给所有拐点加上/减去某个值。
3. f(x) -> f(x) + |x - a|: 在 a 处插入两个拐点。具体操作是在 L 和 R 中 push(a)，然后平衡，保证 L.top() <= R.top()。
4. f(x) -> min_{y<=x} f(y): (min-convolution) 将函数在某点之后的部分“拉平”，可以通过操作优先队列 R 实现。

这是一个非常抽象的技巧，需要专门的学习。
*/
```

##### 使用补充

* 通常用于解决一些序列DP问题，其中DP状态包含一个变量`x`，且转移涉及对`x`的线性或绝对值函数。它的威力在于能将与值域相关的DP复杂度优化掉。

#### WQS 二分

##### 适用场景

* **时空复杂度**: `O(N log W)` 或 `O(N log N log W)`，W 为二分的值域。
* **适用问题**: 解决一类“**恰好**选取 `k` 个物品，求最大/小价值”的 DP 问题。要求不带 `k` 限制的原问题可以高效求解，并且最优解的价值 `g(k)` 关于 `k` 是一个凸函数。

##### 核心代码

```cpp
#include <algorithm>

// check 函数：计算带惩罚项的DP，返回最优价值和此时选择的物品数量
// cost_per_item 是二分的斜率/惩罚
std::pair<long long, int> check(double cost_per_item) {
    // dp[i] 表示前 i 个物品，考虑了惩罚项后的最大价值
    // 每次选择物品时，价值不仅是 v[i]，而是 v[i] - cost_per_item
    // 在DP过程中同时记录选择的物品数量
    // ... run a simpler DP (e.g., O(N) or O(N log N)) ...
    // return {max_value, item_count};
}

void wqs_binary_search(int k) {
    double l = -1e9, r = 1e9;
    for(int iter = 0; iter < 100; ++iter) { // 浮点数二分
        double mid = l + (r - l) / 2;
        if (check(mid).second >= k) {
            l = mid;
        } else {
            r = mid;
        }
    }
  
    // 找到了最优斜率 l
    auto result = check(l);
    // 最终答案是 result.first + k * l
    // 注意这里是 +k*l，因为 check 函数里减去了它
}
```

##### 使用补充

* **凸性**：`g(k)` 的凸性是关键。可以感性理解（每多选一个物品，带来的边际收益递减）或打表验证。
* **几何意义**：WQS二分是在 `(k, g(k))` 的凸包上二分斜率，寻找一条斜率为 `C` 的切线恰好切于 `x=k` 的点。`check(C)` 的返回值就是切点纵坐标，答案是 `check(C).value + k * C`。
* **整数二分**：如果物品价值和惩罚都是整数，可以用整数二分。

#### 状态设计优化

##### 适用场景

* 适用于任何DP问题，是一种思维层面的优化，而不是具体的算法。

##### 核心代码

无统一代码。

##### 使用补充

* **反转状态与值**：
    * **原状态**：`dp[i][j]` 表示用前 `i` 个物品，消耗容量 `j` 的最大价值。
    * **优化后**：`dp[i][v]` 表示用前 `i` 个物品，达到价值 `v` 所需的最小容量。
    * **适用**：当价值范围远小于容量范围时，这种转换可以大幅减小DP数组的大小。

* **减少冗余状态**：
    * 分析DP状态的维度，看是否有某些维度是多余的，或者可以被其他维度推导出来。
    * 例如，在某些序列DP中，`dp[i][j]` 表示 `[i, j]` 的解，但可能解只和区间长度 `j-i` 有关，状态可以优化为 `dp[len]`。

* **差分/前缀和思想**：
    * 当DP状态包含“区间总和”或“区间内元素个数”时，考虑是否可以用差分或前缀和的思想来简化状态或转移。

* **利用数据结构**：
    * 如果DP转移需要查询某个区间的历史信息（如最大值、和），可以使用线段树、树状数组等数据结构来维护DP数组，加速转移。这通常将转移的复杂度从 `O(N)` 降到 `O(log N)`。

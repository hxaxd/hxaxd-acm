# ACM 竞赛模板

## C++基础模板

```cpp
#include <bits/stdc++.h>
#pragma GCC optimize("O2")
#pragma GCC target("avx") // 开启avx指令集，提高效率
using namespace std;

// 常用宏定义
#define ll long long
#define ull unsigned long long
#define INF 0x3f3f3f3f
#define LLINF 0x3f3f3f3f3f3f3f3f
#define mod 1000000007 // 常用模数
#define endl '\n'

// 快速读入（处理1e6+数据）
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

// cin/cout优化
inline void init_io() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
}

int main() {
    init_io(); // 若用cin/cout则调用，用read/write可注释
  
    // ===== 输入区 =====
    int n = read(); // 示例：快速读入
  
    // ===== 逻辑处理区 =====
  
    // ===== 输出区 =====
    write(n); // 示例：快速输出
  
    return 0;
}
```

## C++新标准核心库函数

### 范围for循环 (C++11)

"简化容器遍历语法，避免手动操作迭代器，支持const引用避免拷贝"

```cpp
vector<int> vec = {1, 2, 3, 4, 5};
// 修改元素
for (auto &x : vec) x *= 2;
// 只读访问
for (const auto &x : vec) cout << x << " ";
// map遍历
map<int, string> mp = {{1, "a"}, {2, "b"}};
for (const auto &[key, value] : mp) 
    cout << key << ": " << value << endl;
```

### std::to_string (C++11)

"将数值类型转换为字符串，支持所有基本数值类型，比stringstream更简洁"

```cpp
int i = 42;
double d = 3.14159;
float f = 2.718f;
long long ll = 123456789012345LL;

string s1 = to_string(i);    // "42"
string s2 = to_string(d);    // "3.141590"
string s3 = to_string(f);    // "2.718000"
string s4 = to_string(ll);   // "123456789012345"
string result = "Value: " + to_string(i) + ", Pi: " + to_string(d);
```

### std::stoi/stoll/stod (C++11)

"字符串转数值类型，支持异常处理、进制转换和大小检查"

```cpp
string str1 = "123", str2 = "1010", str3 = "3.14", str4 = "FF";

int x = stoi(str1);                    // 123
int y = stoi(str2, nullptr, 2);        // 二进制转十进制: 10
long long z = stoll("123456789012");    // 123456789012
double d = stod(str3);                 // 3.14
int hex = stoi(str4, nullptr, 16);     // 十六进制转十进制: 255

// 带异常处理
try {
    int num = stoi("abc"); // 抛出std::invalid_argument
} catch (const std::exception& e) {
    cout << "Error: " << e.what() << endl;
}
```

### std::gcd (C++17)

"计算最大公约数，支持各种整数类型，处理负数自动取绝对值"

```cpp
#include <numeric>
int a = 12, b = 18;
int g1 = gcd(a, b);        // 6
int g2 = gcd(-12, 18);     // 6 (自动取绝对值)
long long g3 = gcd(24LL, 36LL); // 12

// 分数化简
int numerator = 15, denominator = 25;
int common = gcd(numerator, denominator);
numerator /= common;      // 3
denominator /= common;    // 5

// 多个数的gcd
int gcd3 = gcd(gcd(a, b), 24); // gcd(6, 24) = 6
```

### std::lcm (C++17)

"计算最小公倍数，防止中间结果溢出，自动处理负数"

```cpp
#include <numeric>
int a = 12, b = 18;
int l1 = lcm(a, b);        // 36
int l2 = lcm(-12, 18);     // 36
long long l3 = lcm(24LL, 36LL); // 72

// 多个数的lcm
int lcm3 = lcm(lcm(a, b), 24); // lcm(36, 24) = 72

// 防止溢出的特性
int large1 = 1000000000, large2 = 1000000000;
// 自动处理中间计算防止溢出
int result = lcm(large1, large2);
```

### std::bit_count/popcount (C++20)

"统计二进制中1的个数，比手动位操作更高效，支持各种整数类型"

```cpp
#include <bit>
unsigned int n1 = 5;        // 二进制: 101
unsigned int n2 = 7;        // 二进制: 111
unsigned long long n3 = 15; // 二进制: 1111

int cnt1 = bit_count(n1);   // 2
int cnt2 = bit_count(n2);   // 3
int cnt3 = bit_count(n3);   // 4

// 应用场景
bool isPowerOfTwo = bit_count(n) == 1;
int hammingDistance = bit_count(a ^ b);
int parity = bit_count(n) % 2; // 奇偶校验
```

### std::views::iota (C++20)

"生成连续整数序列视图，惰性求值节省内存，支持无限序列"

```cpp
#include <ranges>
#include <algorithm>

// 生成有限序列
for (int i : views::iota(1, 5)) 
    cout << i << " ";  // 1 2 3 4

// 生成无限序列（需要限制）
auto infinite = views::iota(0) | views::take(10);
for (int i : infinite) cout << i << " "; // 0-9

// 配合算法使用
vector<int> result;
ranges::copy(views::iota(1, 6), back_inserter(result));

// 生成步长序列（C++23）
// for (int i : views::iota(0, 10, 2)) 
//     cout << i << " ";  // 0 2 4 6 8
```

## STL高频库函数

### `std::stack` (LIFO - 后进先出)

| 类别         | 函数/操作             | 描述                                             |
| ------------ | --------------------- | ------------------------------------------------ |
| **构造函数** | `stack<T> s;`         | 创建一个空栈                                     |
| **元素访问** | `s.top();`            | **返回栈顶元素的引用**（常用）                   |
| **容量**     | `s.empty();`          | 判断栈是否为空                                   |
|              | `s.size();`           | 返回栈中元素个数                                 |
| **修改**     | `s.push(val);`        | 将元素 `val` 压入栈顶（常用）                    |
|              | `s.emplace(args...);` | 在栈顶原地构造一个元素（C++11）                  |
|              | `s.pop();`            | **弹出栈顶元素**（不返回被弹出元素的值）（常用） |

### `std::queue` (FIFO - 先进先出)

| 类别         | 函数/操作             | 描述                                 |
| ------------ | --------------------- | ------------------------------------ |
| **构造函数** | `queue<T> q;`         | 创建一个空队列                       |
| **元素访问** | `q.front();`          | **返回队首元素的引用**（常用）       |
|              | `q.back();`           | 返回队尾元素的引用                   |
| **容量**     | `q.empty();`          | 判断队列是否为空                     |
|              | `q.size();`           | 返回队列中元素个数                   |
| **修改**     | `q.push(val);`        | 将元素 `val` 推到队尾（常用）        |
|              | `q.emplace(args...);` | 在队尾原地构造一个元素（C++11）      |
|              | `q.pop();`            | **弹出队首元素**（不返回值）（常用） |

### `std::priority_queue` (优先队列，默认最大值优先)

| 类别         | 函数/操作                                      | 描述                                             |
| ------------ | ---------------------------------------------- | ------------------------------------------------ |
| **构造函数** | `priority_queue<T> pq;`                        | 创建一个大顶堆                                   |
|              | `priority_queue<T, vector<T>, greater<T>> pq;` | 创建一个小顶堆                                   |
| **元素访问** | `pq.top();`                                    | **返回优先级最高（默认最大）元素的引用**（常用） |
| **容量**     | `pq.empty();`                                  | 判断是否为空                                     |
|              | `pq.size();`                                   | 返回元素个数                                     |
| **修改**     | `pq.push(val);`                                | 插入元素，并调整堆结构（常用）                   |
|              | `pq.emplace(args...);`                         | 原地构造并插入（C++11）                          |
|              | `pq.pop();`                                    | **移除优先级最高的元素**（不返回值）（常用）     |

### 序列容器

**（`vector`, `deque`, `list`, `forward_list`, `array`, `string`）**

#### 通用常用接口

| 类别         | 函数/操作                     | 描述                                                  |
| ------------ | ----------------------------- | ----------------------------------------------------- |
| **构造函数** | `C c;`                        | 默认构造                                              |
|              | `C c(n, val);`                | 构造包含 n 个 val 的容器                              |
|              | `C c(begin_it, end_it);`      | 用迭代器范围构造                                      |
|              | `C c{1, 2, 3};`               | 列表初始化 (C++11)                                    |
| **赋值**     | `c = {1, 2, 3};`              | 列表赋值                                              |
|              | `c.assign(n, val);`           | 赋值 n 个 val                                         |
|              | `c.assign(begin_it, end_it);` | 用迭代器范围赋值                                      |
| **元素访问** | `c[i]`                        | **下标访问（不检查越界）**                            |
|              | `c.at(i)`                     | **带边界检查的下标访问，越界抛出异常**                |
|              | `c.front()`                   | 返回第一个元素的引用                                  |
|              | `c.back()`                    | 返回最后一个元素的引用（`forward_list` 没有）         |
|              | `c.data()`                    | 返回指向底层数组的指针（`array`, `vector`, `string`） |
| **迭代器**   | `c.begin()`, `c.end()`        | 返回指向首元素和尾后位置的迭代器                      |
|              | `c.rbegin()`, `c.rend()`      | 返回反向迭代器（`forward_list` 没有）                 |
| **容量**     | `c.empty()`                   | 判断是否为空                                          |
|              | `c.size()`                    | **返回元素个数**                                      |
|              | `c.capacity()`                | 返回已分配存储空间大小（`vector`, `string`）          |
|              | `c.reserve(n)`                | 预留空间（`vector`, `string`）                        |
|              | `c.shrink_to_fit()`           | 请求移除未使用的容量（`vector`, `deque`, `string`）   |
| **修改器**   | `c.clear()`                   | **清空容器**                                          |
|              | `c.insert(pos_it, val)`       | 在迭代器 pos 前插入元素                               |
|              | `c.emplace(pos_it, args...)`  | 在迭代器 pos 前原地构造元素 (C++11)                   |
|              | `c.erase(pos_it)`             | 删除迭代器位置的元素                                  |
|              | `c.erase(begin_it, end_it)`   | 删除迭代器范围内的元素                                |
|              | `c.push_back(val)`            | **在尾部添加元素**（`forward_list` 没有）             |
|              | `c.emplace_back(args...)`     | 在尾部原地构造元素 (C++11)（`forward_list` 没有）     |
|              | `c.pop_back()`                | **删除尾部元素**（`forward_list` 没有）               |
|              | `c.resize(n)`                 | 改变容器大小                                          |

#### 各容器特色接口

| 容器                                  | 函数/操作                                      | 描述                                          |
| ------------------------------------- | ---------------------------------------------- | --------------------------------------------- |
| **`deque` & `list` & `forward_list`** | `push_front(val)` / `emplace_front(args...)`   | 在头部添加元素                                |
|                                       | `pop_front()`                                  | 删除头部元素                                  |
| **`list` & `forward_list`**           | `c.remove(val)`                                | 删除所有值等于 val 的元素                     |
|                                       | `c.remove_if(pred)`                            | 删除所有谓词 pred 为 true 的元素              |
|                                       | `c.sort()`                                     | 对链表本身进行排序                            |
|                                       | `c.merge(other_list)`                          | 合并两个**已排序**链表                        |
|                                       | `c.splice(pos_it, other_list)`                 | 将另一个链表的所有元素移动到本链表的 pos 之前 |
| **`forward_list`**                    | `c.before_begin()`                             | 返回第一个元素之前的迭代器                    |
|                                       | `insert_after`, `emplace_after`, `erase_after` | 在指定位置后操作                              |

### 关联容器与无序关联容器

**（`set`/`map`/`multiset`/`multimap` 和 `unordered_` 版本）**

#### 通用常用接口

| 类别         | 函数/操作                               | 描述                                                |
| ------------ | --------------------------------------- | --------------------------------------------------- |
| **构造函数** | 与序列容器类似                          |                                                     |
| **容量**     | `empty()`, `size()`                     |                                                     |
| **修改器**   | `clear()`                               | 清空容器                                            |
|              | `insert(val)` 或 `insert({key, value})` | **插入元素**                                        |
|              | `emplace(args...)`                      | 原地构造并插入                                      |
|              | `erase(key)`                            | **按键删除元素**，返回删除的数量                    |
|              | `erase(iterator)`                       | 按迭代器位置删除                                    |
| **查找**     | `c.find(key)`                           | **查找指定键，返回迭代器**（极其常用）              |
|              | `c.count(key)`                          | 返回具有特定键的元素数量                            |
|              | `c.lower_bound(key)`                    | 返回第一个**不小于** key 的元素的迭代器（有序容器） |
|              | `c.upper_bound(key)`                    | 返回第一个**大于** key 的元素的迭代器（有序容器）   |
|              | `c.equal_range(key)`                    | 返回一个迭代器 pair，表示匹配键的范围               |

#### 各容器特色/关键接口

| 容器类型       | 函数/操作               | 描述                                                                           |
| -------------- | ----------------------- | ------------------------------------------------------------------------------ |
| **`map` 系列** | `map_obj[key]`          | **如果 key 存在，返回其引用；如果不存在，则插入 `key` 并用值初始化**（最常用） |
|                | `map_obj.at(key)`       | 如果 key 存在，返回其引用；如果不存在，抛出异常                                |
|                | `map::lower_bound(key)` | 返回第一个**不小于** key 的元素的迭代器（有序容器）                            |
| **有序容器**   | `c.begin()`             | 返回按键排序后的第一个元素                                                     |
| **无序容器**   | `c.bucket_count()`      | 返回桶的数量                                                                   |
|                | `c.load_factor()`       | 返回负载因子                                                                   |
|                | `c.rehash(n)`           | 设置桶数为至少 n                                                               |
|                | `c.reserve(n)`          | 设置桶数，使得可以容纳至少 n 个元素                                            |

好的，这是对 STL 高频库函数的补充，添加了 `std::bitset` 和 `std::pair`。

### **`std::pair` ( utility 头文件)**

用于将两个值组合成一个单元，常用于需要返回两个值的函数或作为 `map` 的元素。

| 类别           | 函数/操作                      | 描述                                                                 |
| -------------- | ------------------------------ | -------------------------------------------------------------------- |
| **构造函数**   | `pair<T1, T2> p;`              | 默认构造，对 `first` 和 `second` 进行值初始化                        |
|                | `pair<T1, T2> p(val1, val2);`  | 用 `val1` 和 `val2` 构造 `first` 和 `second`                         |
|                | `p = make_pair(val1, val2);`   | 辅助函数，创建并返回一个 `pair` (C++11 前常用，现在多用 `{}`)         |
|                | `p = {val1, val2};`            | **使用列表初始化** (C++11，更简洁)                                   |
| **元素访问**   | `p.first`                      | **访问第一个公有数据成员**                                           |
|                | `p.second`                     | **访问第二个公有数据成员**                                           |
| **比较操作**   | `p1 == p2`, `p1 != p2`         | 按字典序比较（先比较 `first`，如果相等再比较 `second`）              |
|                | `p1 < p2`, `p1 <= p2`, etc.    |                                                                      |
| **常用函数**   | `tie(a, b) = p;`               | `#include <tuple>`，将 `pair` 的值解包到变量 `a`, `b` 中             |

### **`std::bitset` ( bitset 头文件)**

表示固定大小的二进制位序列，提供高效的位操作。**模板参数 `N` 是size（位数），不是类型。**

| 类别           | 函数/操作                                      | 描述                                                                                 |
| -------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------ |
| **构造函数**   | `bitset<N> b;`                                 | 默认构造，所有位初始化为 `0`                                                         |
|                | `bitset<N> b(string("1010"));`                 | 从 `std::string` 初始化（从字符串的**最左字符**表示**高位/MSB**）                    |
|                | `bitset<N> b("1010");`                         | 从 C-style 字符串初始化                                                              |
| **位访问**     | `b[pos]`                                       | **访问 `pos` 位置的位（从0开始，右为低位/LSB），返回 `bitset::reference` 代理对象**     |
|                | `b.test(pos)`                                  | 访问 `pos` 位置的位，**进行边界检查**，如果 `pos` 越界则抛出异常                     |
|                | `b.count()`                                    | **返回被设置为 `1` 的位的个数**                                                      |
|                | `b.all()`                                      | (C++11) 如果所有位都是 `1` 则返回 `true`                                             |
|                | `b.any()`                                      | 如果有任何位是 `1` 则返回 `true`                                                     |
|                | `b.none()`                                     | 如果没有位是 `1`（即所有位为 `0`）则返回 `true`                                      |
| **容量**       | `b.size()`                                     | 返回位的大小 `N`                                                                     |
| **位操作**     | `b.set()`                                      | **将所有位设置为 `1`**                                                               |
|                | `b.set(pos, val=true)`                         | **将 `pos` 位置的位设置为 `val`**                                                    |
|                | `b.reset()`                                    | **将所有位设置为 `0`**                                                               |
|                | `b.reset(pos)`                                 | 将 `pos` 位置的位设置为 `0`                                                          |
|                | `b.flip()`                                     | **翻转所有位（0变1，1变0）**                                                         |
|                | `b.flip(pos)`                                  | 翻转 `pos` 位置的位                                                                  |
| **转换**       | `b.to_string()`                                | **将 bitset 转换为 `std::string`**                                                   |
|                | `b.to_ulong()` / `b.to_ullong()`               | **将 bitset 转换为 `unsigned long` 或 `unsigned long long`**（超出范围会抛出异常）   |
| **位运算符重载** | `&`, `\|`, `^`, `~`, `<<`, `>>`                | 支持与另一个 `bitset` 进行按位运算和位移操作                                         |
| **IO操作**     | `cin >> b`, `cout << b`                        | 支持输入输出（输入时，从流中读取 `'0'` 和 `'1'` 字符直到文件尾、遇到非01字符或读满N位） |

## 高频算法库

### 自定义比较

```cpp
// 方式1: greater<int>
priority_queue<int, vector<int>, greater<int>> min_heap1;
min_heap1.push(5); min_heap1.push(2); min_heap1.push(8);
cout << min_heap1.top(); // 2

// 方式2: 自定义比较函数
auto cmp = [](int left, int right) { return left > right; };
priority_queue<int, vector<int>, decltype(cmp)> min_heap2(cmp);

```

### sort()

"快速排序算法，支持自定义比较器，稳定排序可用stable_sort"

```cpp
vector<int> vec = {5, 2, 8, 1, 9, 3, 7, 4, 6};

// 默认升序
sort(vec.begin(), vec.end()); // 1,2,3,4,5,6,7,8,9

// 降序
sort(vec.begin(), vec.end(), greater<int>()); // 9,8,7,6,5,4,3,2,1

// 自定义排序规则
sort(vec.begin(), vec.end(), [](int a, int b) {
    return a % 10 < b % 10; // 按个位数排序
});

// 稳定排序（保持相等元素的相对顺序）
stable_sort(vec.begin(), vec.end());

// 部分排序
partial_sort(vec.begin(), vec.begin() + 5, vec.end()); // 前5个最小元素有序
```

### lower_bound() / upper_bound()

"二分查找算法，要求序列有序，用于快速查找和范围统计"

```cpp
vector<int> vec = {1, 2, 3, 3, 3, 4, 5, 6, 7};

// 查找第一个≥3的位置
auto lb = lower_bound(vec.begin(), vec.end(), 3); // 指向第一个3
// 查找第一个>3的位置
auto ub = upper_bound(vec.begin(), vec.end(), 3); // 指向第一个4

// 统计3的个数
int count = distance(lb, ub); // 3
// 或者
int count = ub - lb; // 3

// 检查元素是否存在
bool exists = binary_search(vec.begin(), vec.end(), 3);

// 插入位置查找
auto insert_pos = lower_bound(vec.begin(), vec.end(), 3);
vec.insert(insert_pos, 3); // 在正确位置插入
```

### 其他重要算法

"常用算法函数集合，覆盖各种数据处理需求"

```cpp
vector<int> vec = {1, 2, 3, 4, 5};

// 反转容器
reverse(vec.begin(), vec.end()); // {5,4,3,2,1}

// 求最大/最小值
int max_val = *max_element(vec.begin(), vec.end()); // 5
int min_val = *min_element(vec.begin(), vec.end()); // 1

// 累积求和
int sum = accumulate(vec.begin(), vec.end(), 0); // 15
double product = accumulate(vec.begin(), vec.end(), 1.0, multiplies<double>());

// 填充容器
fill(vec.begin(), vec.end(), 0); // 全部填充为0
fill_n(vec.begin(), 3, 1); // 前3个填充为1

// 复制容器
vector<int> dest(vec.size());
copy(vec.begin(), vec.end(), dest.begin());
copy_if(vec.begin(), vec.end(), dest.begin(), [](int x) { return x % 2 == 0; });

// 查找算法
auto it = find(vec.begin(), vec.end(), 3); // 查找值为3的元素
auto it2 = find_if(vec.begin(), vec.end(), [](int x) { return x > 3; });

// 计数算法
int count_even = count_if(vec.begin(), vec.end(), [](int x) { return x % 2 == 0; });
```

## 错误检查

### 因环境不同导致的错误

- `scanf` 或 `printf` 使用 `%I64d` 格式指示符在 Linux 下可能导致输出格式错误。

### 会引起 CE 的错误

这类错误多为词法、语法和语义错误，引发的原因较为简单，修复难度较低。

例：

- `int main()` 写为 `int mian()` 之类的拼写错误。

- 写完 `struct` 或 `class` 忘记写分号。

- 数组开太大，（在 OJ 上）使用了不合法的函数（例如多线程），或者函数声明但未定义，会引起链接错误。

- 函数参数类型不匹配。


- 使用 `goto` 和 `switch-case` 的时候跳过了一些局部变量的初始化。

#引起 CE 但会引起 Warning 的错误

犯这类错误时写下的程序虽然能通过编译，但大概率会得到错误的程序运行结果。这类错误会在使用 `-W{warningtype}` 参数编译时被编译器指出。

- 赋值运算符 `=` 和比较运算符 `==` 不分。

- 由于运算符优先级产生的错误。

- 不正确地使用 `static` 修饰符。

- 使用 `scanf` 读入的时候没加取地址符 `&`。

- 使用 `scanf` 或 `printf` 的时候参数类型与格式指定符不符。

- 同时使用位运算和逻辑运算符 `==` 并且未加括号。
    - 示例：`(x >> j) & 3 == 2`

- `int` 字面量溢出。

- 未初始化局部变量。

- 局部变量与全局变量重名，导致全局变量被意外覆盖。（开 `-Wshadow` 就可检查此类错误。）

- 运算符重载后引发的输出错误。
    - 示例：

        ```cpp
        // 本意：前一个 << 为重载后的运算符，表示输出；后一个 << 为移位运算符，表示将 1
        // 左移 1 位。 但由于忘记加括号，导致编译器将后一个 <<
        // 也判作输出运算符，而导致输出的结果与预期不同。 错误 std::cout << 1 << 1; 正确
        std::cout << (1 << 1);
        ```

### 既不会引起 CE 也不会引发 Warning 的错误

这类错误无法被编译器发现，仅能自行查明。

#### 会导致 WA 的错误

- 上一组数据处理完毕，读入下一组数据前，未清空数组。

- 读入优化未判断负数。

- 所用数据类型位宽不足，导致溢出。
    - 如习语「三年 OI 一场空，不开 `long long` 见祖宗」所描述的场景。选手因为没有在正确的地方开 `long long`（将整数定义为 `long long` 类型），导致得出错误的答案而失分。

- 存图时，节点编号 0 开始，而题目给的边中两个端点的编号从 1 开始，读入的时候忘记 -1。

- 大/小于号打错或打反。

- 在执行 `ios::sync_with_stdio(false);` 后混用 `scanf/printf` 和 `std::cin/std::cout` 两种 IO，导致输入/输出错乱。

- 由于宏的展开，且未加括号导致的错误。

- 哈希的时候没有使用 `unsigned` 导致的运算错误。
    - 对负数的右移运算会在最高位补 1。参见：[位运算](../math/bit.md)

- 没有删除或注释掉调试输出语句。

- 误加了 `;`。

- 哨兵值设置错误。例如，平衡树的 `0` 节点。

- 在类或结构体的构造函数中使用 `:` 初始化变量时，变量声明顺序不符合初始化时候的依赖关系。

- 并查集合并集合时没有把两个元素的祖先合并。

- `freopen` 使用 `a` 进行追加写
    - CCF 的检测环境不会清空输出文件，使用 `a` 会导致上一位选手的输出也被评测机读入引发 WA

##### 换行符不同

不同的操作系统使用不同的符号来标记换行，以下为几种常用系统的换行符：

- LF（用 `\n` 表示）：`Unix` 或 `Unix` 兼容系统

- CR+LF（用 `\r\n` 表示）：`Windows`

- CR（用 `\r` 表示）：`Mac OS` 至版本 9

而 C/C++ 利用转义序列 `\n` 来换行，这可能会导致我们认为输入中的换行符也一定是由 `\n` 来表示，而只读入了一个字符来代表换行符，这就会导致我们没有完全读入输入文件。

以下为解决方案：

- 多次 `getchar()`，直到读到想要的字符为止。

- 使用 `cin` 读入，**这可能会增大代码常数**。

- 使用 `scanf("%s",str)` 读入一个字符串，然后取 `str[0]` 作为读入的字符。

- 使用 `scanf(" %c",&c)` 过滤掉所有空白字符。

#### 会导致未知的结果

未定义行为会导致未知的结果，可能是 WA，RE 等。编译器通常会假定你的程序不会出现未定义行为，因此出现开 O2 与不开 O2 代码行为不一致的情况。

- 除以 0（求 0 的逆元）

- 数组（下标）越界

    例如：

    - 未正确设置循环的初值导致访问了下标为 -1 的值。

    - 无向图边表未开 2 倍。

    - 线段树未开 4 倍空间。

    - 看错数据范围，少打一个零。

    - 错误预估了算法的空间复杂度。

    - 写线段树的时候，`pushup` 或 `pushdown` 叶节点。

- 除 main 外有返回值函数执行至结尾未执行任何 return 语句

- 尝试修改字符串字面量

- 多次释放/非法解引用一片内存

- 尝试释放由 `new []` 分配的整块内存的一部分


- 解引用空指针/野指针


- 有符号数溢出


- 使用未初始化的变量


#### 会导致 RE

- 没删文件操作（某些 OJ）。

- 排序时比较函数的错误 `std::sort` 要求比较函数是严格弱序：`a<a` 为 `false`；若 `a<b` 为 `true`，则 `b<a` 为 `false`；若 `a<b` 为 `true` 且 `b<c` 为 `true`，则 `a<c` 为 `true`。其中要特别注意第二点。

#### 会导致 TLE

- 分治未判边界导致死递归。

- 死循环。

    - 循环变量重名。

    - 循环方向反了。

- BFS 时不标记某个状态是否已访问过。

- 使用宏展开编写 min/max

- 使用 + 运算符向 `std::string` 类字符串追加字符

- 没删文件操作（某些 OJ）。

- 在 `for/while` 循环中重复执行复杂度非 $O(1)$ 的函数。严格来说，这可能会引起时间复杂度的改变。

#### 会导致 MLE

- 数组过大。
- STL 容器中插入了过多的元素。

    - 经常是在一个会向 STL 插入元素的循环中死循环了。

    - 也有可能被卡了。

#### 会导致常数过大

- 定义模数的时候，未定义为常量。
    - 示例：

        ```cpp
        // int mod = 998244353;      // 错误
        const int mod = 998244353;  // 正确，方便编译器按常量处理
        ```

- 使用了不必要的递归（尾递归不在此列）。
- 将递归转化成迭代的时候，引入了大量额外运算。

#### 只在程序在本地运行的时候造成影响的错误

- 文件操作有可能会发生的错误：
    - 对拍时未关闭文件指针 `fclose(fp)` 就又令 `fp = fopen()`。这会使得进程出现大量的文件野指针。
    - `freopen()` 中的文件名未加 `.in`/`.out`。

- 使用堆空间后忘记 `delete` 或 `free`。

## 技巧

### 循环宏定义

如下代码可使用宏定义简化：

```cpp
for (int i = 0; i < N; i++) {
  // 循环内容略
}

// 使用宏简化
#define f(x, y, z) for (int x = (y), __ = (z); x < __; ++x)

// 这样写循环代码时，就可以简化成 `f(i, 0, N)` 。例如：
// a is a STL container
f(i, 0, a.size()) { ... }
```

另外推荐一个比较有用的宏定义：

```cpp
#define _rep(i, a, b) for (int i = (a); i <= (b); ++i)
```

### 内存池

当动态分配内存时，频繁使用 `new`/`malloc` 会占用大量的时间和空间，甚至生成大量的内存碎片从而降低程序的性能，可能会使原本正确的程序 TLE/MLE。

这时候需要使用到「内存池」这种技巧：在真正使用内存之前，先申请分配一定大小的内存作为备用。当需要动态分配时直接从备用内存中分配一块即可。

在大多数 OI 题当中，可以预先算出需要使用到的最大内存并一次性申请分配。

示例：

```cpp
// 申请动态分配 32 位有符号整数数组：
int* newarr(int sz) {
  static int pool[MAXN], *allocp = pool;
  return allocp += sz, allocp - sz;
}

// 线段树动态开点的代码：
Node* newnode() {
  static Node pool[MAXN << 1], *allocp = pool - 1;
  return ++allocp;
}
```

## 算法

###
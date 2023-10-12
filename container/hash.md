### set

1.  容器，内部排序，元素唯一。由于这个性质，set可以视为unique后的优先队列
2.  内部实现时，里面的元素是常量，所以不能修改，但可以添加或者删除
3.  排序方法：strict weak ordering criterion 默认是非降序
4.  通过key去获取元素比unordered\_set慢，but they allow the direct iteration on subsets based on their order.
5.  内部实现 binary search trees.红黑树

```cpp
// 初始化
set<int> s(arr.begin(), arr.end());  // 用一个vector来做初始化

// 插入
set.insert(val);
set.insert(v.begin(), v.end());  // Nlog(size+N)。如果已经排好序了，更快
.emplace(val);

// 返回值：
pair<set<int>::iterator, bool>
set.insert(val).first    // 如上，指向新插入的位置的迭代器
if (set.insert(val).second) {}    // 插入是否成功，可以用来判断是否重复

// 查找
if (set.count(val) != 0) // 返回0/1
if (set.find(val) != set.end())  // 找不到 返回迭代器
// set特有，因为哈希版本没有排序。
set.lower_bound(val);
set.upper_bound(val); 

// 删除
// 记得删除前要判断是否存在
set.erase(it); // 常数级
set.erase(val); // 对数级
set.erase(set.find(3), set.end()); // 线性级

// 反的迭代器 std::set<int>::reverse_iterator
for (auto rit = myset.rbegin(); rit != myset.rend(); ++rit) {
    std::cout << ' ' << *rit;
}
// 排好序的
for (const int& num : nums) {
    cout << num << " ";
}

```

### unordered_set
1.  内部实现时，里面的元素是常量，所以不能修改，但可以添加或者删除
2.  只有正向迭代（这个正向只是根据bucket的hash值来说）

基本只有迭代不同
```cpp
// 桶遍历
for (auto i = 0; i != myset.bucket_count(); ++i) {
    std::cout << "bucket #" << i << " contains:";
    for (auto local_it = myset.begin(i); local_it!= myset.end(i); ++local_it )
      std::cout << " " << *local_it;
}
// 不是排好序的
for (const int& num : nums) {
    cout << num << " ";
}
```

### multiset
```cpp
std::multiset<int> ms{1,2,2,2,3,4,5};

ms.count(x); // 出现次数
ms.lower_bound(x)

// 1 if中可以定义，然后使用;，生命域限定{}
// 2 std::prev std::next，默认是1
if (auto it = prev(ms.end()); *it >= tasks[i]) ws.erase(it);
```

### map
* 初始化
```cpp
map<int, string> m;  
unordered_map<int, string> m;
unordered_map<int, int> m(5); // 桶数是5
```

* 插入

两种插入方式：

1. operator[] 

```cpp
m[1] = Node(1.50);
// 原理
auto it = m.insert({1, Node()}); // 先构造一对象<k,v>
*(it->second) = Node(1.50); // 再对v进行赋值，因此如果没有v的默认构造函数，编译会报错。

m[i]++; // 如果key不存在，operator[]会进行树的插入，所以operator[]必须非const的成员函数。（不能被插入）
// 因此如果是个const unordered_map，无法用[]去访问，只能用find去做。
```

2. insert()

   插入的就是pair，有多种方式。

   返回的也是return pair<iterator, bool>，插入成功就是返回的插入位置的迭代器。

```cpp
m.insert(pair<int, string>(1, "student_one"));
m.insert(make_pair(2, "student_two"));
```

当添加新键值对时，insert() 执行效率更高；
而更新键值对时，operator[] 的效率更高。

* 迭代


```cpp
// 遍历 it->first it->second 表示 k和v
for(map<int, string>::iterator iter = m.begin(); iter != m.end(); iter++) {
    cout << iter->first << ' ' << iter->second << endl;
}
for (auto it : m) { // 这样是值，不是迭代器
    cout << it.first << ' ' << it.second << endl;
}
for (auto [x, y]: m) { // 可以用_省略
    cout << x << ' ' << y << endl;
} 
```

* 查找


```cpp
// 查找
// count和find同set
// find return <key,value> a pair

// 修改
// 在一定能找到的情况下
Node cur = m[1]; // 不推荐这样写，因为operator[]需要有默认构造函数，而且效率较低

auto cur = mapStudent.find(key); // return <key,value>
cur->second = newVal; // 哈希表更新节点，没有写默认构造，就只能这样更新


// 删除
// 同set
```



### 混合
处理空的情况
```c++
unordered_map<char, unordered_set<char>> adjm;
if (!adjm.count(ch)) {
    adjm[ch] = unordered_set<char>{};
}
```

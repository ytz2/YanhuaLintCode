# Segment Tree

## 线段树的表达

```
            [0,3]
           (val=4)
         /         \
     [0,1]         [2,3]
    (val=4)       (val=3)
    /    \         /    \
 [0,0]  [1,1]   [2,2]  [3,3]
(val=1)(val=4) (val=2)(val=3)
```

\[1, 4, 2, 3\]

第一层： \[0, 3\]区间， 最大4

第二层： \[0, 1\]区间， 最大4

```
            \[2, 3\]区间， 最大3
```

第三层： \[0, 0\]区间， 最大1

```
            [1, 1\]区间， 最大4

           \[2,  2\]区间， 最大2

           \[3,  3\]区间， 最大3
```

如上：

线段树是把一个array表达成以\[ind0, ind1\]为线段的树结构

所以定义SegmentTreeNode如下

```cpp
struct SegmentTreeNode{

    SegmentTreeNode(int s, int e, int v)
        : start(s), end(e), val(v), left(nullptr), right(nullptr)
    {
    }
    ~SegmentTreeNode()
    {
        if (left)
            delete left;
        if (right)
            delete right;
    }
    int start;
    int end;
    int val;
    SegmentTreeNode* left;
    SegmentTreeNode* right;

};
```

## 线段树的构建

根据线段树的定义， 树的构建是beg, end, arr，左右切一下，一直切到一个一个的元素为止

```cpp
    void build(const std::vector<int>& arr)
    {
        root = buildHelper(0, arr.size()-1, arr);
    }

    SegmentTreeNode* buildHelper(int beg, int end, const std::vector<int>& arr)
    {
        if (beg > end)
            return nullptr;
        SegmentTreeNode* node = new SegmentTreeNode(beg, end, arr[beg]);
        if (beg == end)
            return node;
        int mid = ( beg + end ) / 2;
        node->left = buildHelper(beg, mid, arr);
        node->right = buildHelper(mid+1, end, arr);
        node->val = std::max(node->left?node->left->val:INT_MIN, node->right? node->right->val : INT_MIN);
        return node;
    }
```

## 线段树的查询

![](/assets/segtree.png)

第一层会查询试图查询\[1, 7\], 发现区间不存在，然后根据mid位置拆分\[1, 4\]和\[5, 7\]  
第二层会查询\[1, 4\],\[5, 7\], 发现\[1, 4\]已经存在，返回即可，\[5, 7\]仍旧需要继续拆分  
第三层会查询\[5, 6\],\[7, 7\], 发现\[5, 6\]已经存在，返回即可，\[7, 7\]仍旧需要继续拆分  
第四层会查询\[7, 7\]

任意长度的线段，最多被拆分成logn条线段树上存在的线段，所以查询的时间复杂度为O\(log\(n\)\)

```cpp
    int query(int beg, int end)
    {
        return queryHelper(root, beg, end);
    }


    int queryHelper(SegmentTreeNode* node, int start, int end)
    {
        if (!node)
            return INT_MIN;

        if (start <= node->start && node->end <= end) //如果全部在范围之内， 则取root
            return node->val;
        int mid = (node->start + node->end) /2; // 找到两个儿子的分界点
        int ans = INT_MIN;
        if (node->left && mid >= start) //左儿子的查找条件， 和左儿子有交集
            ans = std::max(ans, queryHelper(node->left, start, end));
        if (node->right && mid + 1 <= end)//和右儿子有交集
            ans = std::max(ans, queryHelper(node->right, start, end));
        return ans;
    }
```

## 线段树的修改

```
            [0,3]
           (val=4)
         /         \
     [0,1]         [2,3]
    (val=4)       (val=3)
    /    \         /    \
 [0,0]  [1,1]   [2,2]  [3,3]
(val=1)(val=4) (val=2)(val=3)
```

修改3,3 为 5 则更新 3，3   2，3， 0， 3

```cpp
    void modify(int ind, int val)
    {
        modifyHelper(root, ind, val);
    }


    void modifyHelper(SegmentTreeNode* node, int i, int v)
    {
        if (!node)
            return;
        // leaf node
        if (i== node->start && i== node->end)
        {
            node->val = v;
            return;
        }
        int mid = (node->start + node->end)/2;
        if (i <= mid )
        {
            modifyHelper(node->left, i, v);
            node->val = std::max(node->left? node->left->val: INT_MIN, node->right? node->right->val: INT_MIN);
        }
        else
        {
            modifyHelper(node->right, i, v);
            node->val = std::max(node->left? node->left->val: INT_MIN, node->right? node->right->val: INT_MIN);
        }
    }
```

# 区间最大线段树完整代码

```cpp
// Example program
#include <iostream>
#include <string>
#include <vector>
#include <climits>
struct SegmentTreeNode{

    SegmentTreeNode(int s, int e, int v)
        : start(s), end(e), val(v), left(nullptr), right(nullptr)
    {
    }
    ~SegmentTreeNode()
    {
        if (left)
            delete left;
        if (right)
            delete right;
    }
    int start;
    int end;
    int val;
    SegmentTreeNode* left;
    SegmentTreeNode* right;

};

class SegmentTree{
public:
    SegmentTree()
    {
        root = nullptr;
    }
    ~SegmentTree()
    {
        if (!root)
            delete root;
    }

    void build(const std::vector<int>& arr)
    {
        root = buildHelper(0, arr.size()-1, arr);
    }

    int query(int beg, int end)
    {
        return queryHelper(root, beg, end);
    }

    void modify(int ind, int val)
    {
        modifyHelper(root, ind, val);
    }

private:
    SegmentTreeNode* buildHelper(int beg, int end, const std::vector<int>& arr)
    {
        if (beg > end)
            return nullptr;
        SegmentTreeNode* node = new SegmentTreeNode(beg, end, arr[beg]);
        if (beg == end)
            return node;
        int mid = ( beg + end ) / 2;
        node->left = buildHelper(beg, mid, arr);
        node->right = buildHelper(mid+1, end, arr);
        node->val = std::max(node->left?node->left->val:INT_MIN, node->right? node->right->val : INT_MIN);
        return node;
    }

    int queryHelper(SegmentTreeNode* node, int start, int end)
    {
        if (!node)
            return INT_MIN;

        if (start <= node->start && node->end <= end)
            return node->val;
        int mid = (node->start + node->end) /2;
        int ans = INT_MIN;
        if (node->left && mid >= start)
            ans = std::max(ans, queryHelper(node->left, start, end));
        if (node->right && mid + 1 <= end)
            ans = std::max(ans, queryHelper(node->right, start, end));
        return ans;
    }

    void modifyHelper(SegmentTreeNode* node, int i, int v)
    {
        if (!node)
            return;
        // leaf node
        if (i== node->start && i== node->end)
        {
            node->val = v;
            return;
        }
        int mid = (node->start + node->end)/2;
        if (i <= mid )
        {
            modifyHelper(node->left, i, v);
            node->val = std::max(node->left? node->left->val: INT_MIN, node->right? node->right->val: INT_MIN);
        }
        else
        {
            modifyHelper(node->right, i, v);
            node->val = std::max(node->left? node->left->val: INT_MIN, node->right? node->right->val: INT_MIN);
        }
    }

private:
    SegmentTreeNode* root;
};

int main()
{
  std::vector<int> test{1,2, 3,4,5,6};
  SegmentTree st;
  st.build(test);
  std::cout<<st.query(2,4)<<std::endl;
  st.modify(3, 11);
  std::cout<<st.query(2,4)<<std::endl;
}
```

## 201. Segment Tree Build

The structure of Segment Tree is a binary tree which each node has two attributes`start`and`end`denote an segment / interval.

\_start\_and\_end\_are both integers, they should be assigned in following rules:

* The root's
  _start_
  and
  _end_
  is given by
  `build`
  method.
* The left child of node A has
  `start=A.left, end=(A.left + A.right) / 2`
  .
* The right child of node A has
  `start=(A.left + A.right) / 2 + 1, end=A.right`
  .
* if
  _start_
  equals to
  _end_
  , there will be no children for this node.

Implement a`build`method with two parameters_start\_and\_end_, so that we can create a corresponding segment tree with every node has the correct\_start\_and\_end\_value, return the root of this segment tree.

### Example

Given`start=0, end=3`. The segment tree will be:

```
               [0,  3]
             /        \
      [0,  1]           [2, 3]
      /     \           /     \
   [0, 0]  [1, 1]     [2, 2]  [3, 3]
```

Given`start=1, end=6`. The segment tree will be:

```
               [1,  6]
             /        \
      [1,  3]           [4,  6]
      /     \           /     \
   [1, 2]  [3,3]     [4, 5]   [6,6]
   /    \           /     \
[1,1]   [2,2]     [4,4]   [5,5]
```

[https://www.lintcode.com/problem/segment-tree-build/description](https://www.lintcode.com/problem/segment-tree-build/description)

### 解题分析:

O\(n\)

### 代码：

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end) {
 *         this->start = start, this->end = end;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    SegmentTreeNode * build(int start, int end) {
        // write your code here
        if (start > end) return nullptr;
        auto node = new SegmentTreeNode(start, end);
        if ( start == end ) return node;
        int mid = start + (end - start) / 2;
        node->left = build(start, mid);
        node->right = build(mid + 1, end);
        return node;
    }
};
```

## 202. Segment Tree Query

For an integer array \(index from 0 to n-1, where n is the size of this array\), in the corresponding SegmentTree, each node stores an extra attribute`max`to denote the maximum number in the interval of the array \(index from start to end\).

Design a`query`method with three parameters`root`,`start`and`end`, find the maximum number in the interval \[start, end\] by the given root of segment tree.

### Example

For array`[1, 4, 2, 3]`, the corresponding Segment Tree is:

```
                  [0, 3, max=4]
                 /             \
          [0,1,max=4]        [2,3,max=3]
          /         \        /         \
   [0,0,max=1] [1,1,max=4] [2,2,max=2], [3,3,max=3]
```

query\(root, 1, 1\), return`4`

query\(root, 1, 2\), return`4`

query\(root, 2, 3\), return`3`

query\(root, 0, 2\), return`4`

[https://www.lintcode.com/problem/segment-tree-query/description](https://www.lintcode.com/problem/segment-tree-query/description)

### 解题分析:

O\(log\(n\)\)

### 代码：

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end, max;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end, int max) {
 *         this->start = start;
 *         this->end = end;
 *         this->max = max;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param root: The root of segment tree.
     * @param start: start value.
     * @param end: end value.
     * @return: The maximum number in the interval [start, end]
     */
    int query(SegmentTreeNode * root, int start, int end) {
        // write your code here
        if (!root)
            return INT_MIN;
        if (start <= root->start && root->end <= end)
            return root->max;
        int mid = (root->start + root->end) / 2;
        int ans = INT_MIN;
        if (start <= mid && root->left)
            ans = max(ans, query(root->left, start, end));
        if (mid+1 <= end && root->right)
            ans = max(ans, query(root->right, start, end));
        return ans;
    }
};
```

## 203. Segment Tree Modify

For a`Maximum Segment Tree`, which each node has an extra value`max`to store the maximum value in this node's interval.

Implement a`modify`function with three parameter`root`,`index`and`value`to change the node's value with\_**\[start, end\] = \[index, index\]**\_to the new given value. Make sure after this change, every node in segment tree still has the**max**attribute with the correct value.

### Example

For segment tree:

```
                      [1, 4, max=3]
                    /                \
        [1, 2, max=2]                [3, 4, max=3]
       /              \             /             \
[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]
```

if call`modify(root, 2, 4)`, we can get:

```
                      [1, 4, max=4]
                    /                \
        [1, 2, max=4]                [3, 4, max=3]
       /              \             /             \
[1, 1, max=2], [2, 2, max=4], [3, 3, max=0], [4, 4, max=3]
```

**or**call`modify(root, 4, 0)`, we can get:

```
                      [1, 4, max=2]
                    /                \
        [1, 2, max=2]                [3, 4, max=0]
       /              \             /             \
[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=0]
```

### Challenge

Do it in`O(h)`time, h is the height of the segment tree.

[https://lintcode.com/problem/segment-tree-modify/description](https://lintcode.com/problem/segment-tree-modify/description)

### 解题分析:

O\(log\(n\)\)

### 代码：

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end, max;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end, int max) {
 *         this->start = start;
 *         this->end = end;
 *         this->max = max;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param root: The root of segment tree.
     * @param index: index.
     * @param value: value
     * @return: nothing
     */
    void modify(SegmentTreeNode * root, int index, int value) {
        // write your code here
        if (!root) return;
        if (root->start == root->end && root->start == index) {
            root->max = value;
            return;
        }
        int mid = (root->start + root->end)/2;
        if (index <= mid) modify(root->left, index, value);
        if (index >= mid + 1) modify(root->right, index, value);
        root->max = INT_MIN;
        root->max = max(root->max , root->left? root->left->max : INT_MIN);
        root->max = max(root->max , root->right? root->right->max : INT_MIN);
    }
};
```

## 207. Interval Sum II

Given an integer array in the construct method, implement two methods`query(start, end)`and`modify(index, value)`

### Challenge

O\(logN\) time for`query`and`modify`.

[https://www.lintcode.com/problem/interval-sum-ii/description](https://www.lintcode.com/problem/interval-sum-ii/description)

### 解题分析:

O\(n\)

### 代码：

```cpp
class Solution {
public:

    Solution(vector<int> A) {
        // do intialization if necessary
        if (A.empty()) return;
        root = build(A, 0, A.size());
    }

    long long query(int start, int end) {
        // write your code here
        return query(root, start, end);
    }

    void modify(int index, int value) {
        // write your code here
        modify(root, index, value);
    }

private:
    struct Node {
        Node* left = nullptr;
        Node* right = nullptr;
        int start;
        int end;
        long long sum;
        Node(int s, int e, int sum) : start(s), end(e), sum(sum) {};
    };
    Node* root;
private:
    Node* build(vector<int>& A, int beg, int end) {
        if (end < beg) return nullptr;
        auto node = new Node(beg, end, A[beg]);
        if (beg == end) return node;
        int mid = (beg + end) / 2;
        node->left = build(A, beg, mid);
        node->right = build(A, mid+1, end);
        node->sum = node->left->sum + node->right->sum;
        return node;
    }

    long long query(Node* root, int start, int end) {
        if (!root || start > end) return 0;
        if (start <= root->start && root->end <= end) return root->sum;
        int mid = (root->start + root->end) / 2;
        long long res = 0;
        if (start <= mid && root->left) res += query(root->left, start, end);
        if (mid + 1 <= end && root->right) res += query(root->right, start, end);
        return res;
    }

    void modify(Node* root, int index, int value) {
        if (!root) return;
        if (root->start == root->end && root->start == index) {
            root->sum = value;
            return;
        }
        int mid = (root->start + root->end) / 2;
        if (index <= mid && root->left) modify(root->left, index, value);
        else modify(root->right, index, value);
        root->sum = 0;
        if (root->left) root->sum += root->left->sum;
        if (root->right) root->sum += root->right->sum;
    }
};
```

## 439. Segment Tree Build II

The structure of Segment Tree is a binary tree which each node has two attributes`start`and`end`denote an segment / interval.

\_start\_and\_end\_are both integers, they should be assigned in following rules:

* The root's
  _start_
  and
  _end_
  is given by
  `build`
  method.
* The left child of node A has
  `start=A.left, end=(A.left + A.right) / 2`
  .
* The right child of node A has
  `start=(A.left + A.right) / 2 + 1, end=A.right`
  .
* if
  _start_
  equals to
  _end_
  , there will be no children for this node.

Implement a`build`method with a given array, so that we can create a corresponding segment tree with every node value represent the corresponding interval max value in the array, return the root of this segment tree.

### Example

Given`[3,2,1,4]`. The segment tree will be:

```
                 [0,  3] (max = 4)
                  /            \
        [0,  1] (max = 3)     [2, 3]  (max = 4)
        /        \               /             \
[0, 0](max = 3)  [1, 1](max = 2)[2, 2](max = 1) [3, 3] (max = 4)
```

[https://www.lintcode.com/problem/segment-tree-build-ii/description](https://www.lintcode.com/problem/segment-tree-build-ii/description)

### 解题分析:

O\(n\)

### 代码：

```cpp
class Solution {
public:
    SegmentTreeNode * build(vector<int> &A) {
        return helper(A, 0, A.size() - 1);
    }

    SegmentTreeNode* helper(vector<int> &A, int start, int end) {
        if (start > end) return nullptr;
        auto node = new SegmentTreeNode(start, end, A[start]);
        if (start == end) return node;
        int mid = (start + end) / 2;
        node->left = helper(A, start, mid);
        node->right = helper(A, mid + 1, end);
        node->max = max(node->left->max, node->right->max);
        return node;
    }
};
```

## 247. Segment Tree Query II

For an array, we can build a`SegmentTree`for it, each node stores an extra attribute`count`to denote the number of elements in the the array which value is between interval start and end. \(The array may not fully filled by elements\)

Design a`query`method with three parameters`root`,`start`and`end`, find the number of elements in the in array's interval \[_start_,_end_\] by the given root of value SegmentTree.

### Example

For array`[0, 2, 3]`, the corresponding value Segment Tree is:

```
                     [0, 3, count=3]
                     /             \
          [0,1,count=1]             [2,3,count=2]
          /         \               /            \
   [0,0,count=1] [1,1,count=0] [2,2,count=1], [3,3,count=1]
```

`query(1, 1)`, return`0`

`query(1, 2)`, return`1`

`query(2, 3)`, return`2`

`query(0, 2)`, return`2`

[https://www.lintcode.com/problem/segment-tree-query-ii/description](https://www.lintcode.com/problem/segment-tree-query-ii/description)

### 解题分析:

O\(logn\)

### 代码：

```cpp
class Solution {
public:
    int query(SegmentTreeNode * root, int start, int end) {
        // write your code here
        if (!root || start > end) return 0;
        if (start <= root->start && root->end <= end) return root->count;
        int mid = (root->start + root->end)/2;
        int res = 0;
        if (start <= mid && root->left) res += query(root->left, start, end);
        if (end >= mid + 1 && root->right) res += query(root->right, start, end);
        return res;
    }
};
```

## 248. Count of Smaller Number

Give you an integer array \(index from 0 to n-1, where n is the size of this array, value from 0 to 10000\) and an query list. For each query, give you an integer, return the number of element in the array that are smaller than the given integer.

### Example

For array`[1,2,7,8,5]`, and queries`[1,8,5]`, return`[0,4,2]`

### Challenge

Could you use three ways to do it.

1. Just loop
2. Sort and binary search
3. Build Segment Tree and Search.

[https://www.lintcode.com/problem/count-of-smaller-number/description](https://www.lintcode.com/problem/count-of-smaller-number/description)

### 解题分析:

O\(n + klog\(n\)\)

### 代码：

```cpp
class Solution {
public:
    vector<int> countOfSmallerNumber(vector<int> &A, vector<int> &queries) {
        vector<int> res;
        if (A.empty()) return vector<int>(queries.size(), 0);
        int minV = INT_MAX;
        int maxV = INT_MIN;
        for (auto each : A) {
            minV = min(each, minV);
            maxV = max(each, maxV);
        }
        auto root = build(minV, maxV);
        for (auto each : A) modify(root, each);
        for (auto each : queries) res.push_back(query(root, each));
        delete root;
        return res;
    }

private:
    struct Node {
        Node* left = nullptr;
        Node* right = nullptr;
        int start;
        int end;
        int count = 0;
        Node(int s, int e) : start(s), end(e) {}
    };

    Node* build(int start, int end) {
        if (start > end) return nullptr;
        auto node = new Node(start, end);
        if (start == end) return node;
        int mid = (start + end) / 2;
        node->left = build(start, mid);
        node->right = build(mid + 1, end);
        return node;
    }

    void modify(Node* root, int index) {
        if (!root) return;
        if  (root->start == root->end && root->start == index) {
            root->count++;
            return;
        }
        int mid = (root->start + root->end) / 2;
        if (index <= mid) modify(root->left, index);
        else modify(root->right, index);
        root->count = 0;
        if (root->left) root->count += root->left->count;
        if (root->right) root->count += root->right->count;
    }

    int query(Node* root, int index) {
        if (!root || root->start >= index) return 0;
        if (root->end < index) return root->count;
        return query(root->left, index) + query(root->right, index);
    } 
};
```

## 206. Interval Sum

Given an integer array \(index from 0 to n-1, where n is the size of this array\), and an query list. Each query has two integers`[start, end]`. For each query, calculate the sum number between index start and end in the given array, return the result list.

### Example

For array`[1,2,7,8,5]`, and queries`[(0,4),(1,2),(2,4)]`, return`[23,9,20]`

### Challenge

O\(logN\) time for each query

[https://www.lintcode.com/problem/count-of-smaller-number/description](https://www.lintcode.com/problem/interval-sum/description)

### 解题分析:

这道题根本就不是用segment tree解决，浪费时间用的， 完全加权之后取两端就好O（1）

### 代码：

```cpp
class Solution {
public:
    vector<long long> intervalSum(vector<int> &A, vector<Interval> &queries) {
        // write your code here
        vector<long long> sumArr{0};
        for (auto each : A) sumArr.push_back(sumArr.back() + each);
        vector<long long> res;
        for (const auto& each : queries) {
            int start = each.start;
            int end = each.end + 1;
            if (start > sumArr.size()-1 || end <= 0) {
                res.push_back(0);
                continue;
            }
            if (start < 0) start = 0;
            if (end > sumArr.size() - 1) end = sumArr.size() - 1;
            res.push_back(sumArr[end] - sumArr[start]);
        }
        return res;
    }
};
```

## 205. Interval Minimum Number

Given an integer array \(index from 0 to n-1, where n is the size of this array\), and an query list. Each query has two integers`[start, end]`. For each query, calculate the minimum number between index start and end in the given array, return the result list.

### Example

For array`[1,2,7,8,5]`, and queries`[(1,2),(0,4),(2,4)]`, return`[2,1,5]`

### Challenge

O\(logN\) time for each query

[https://www.lintcode.com/problem/interval-minimum-number/description](https://www.lintcode.com/problem/interval-minimum-number/description)

### 解题分析:

这道题根本就不是用segment tree解决，浪费时间用的， 完全加权之后取两端就好O（1）

### 代码：

```cpp
/**
 * Definition of Interval:
 * classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

struct Node{
  Node(int b, int e, int v)
    :beg(b), end(e), val(v), left(nullptr), right(nullptr)
  {}
  ~Node()
  {
      if (left) delete left;
      if (right) delete right; 
  }

  int beg, end, val;
  Node *left,*right;
};

class Solution {
public:
    /**
     * @param A: An integer array
     * @param queries: An query list
     * @return: The result list
     */
    vector<int> intervalMinNumber(vector<int> &A, vector<Interval> &queries) {
        // write your code here
        vector<int> res;
        auto root= build(0, A.size()-1, A);
        for (auto& interval:queries)
        {
            res.push_back(query(root, interval.start, interval.end));
        }
        if (root)
            delete root;
        return res;
    }

    Node* build(int beg, int end, vector<int>& A )
    {
        if (beg > end)
            return nullptr;
        auto node = new Node(beg, end, A[beg]);
        if (beg == end)
            return node;
        int mid = (beg + end)/2;
        node->left = build(beg, mid, A);
        node->right = build(mid+1, end, A);
        node->val = INT_MAX;
        if (node->left)
            node->val = min(node->val, node->left->val);
        if (node->right)
            node->val = min(node->val, node->right->val);
        return node;
    }
    int query(Node* root, int beg, int end)
    {
        if (!root)
            return INT_MAX;
        if (beg <= root->beg && root->end <= end)
            return root->val;
        int mid = (root->beg + root->end)/2;
        int c = INT_MAX;
        if (beg <= mid)
            c = min(c, query(root->left, beg, end));
        if (mid + 1 <=end)
            c= min(c, query(root->right, beg, end));
        return c;
    }

};
```

## 249. Count of Smaller Number before itself

Give you an integer array \(index from 0 to n-1, where n is the size of this array, data value from 0 to 10000\) . For each element`Ai`in the array, count the number of element before this element`Ai`is smaller than it and return count number array.

### Example

For array`[1,2,7,8,5]`, return`[0,1,2,3,2]`

[https://www.lintcode.com/problem/count-of-smaller-number-before-itself/description](https://www.lintcode.com/problem/count-of-smaller-number-before-itself/description)

### 解题分析:

边build边query

### 代码：

```cpp
struct Node{
  Node(int b, int e, int c)
    : beg(b), end(e), count(c), left(nullptr),right(nullptr)
  {}
  ~Node()
  {
      if (left)
        delete left;
      if (right)
        delete right;
  }
  int beg, end, count;
  Node *left, *right;
};


class Solution {
public:
    /**
     * @param A: An integer array
     * @param queries: The query list
     * @return: The number of element in the array that are smaller that the given integer
     */
    vector<int> countOfSmallerNumberII(vector<int> &A) {
        // write your code here
        auto root = build(0, 10000);
        vector<int> res;
        for (auto& each : A)
        {
            modify(root, each);
            res.push_back(query(root, each));
        }

        if (root)
            delete root;
        return res;
    }

    Node* build(int beg, int end)
    {
        if (beg > end)
            return nullptr;
        auto node = new Node(beg, end, 0);
        if (beg == end)
            return node;
        int mid = (beg + end)/2;
        node->left = build(beg, mid);
        node->right = build(mid+1, end);
        return node;
    }

    int query(Node* root, int q)
    {
        if (!root || root->beg >= q)
            return 0;
        if (root->end < q)
            return root->count;
        return query(root->left, q) + query(root->right, q);
    }

    void modify(Node* root, int v)
    {
        if (!root)
            return;
        if (root->beg == root->end && root->beg == v)
        {
            root->count += 1;
            return;
        }
        int mid = (root->beg + root->end) / 2;
        if (v <= mid)
            modify(root->left, v);
        else
            modify(root->right,v);
        root->count = 0;
        if (root->left)
            root->count += root->left->count;
        if (root->right)
            root->count += root->right->count;
    }
};
```

Leetcode 315 有类似题目

[https://leetcode.com/problems/count-of-smaller-numbers-after-self/](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

第一版用的multiset， 超时了，看了眼就是因为重复元素很多，单因为BST所以还contribute 复杂度， 且二分在重复情况下退化为o\(n\)

线段树无重复问题，相对上题存在负值问题，所有找到最小数，build, modify, query的时候shift一下

```
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        multiset<int> data;
        vector<int> res;
        for (auto it = nums.rbegin(); it != nums.rend(); ++it) {
            auto p = data.lower_bound(*it);
            if (p == data.end()) {
                res.push_back(data.size());
            } else {
                res.push_back(distance(data.begin(), p));
            }
            data.insert(*it);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};

class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        if (nums.empty()) return {};
        int minV = INT_MAX;
        int maxV = INT_MIN;
        for (auto& num:nums) {
            minV = min(minV, num);
            maxV = max(maxV, num);
        }
        auto root= build(0, maxV - minV);
        vector<int> res;
        for (auto it = nums.rbegin(); it != nums.rend(); it++) {
            res.push_back(query(root, *it - minV));
            modify(root, *it - minV);
        }
        reverse(res.begin(), res.end());
        return res;
    }
    
private:
    struct Node {
        Node* left = nullptr;
        Node* right = nullptr;
        int start = 0;
        int end = 0;
        int count = 0;
        Node(int start, int end) : start(start), end(end) {}
        ~Node() {if (left) delete left; if (right) delete right;}
     };
    
    Node* build(int start, int end) {
        if ( start > end) return nullptr;
        auto node = new Node(start, end);
        if (start == end) return node;
        int mid = (start + end) / 2;
        node->left = build(start, mid);
        node->right = build(mid+1, end);     
        return node;
    }
    
    void modify(Node* root, int index) {
        if (!root || root->start > index || root->end < index) return;
        if (root->start == root->end && root->start == index) {
            root->count++;
            return;
        }
        int mid = (root->start + root->end) / 2;
        if (index <= mid) modify(root->left, index);
        else modify(root->right, index);
        root->count = 0;
        if (root->left) root->count += root->left->count;
        if (root->right) root->count += root->right->count;
    } 
    
    int query(Node* root, int v) {
        if (!root || root->start >= v) return 0;
        if (root->end < v ) return root->count;
        return query(root->left, v) + query(root->right, v);
    }
};
```






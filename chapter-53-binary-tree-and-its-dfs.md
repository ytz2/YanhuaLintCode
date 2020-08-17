## 595. Binary Tree Longest Consecutive Sequence

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child \(`cannot be the reverse`\).

Have you met this question in a real interview?

Yes

**Example**

For example,

```
   1
    \
     3
    / \
   2   4
        \
         5
```

Longest consecutive sequence path is`3-4-5`, so return`3`.

```
   2
    \
     3
    / 
   2    
  / 
 1
```

Longest consecutive sequence path is`2-3`,not`3-2-1`, so return`2`.

[https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence/\#](https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence/#)

[https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)

### 解题分析:

搜索问题，从上往下找，如果连续， +1， 不连续，设为1

### 代码：

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
    struct ReturnType {
        int max = 0;
        int length = 0;
    };
public:
    int longestConsecutive(TreeNode* root) {
        return helper(root).max;
    }
    
    ReturnType helper(TreeNode* root) {
        ReturnType res;
        if (!root) return res;
        auto l = helper(root->left);
        auto r = helper(root->right);
        auto len = 1;
        if (root->left && root->val + 1 == root->left->val) len += l.length;
        if (root->right && root->val + 1 == root->right->val) len = max(len, 1+r.length);
        res.length = len;
        res.max = max(len, max(l.max, r.max));
        return res;
    }
};
```

### 复杂度分析:

o\(n\)

2020/08/17 重新写为打擂台的形式

## 614. Binary Tree Longest Consecutive Sequence II

Given a binary tree, find the length of the longest consecutive sequence path.  
The path could be start and end at any node in the tree

Have you met this question in a real interview?

Yes

**Example**

```
    1
   / \
  2   0
 /
3
```

Return`4`//`0-1-2-3`

[https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-ii/](https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-ii/)

### 解题分析:

解决的问题是

找出左边连续增长，右边连续增长的长度

返回最长的增长长度和最短的减少长度+1 （如果连续的话）

还要打一个擂台， 左右两边连起来，增长，降低， 看谁最大，更新maxLen

### 代码：

```cpp
/**
 * Definition of TreeNode:
 * class TreeNode {
 * public:
 *     int val;
 *     TreeNode *left, *right;
 *     TreeNode(int val) {
 *         this->val = val;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

struct ReturnType{
    ReturnType(int i, int d)
        : inc(i), dec(d)
        {}
    int inc;
    int dec;
};

class Solution {
public:
    /**
     * @param root: the root of binary tree
     * @return: the length of the longest consecutive sequence path
     */
    int longestConsecutive2(TreeNode * root) {
        // write your code here
        if (!root)
            return 0;
        maxLen = INT_MIN;
        auto res = helper(root);
        return maxLen;
    }

    ReturnType helper(TreeNode * root)
    {
        if (!root)
            return ReturnType(0, 0);
        ReturnType res(1, 1);
        if (!root->left && !root->right)
            return res;

        int lIncrease = 0, lDecrease = 0;
        if (root->left)
        {
            auto left = helper(root->left);
            if (root->val + 1 == root->left->val)
                lIncrease = left.inc;
            if (root->val -1 == root->left->val)
                lDecrease = left.dec;
        }
        int rIncrease = 0, rDecrease = 0;
        if (root->right)
        {
            auto right = helper(root->right);
            if (root->val+1 == root->right->val)
                rIncrease = right.inc;
            if (root->val -1 == root->right->val)
                rDecrease = right.dec;
        }
        res.inc = max(lIncrease, rIncrease)+1;
        res.dec = max(lDecrease, rDecrease)+1; 

        maxLen = max(maxLen, max(lIncrease + rDecrease, lDecrease + rIncrease) +1);
        return res;
    }

    int maxLen;

};
```

### 复杂度分析:

o\(n\)

## 619. Binary Tree Longest Consecutive Sequence III

It's follow up problem for[`Binary Tree Longest Consecutive Sequence II`](https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-ii/)

Given a`k-ary tree`, find the length of the longest consecutive sequence path.  
The path could be start and end at any node in the tree

Have you met this question in a real interview?

Yes

**Example**

An example of test data: k-ary tree`5<6<7<>,5<>,8<>>,4<3<>,5<>,3<>>>`, denote the following structure:

```
     5
   /   \
  6     4
 /|\   /|\
7 5 8 3 5 3

Return 5, // 3-4-5-6-7
```

[https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-iii/](https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-iii/)

### 解题分析:

其实follow up 更简单

### 代码：

```cpp
/**
 * Definition for a multi tree node.
 * struct MultiTreeNode {
 *     int val;
 *     vector<MultiTreeNode*> children;
 *     MultiTreeNode(int x) : val(x) {}
 * };
 */

class Solution {
public:
    /**
     * @param root the root of k-ary tree
     * @return the length of the longest consecutive sequence path
     */
    int longestConsecutive3(MultiTreeNode* root) {
        // Write your code here
        if (!root)
            return 0;
        m_max = INT_MIN;
        auto res = helper(root);
        return m_max;
    }

    struct ReturnType{
      ReturnType(int i, int d)
        : inc(i), dec(d)
        {};
      int inc;
      int dec;
    };

    ReturnType helper(MultiTreeNode* root)
    {
        if (!root)
            return ReturnType(0, 0);
        int inc = 0, dec = 0;
        for (auto each : root->children)
        {
            auto res = helper(each);
            if (root->val+1 == each->val)
                inc = max(inc, res.inc);
            if (root->val-1 == each->val)
                dec = max(dec, res.dec);
        }
        m_max = max(m_max, inc+dec+1);
        return ReturnType(inc+1, dec+1);
    }

    int m_max;
};
```

### 复杂度分析:

o\(n\)

## 94. Binary Tree Maximum Path Sum

Given a binary tree, find the maximum path sum.

The path may start and end at any node in the tree.

Have you met this question in a real interview?

Yes

**Example**

Given the below binary tree:

```
  1
 / \
2   3
```

return`6`.

[https://www.lintcode.com/en/problem/binary-tree-maximum-path-sum/\#](https://www.lintcode.com/en/problem/binary-tree-maximum-path-sum/#)

### 解题分析:

左边最大和右边最大求得后，要做选择， root-&gt;val, left+root-&gt;val, right+root-&gt;val 返回最大，因为 返回的可能是负数。

max就是打一个擂台就好了

### 代码：

```cpp
/**
 * Definition of TreeNode:
 * class TreeNode {
 * public:
 *     int val;
 *     TreeNode *left, *right;
 *     TreeNode(int val) {
 *         this->val = val;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param root: The root of binary tree.
     * @return: An integer
     */
    int maxPathSum(TreeNode * root) {
        // write your code here
        if (!root)
            return 0;
        m_max = INT_MIN;
        auto s = helper(root);
        return m_max;
    }

    int helper(TreeNode* root)
    {
        if (!root)
            return 0;
        int left = helper(root->left);
        int right = helper(root->right);
        int ret = max(root->val, max(root->val+left, root->val+right));
        m_max = max(m_max, max(ret, root->val + left+right));
        return ret;
    }

    int m_max;
};
```

### 复杂度分析:

o\(n\)

## 475. Binary Tree Maximum Path Sum II

Given a binary tree, find the maximum path sum from root.

The path may end at any node in the tree and contain at least one node in it.

Have you met this question in a real interview?

Yes

**Example**

Given the below binary tree:

```
  1
 / \
2   3
```

return`4`. \(1-&gt;3\)

[https://www.lintcode.com/en/problem/binary-tree-maximum-path-sum-ii/\#](https://www.lintcode.com/en/problem/binary-tree-maximum-path-sum-ii/#)

### 解题分析:

从根节点，沿着路径走，无脑preorder+回溯就好了

### 代码：

```cpp
/**
 * Definition of TreeNode:
 * class TreeNode {
 * public:
 *     int val;
 *     TreeNode *left, *right;
 *     TreeNode(int val) {
 *         this->val = val;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param root: the root of binary tree.
     * @return: An integer
     */
    int maxPathSum2(TreeNode * root) {
        // write your code here
        if (!root)
            return 0;
        m_max = INT_MIN;
        int sum = 0;
        preorder(root, sum);
        return m_max;
    }

    void preorder(TreeNode* root, int& sum)
    {
        if (!root)
            return;
         sum += root->val;
         m_max = max(m_max, sum);
         preorder(root->left,sum);
         preorder(root->right,sum);
         sum -= root->val;
    }

    int m_max;
};
```

### 复杂度分析:

o\(n\)

## 520. Consistent Hashing II

在 Consistent Hashing I 中我们介绍了一个比较简单的一致性哈希算法，这个简单的版本有两个缺陷：

1. 增加一台机器之后，数据全部从其中一台机器过来，这一台机器的读负载过大，对正常的服务会造成影响。
2. 当增加到3台机器的时候，每台服务器的负载量不均衡，为1:1:2。

为了解决这个问题，引入了 micro-shards 的概念，一个更好的算法是这样：

1. 将 360° 的区间分得更细。从 0~359 变为一个 0 ~ n-1 的区间，将这个区间首尾相接，连成一个圆。
2. 当加入一台新的机器的时候，随机选择在圆周中撒 k 个点，代表这台机器的 k 个 micro-shards。
3. 每个数据在圆周上也对应一个点，这个点通过一个 hash function 来计算。
4. 一个数据该属于那台机器负责管理，是按照该数据对应的圆周上的点在圆上顺时针碰到的第一个 micro-shard 点所属的机器来决定。

n 和 k在真实的 NoSQL 数据库中一般是 2^64 和 1000。

请实现这种引入了 micro-shard 的 consistent hashing 的方法。主要实现如下的三个函数：

1. create\(int n, int k\)
2. addMachine\(int machine\_id\) // add a new machine, return a list of shard ids.
3. getMachineIdByHashCode\(int hashcode\) // return machine id

##### Notice

当 n 为 2^64 时，在这个区间内随机基本不会出现重复。  
但是为了方便测试您程序的正确性，n 在数据中可能会比较小，所以你必须保证你生成的 k 个随机数不会出现重复。  
LintCode并不会判断你addMachine的返回结果的正确性（因为是随机数），只会根据您返回的addMachine的结果判断你getMachineIdByHashCode结果的正确性。

[https://www.lintcode.com/en/problem/consistent-hashing-ii/](https://www.lintcode.com/en/problem/consistent-hashing-ii/)

### 解题分析:

As is

### 代码：

```cpp
class Solution {
public:
    /*
     * @param n: a positive integer
     * @param k: a positive integer
     * @return: a Solution object
     */
    static Solution create(int n, int k) {
        // Write your code here
        Solution instance;
        instance.n_ = n;
        instance.k_ = k;
        return instance;
    }

    /*
     * @param machine_id: An integer
     * @return: a list of shard ids
     */
    vector<int> addMachine(int machine_id) {
        // write your code here
        vector<int> res;
        for (int i = 0; i<k_; i++)
        {
            while(true)
            {
                int microShard = rand() % n_;
                if (shards_.find(microShard) != shards_.end())
                    continue;
                else
                {
                    shards_[microShard] = machine_id;
                    res.push_back(microShard);
                    break;
                }
            }

        }
        return res;
    }

    /*
     * @param hashcode: An integer
     * @return: A machine id
     */
    int getMachineIdByHashCode(int hashcode) {
        // write your code here
        int hash = hashcode % n_;
        if (shards_.find(hash) !=shards_.end())
            return shards_[hash];
        auto up = shards_.upper_bound(hash);
        if (up == shards_.end())
            return shards_.begin()->second;
        return up->second;
    }

    int n_;
    int k_;
    map<int, int> shards_;
};
```

### 




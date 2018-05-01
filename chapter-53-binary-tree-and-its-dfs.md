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

### 解题分析:

搜索问题，从上往下找，如果连续， +1， 不连续，设为1

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
     * @param root: the root of binary tree
     * @return: the length of the longest consecutive sequence path
     */
    int longestConsecutive(TreeNode * root) {
        // write your code here
        if (!root)
            return 0;
        maxLen = INT_MIN;
        len = 0; 
        helper(root, nullptr);
        return maxLen;
    }

    void helper(TreeNode* root, TreeNode* prev)
    {
        if (!root)
            return;
        if (!prev || prev->val+1 != root->val)
            len = 1;
        else
            len++;
        maxLen = max(maxLen, len);
        prev = root;
        helper(root->left, root);
        helper(root->right, root);
    }

    int maxLen;
    int len;
};
```

### 复杂度分析:

o\(n\)

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

https://www.lintcode.com/en/problem/binary-tree-maximum-path-sum/\#

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


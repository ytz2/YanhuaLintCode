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

https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-ii/

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


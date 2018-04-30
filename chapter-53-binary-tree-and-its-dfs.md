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

https://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence/\#

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


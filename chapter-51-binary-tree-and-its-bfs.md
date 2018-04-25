## 数的形态

## 596. Minimum Subtree

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

##### Notice

LintCode will print the subtree which root is your return node.  
It's guaranteed that there is only one subtree with minimum sum and the given binary tree is not an empty tree.

Have you met this question in a real interview?

Yes

**Example**

Given a binary tree:

```
     1
   /   \
 -5     2
 / \   /  \
0   2 -4  -5
```

return the node`1`.

[https://www.lintcode.com/en/problem/minimum-subtree/\#](https://www.lintcode.com/en/problem/minimum-subtree/#)

### 解题分析:

做两种解法吧， 一种是divide and conquer 的返回returnType,一种 是打擂台。。。其实这种题目打擂台要简洁一些

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

struct ReturnType
{
    int minSum;
    TreeNode* node;
    int sum;
    ReturnType(int mSum, TreeNode* nd, int csum)
        :minSum(mSum), node(nd), sum(csum)
        {}
};
class Solution {
public:
    /**
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */
    TreeNode * findSubtree(TreeNode * root) {
        // write your code here
        auto res = helper(root);
        return res.node;
    }

    ReturnType helper(TreeNode * root)
    {
        if (!root)
            return ReturnType(INT_MAX, nullptr, 0);

        auto left = helper(root->left);
        auto right = helper(root->right);
        auto sum = left.sum + right.sum + root->val;

        if ( sum < left.minSum && sum < right.minSum)
            return ReturnType(sum, root, sum);

        return left.minSum < right.minSum? ReturnType(left.minSum, left.node, sum) : ReturnType(right.minSum, right.node, sum);

    }

};
```

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
     * @return: the root of the minimum subtree
     */
    TreeNode * findSubtree(TreeNode * root) {
        // write your code here
        m_sum = INT_MAX;
        m_node = nullptr;
        int val = helper(root);
        return m_node;
    }

    int helper(TreeNode * root)
    {
        if (!root)
            return INT_MAX;
        int left = helper(root->left);
        int right = helper(root->right);
        int sum = root->val;
        if (root->left)
            sum += left;
        if (root->right)
            sum += right;
        if (sum < m_sum)
        {
            m_sum = sum;
            m_node = root;
        }
        return sum;
    }

    int m_sum;
    TreeNode* m_node;

};
```

### 

### 复杂度分析:

o\(n\)



## 数的形态

## 480. Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

Have you met this question in a real interview?

Yes

**Example**

Given the following binary tree:

```
   1
 /   \
2     3
 \
  5

```

All root-to-leaf paths are:

```
[
  "1-
>
2-
>
5",
  "1-
>
3"
]
```

https://www.lintcode.com/en/problem/binary-tree-paths/\#

### 解题分析:

路径问题，前序遍历，但是要注意路径的定义， 左右孩子全无才算路径终点， 就不算divide conquer了，用traversal

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
     * @param root: the root of the binary tree
     * @return: all root-to-leaf paths
     */
    vector<string> binaryTreePaths(TreeNode * root) {
        // write your code here
        vector<string> res;
        if (!root)
            return res;
        string s;
        helper(root, res, s);
        return res;
    }
    
    void helper(TreeNode* root, vector<string>& vec, string& s)
    {
        static string delim = "->";
        auto val= to_string(root->val) +delim; 
        s += val;
        if (!root->left && !root->right)
            vec.push_back(s.substr(0, s.size() - 2));
        if (root->left)
            helper(root->left, vec, s);
        if (root->right)
            helper(root->right, vec, s);
        s = s.substr(0, s.size()-val.size());
    }
};
```



### 复杂度分析:

o\(n\)


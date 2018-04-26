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

[https://www.lintcode.com/en/problem/binary-tree-paths/\#](https://www.lintcode.com/en/problem/binary-tree-paths/#)

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

## 88. Lowest Common Ancestor

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor\(LCA\) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

##### Notice

Assume two nodes are exist in tree.

Have you met this question in a real interview?

Yes

**Example**

For the following binary tree:

```
  4
 / \
3   7
   / \
  5   6
```

LCA\(3, 5\) =`4`

LCA\(5, 6\) =`7`

LCA\(6, 7\) =`7`

[https://www.lintcode.com/en/problem/lowest-common-ancestor/\#](https://www.lintcode.com/en/problem/lowest-common-ancestor/#)

### 解题分析:

祖先问题， 一般都是先左右后中间，所以模式上是一个后序遍历，但是有个tricky的地方就是递归的退出不再是root== null，而是root == null or root==A or root==B, 这样保证了递归总是从最底部返回一个指向至少一个的位置，如果子树不存在A,B则返回null...

算是非典型的树的题目，但是有共同原理，

if root == null or root ==A or root ==B

```
return root;
```

visit left

visit right

if left and right ne null

```
return root;
```

if left

return left

if right

```
return right
```

return null

代码：

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
    /*
     * @param root: The root of the binary search tree.
     * @param A: A TreeNode in a Binary.
     * @param B: A TreeNode in a Binary.
     * @return: Return the least common ancestor(LCA) of the two nodes.
     */
    TreeNode * lowestCommonAncestor(TreeNode * root, TreeNode * A, TreeNode * B) {
        // write your code here
        if (!root || root==A || root == B)
            return root;
        auto left = lowestCommonAncestor(root->left, A, B);
        auto right = lowestCommonAncestor(root->right,A,B);
        if (left && right)
            return root;
        if (left)
            return left;
        if (right)
            return right;
        return nullptr;
    }
};
```

### 复杂度分析:

o\(n\)


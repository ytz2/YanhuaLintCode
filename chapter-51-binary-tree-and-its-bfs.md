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

## 578. Lowest Common Ancestor III

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor\(LCA\) of the two nodes.  
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.  
Return`null`if LCA does not exist.

##### Notice

node A or node B may not exist in tree.

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

### 解题分析:

和上题不一样， A,B不一定存在，所以不能着急退出， 而是在遍历完左右孩子后，判断A,B是否为root,只有两个都有且则由上个题目衍生出来的结论才成立

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
    /*
     * @param root: The root of the binary tree.
     * @param A: A TreeNode
     * @param B: A TreeNode
     * @return: Return the LCA of the two nodes.
     */
    TreeNode * lowestCommonAncestor3(TreeNode * root, TreeNode * A, TreeNode * B) {
        // write your code here
        hasA= false; hasB=false;
        auto v = helper(root, A,B);
        return hasA && hasB? v:nullptr;
    }

    TreeNode* helper(TreeNode* root, TreeNode* A, TreeNode* B)
    {
        if (!root )
            return root;
        auto left =helper(root->left, A, B);
        auto right = helper(root->right, A, B);
        if (root == A)
            hasA = true;
        if (root == B)
            hasB = true;
        if (root == A || root == B)
            return root;
        if (left && right)
            return root;
        if (left)
            return left;
        if (right)
            return right;
        return nullptr;
    }

    bool hasA;
    bool hasB;

};
```

### 复杂度分析:

o\(n\)

## 小结

树的最大最小求和，divide & conquere 或者遍历打擂台

树的路径前序遍历

树的祖先后续遍历， 共同祖先有个小技巧，只能记住了。。。

## 453. Flatten Binary Tree to Linked List

Flatten a binary tree to a fake "linked list" in pre-order traversal.

Here we use the\_right\_pointer in TreeNode as the\_next\_pointer in ListNode.

##### Notice

Don't forget to mark the left child of each node to null. Or you will get Time Limit Exceeded or Memory Limit Exceeded.

Have you met this question in a real interview?

Yes

**Example**

```
              1
               \
     1          2
    / \          \
   2   5    =
>
    3
  / \   \          \
 3   4   6          4
                     \
                      5
                       \
                        6
```

[https://www.lintcode.com/en/problem/flatten-binary-tree-to-linked-list/\#](https://www.lintcode.com/en/problem/flatten-binary-tree-to-linked-list/#)

### 解题分析:

前序遍历可以求得路径，那么一般情况下就是把左面指空，右面指向左面，左面的最右面指向右面

所以以上分析可知：

1.需要知道最右的叶子节点

2.左右变形后需要执行分析步骤

1. corner: 左面没有，不需要变，只需要返回右面的最右叶子 2右面没有， 左面换到右面，返回左面的最右叶子

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
     * @param root: a TreeNode, the root of the binary tree
     * @return: nothing
     */
    void flatten(TreeNode * root) {
        // write your code here
        auto tmp =helper(root);
    }

    TreeNode* helper(TreeNode* root)
    {
        if (!root || !root->left && !root->right)
            return root;
        auto left = helper(root->left);
        auto right = helper(root->right);

        if (!left)
            return right;
        if (!right)
        {
            root->right = root->left;
            root->left = nullptr;
            return left;
        }
        auto tmp = root->right;
        root->right = root->left;
        root->left = nullptr;
        left->left = nullptr;
        left->right = tmp;
        return right;
    }
};
```

### 复杂度分析:

o\(n\)

![](/assets/BSTchar.png)




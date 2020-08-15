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

2020/08/14, rewrite with modern cpp

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
    struct ReturnType {
        int minVal;
        TreeNode* minNode;
        int sum;
        ReturnType(int m, TreeNode* n, int s)
        :minVal(m), minNode(n), sum(s) {}
    };

    TreeNode * findSubtree(TreeNode * root) {
        return helper(root).minNode;
    }

    ReturnType helper(TreeNode* root) {
        ReturnType res(INT_MAX, root, 0);
        if (!root) return res;
        auto left = helper(root->left);
        auto right = helper(root->right);
        res.sum = left.sum + right.sum + root->val;
        res.minVal = res.sum;
        if (left.minVal < res.minVal) {
            res.minVal = left.minVal;
            res.minNode = left.minNode;
        }
        if (right.minVal < res.minVal) {
            res.minVal = right.minVal;
            res.minNode = right.minNode;
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    /**
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */
    TreeNode * findSubtree(TreeNode * root) {
        if (!root)
            return nullptr;
        minSum(root);
        return minRoot;
    }

    int minSum(TreeNode* root) {
        if (!root)
            return 0;
        auto v = root->val + minSum(root->left) + minSum(root->right);
        if (v < minVal) {
            minVal = v;
            minRoot = root;
        }     
        return v;
    }

    TreeNode* minRoot = nullptr;
    int minVal = INT_MAX;
};
```

### 

### 复杂度分析:

o\(n\)

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

[https://leetcode.com/problems/binary-tree-paths/](https://leetcode.com/problems/binary-tree-paths/)

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

2020/08/14

```go
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *ListNode
 *     Right *ListNode
 * }
 */
 func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
     if (root == nil) {
         return nil
     }     
     if (root == p || root == q) {
         return root
     }
     l := lowestCommonAncestor(root.Left, p, q)
     r := lowestCommonAncestor(root.Right, p, q)
     if ( l != nil && r != nil) {
         return root
     }
     if (l != nil ) {
         return l
     }
     if (r != nil ) {
         return r
     }
     return nil

 }
```

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

[https://www.lintcode.com/problem/lowest-common-ancestor-iii/description](https://www.lintcode.com/problem/lowest-common-ancestor-iii/description)

### 解题分析:

和上题不一样， A,B不一定存在，所以不能着急退出， 而是在遍历完左右孩子后，判断A,B是否为root,只有两个都有且则由上个题目衍生出来的结论才成立

### 代码：

2020/08/14 重写了一下， 以前是搞两个状态量has A has B, 重写以后先假设他必然有解， 然后再验证，这样感觉更易懂易读一些

```cpp
class Solution {
public:
    /*
     * @param root: The root of the binary tree.
     * @param A: A TreeNode
     * @param B: A TreeNode
     * @return: Return the LCA of the two nodes.
     */
    TreeNode * lowestCommonAncestor3(TreeNode * root, TreeNode * A, TreeNode * B) {
        auto res = helper(root, A, B);
        if (res && Find(res, A) && Find(res, B)) return res;
        return nullptr;
    }

    TreeNode * helper(TreeNode * root, TreeNode * A, TreeNode * B) {
        if (!root) return nullptr;
        if (root == A || root == B) return root;
        auto l = helper(root->left, A, B);
        auto r = helper(root->right, A, B);
        if (l && r) return root;
        if (l) return l;
        if (r) return r;
        return nullptr;
    }

    TreeNode* Find(TreeNode* root, TreeNode* t) {
        if (!root) return nullptr;
        if (root == t) return root;
        auto l = Find(root->left, t);
        if (l) return l;
        auto r = Find(root->right, t);
        if (r) return r;
        return nullptr;
    }
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
public:
    void flatten(TreeNode* root) {
        helper(root);
    }

    pair<TreeNode*, TreeNode*> helper(TreeNode* root) {
        if (!root) 
            return make_pair(nullptr, nullptr);
        auto ret = make_pair(root, root);
        if (!root->left && !root->right) 
            return ret;
        auto left = root->left;
        auto right = root->right;
        if (left) {
            auto l = helper(left);
            ret.second->right = l.first;
            ret.second = l.second;
        }
        if (right) {
            auto r = helper(right);
            ret.second->right = r.first;
            ret.second = r.second;
        }
        root->left = nullptr;
        return ret;
    }
};
```

### 复杂度分析:

o\(n\)

![](/assets/BSTchar.png)

## 902. Kth Smallest Element in a BST

Given a binary search tree, write a function`kthSmallest`to find the kth smallest element in it.

##### Notice

You may assume k is always valid,`1 ≤ k ≤ BST's total elements`.

Have you met this question in a real interview?

Yes

**Example**

Given root =`{1,#,2}`, k =`2`, return`2`.

[**Challenge**](https://www.lintcode.com/en/problem/kth-smallest-element-in-a-bst/#challenge)

What if the BST is modified \(insert/delete operations\) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

[https://www.lintcode.com/en/problem/kth-smallest-element-in-a-bst/](https://www.lintcode.com/en/problem/kth-smallest-element-in-a-bst/)

[https://leetcode.com/problems/kth-smallest-element-in-a-bst/](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

### 解题分析:

最开始把问题考虑复杂了，类比了quick select的算法：

因为这个必然有解，则唯一出口就是左边有k-1个数， root为当前值

按照quick select的思想：

左边如果大于k-1个，答案必然在左边 直接返回就好

如果为k-1，返回root

如果小于k-1, 则在右边找第k-left-1个数

看了一眼别人的答案，觉得自己真傻， inorder加一个计数器，到第k个停下好了。

challenge 很有意思， 可能我的解法就有用了，增减删太频繁之后需要维护第k个，那么o\(h+k\)可能不太够，如果要尽量优化的话就是把count 维护在node中，或者建立一个hashmap计数， 每次修改都去维护hashtable的计数器。然后用我第一版答案来解，会是O\(k）的速度。

### 代码:

1.

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
     * @param root: the given BST
     * @param k: the given k
     * @return: the kth smallest element in BST
     */
    int kthSmallest(TreeNode * root, int k) {
        // write your code here
        int count = 0;
        auto res = helper(root, k, count);
        return res->val;
    }

    TreeNode* helper(TreeNode* root, int k, int& count)
    {
        if (!root)
            return nullptr;
        int leftCount = 0;
        auto left = helper(root->left, k, leftCount);
        if (left)
            return left;
        if (leftCount == k-1)
            return root;
        int rightCount = 0;
        auto right = helper(root->right, k-leftCount-1, rightCount);
        count =  leftCount + rightCount+1;
        return right;
    }
};
```

### 2

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
     * @param root: the given BST
     * @param k: the given k
     * @return: the kth smallest element in BST
     */
    int kthSmallest(TreeNode * root, int k) {
        // write your code here
        ind = 0;
        helper(root, k);
        return val;
    }

    void helper(TreeNode* root, int k)
    {
        if (!root)
            return;
        helper(root->left, k);
        ind++;
        if (ind == k)
        {
            val =root->val;
            return;
        }
        helper(root->right, k);
    }


    int ind;
    int val;
};
```

### 复杂度分析:

o\(k+h\)

2020/08/14

```cpp
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        return helper(root, k)->val;
    }

    TreeNode* helper(TreeNode* root, int& k) {
        if (!root) return nullptr;
        auto l = helper(root->left, k);
        if (l) return l;
        k--;
        if (k == 0) return root;
        auto r = helper(root->right, k);
        if (r) return r;
        return nullptr;
    }
};
```




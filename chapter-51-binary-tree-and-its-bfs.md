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

## 902. Flatten Binary Tree to Linked List

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

## 86. Binary Search Tree Iterator

Design an iterator over a binary search tree with the following rules:

* Elements are visited in ascending order \(i.e. an in-order traversal\)
* `next()`
  and
  `hasNext()`
  queries run in O\(
  _1_
  \) time in
  **average**
  .

Have you met this question in a real interview?

Yes

**Example**

For the following binary search tree, in-order traversal by using iterator is`[1, 6, 10, 11, 12]`

```
   10
 /    \
1      11
 \       \
  6       12
```

[https://www.lintcode.com/en/problem/binary-search-tree-iterator/\#](https://www.lintcode.com/en/problem/binary-search-tree-iterator/#)

### 解题分析:

用一个stack一直存到最左边

下一个：

top为下一个返回值， 但是每次返回后：

1.如果cur有右面，一直走到右面的最左边

2 如果cur没右面， 一直走到第一个左拐点 ： while\(!stk.empty\(\) && stk.top\(\)-&gt;right == node\) node = stk.top\(\), stk.pop\(\)

### 代码:

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
 * Example of iterate a tree:
 * BSTIterator iterator = BSTIterator(root);
 * while (iterator.hasNext()) {
 *    TreeNode * node = iterator.next();
 *    do something for node
 */


class BSTIterator {
public:
    /*
    * @param root: The root of binary tree.
    */BSTIterator(TreeNode * root) {
        // do intialization if necessary
        while(root)
        {
            stk.push(root);
            root=root->left;
        }
    }

    /*
     * @return: True if there has next node, or false
     */
    bool hasNext() {
        // write your code here
        return !stk.empty();
    }

    /*
     * @return: return next node
     */
    TreeNode * next() {
        // write your code here
        auto cur = stk.top();
        stk.pop();
        if (cur->right)
        {
            auto node = cur->right;
            while(node)
            {
                stk.push(node);
                node = node->left;
            }
        }
        else
        {
            auto node = cur;
            while(!stk.empty() && stk.top()->right == node)
            {
                node = stk.top();
                stk.pop();
            }
        }
        return cur;
    }

private:
    stack<TreeNode*> stk;
};
```

amortized o\(1\)

## \*\*\*\*\*900. Closest Binary Search Tree Value

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

##### Notice

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

Have you met this question in a real interview?

Yes

**Example**

Given root =`{1}`, target =`4.428571`, return`1`.

[https://www.lintcode.com/en/problem/closest-binary-search-tree-value/](https://www.lintcode.com/en/problem/closest-binary-search-tree-value/)

### 解题分析:

第一版，用了遍历打擂台， o\(n\)的办法， 肯定不是最优,第二版用的上下边界求，写了好久不AC， 妈的。 只能熟记了。。。STL里mutltiset用的就是这个玩意

### 代码:

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
     * @param target: the given target
     * @return: the value in the BST that is closest to the target
     */
    int closestValue(TreeNode * root, double target) {
        // write your code here
        val = 0;
        minDiff =  std::numeric_limits<double>::max();
        helper(root, target);
        return val;
    }

    void helper(TreeNode* root, double target)
    {
        if (!root)
            return;
        double diff = fabs((double)root->val-target);
        if (diff < minDiff)
        {
            minDiff = diff;
            val = root->val;
        }
        helper(root->left, target);
        helper(root->right, target);
    }

    int val;
    double minDiff;
};
```

o\(n\)

第二版，上边界，下边界，两边比一下，这样就是o\(log\(n\)\)

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
     * @param target: the given target
     * @return: the value in the BST that is closest to the target
     */
    int closestValue(TreeNode * root, double target) {
        // write your code here
        if (!root)
            return INT_MAX;
        auto lower = lower_bound(root, target);
        auto upper = upper_bound(root, target);
        if (!lower)
            return upper->val;
        if (!upper)
            return lower->val;
        return target-lower->val < upper->val-target? lower->val : upper->val;
    }

    TreeNode* lower_bound(TreeNode* root, double target)
    {
        if (!root)
            return root;
        if (root->val > target)
            return lower_bound(root->left, target);
        // root->val <= target
        auto node = lower_bound(root->right, target);
        if (node)
            return node;
        return root;
    }

    TreeNode* upper_bound(TreeNode* root, double target)
    {
        if (!root)
            return nullptr;
        if (root->val <= target)
            return upper_bound(root->right, target);
        auto node = upper_bound(root->left, target);
        if (node)
            return node;
        return root;
    }
};
```

## 93. Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Have you met this question in a real interview?

Yes

**Example**

Given binary tree A =`{3,9,20,#,#,15,7}`, B =`{3,#,20,15,7}`

```
A)  3            B)    3 
   / \                  \
  9  20                 20
    /  \                / \
   15   7              15  7
```

The binary tree A is a height-balanced binary tree, but B is not.

[https://www.lintcode.com/en/problem/balanced-binary-tree/\#](https://www.lintcode.com/en/problem/balanced-binary-tree/#)

### 解题分析:

左右两边的，D&Q。。。

### 代码:

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
    bool isBalanced;
    int depth;
    ReturnType(bool isB, int d)
        : isBalanced(isB), depth(d)
        {}
};

class Solution {
public:
    /**
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
    bool isBalanced(TreeNode * root) {
        // write your code here
        return helper(root).isBalanced;
    }

    ReturnType helper(TreeNode* root)
    {
        if (!root)
            return ReturnType(true, 0);
        auto left = helper(root->left);
        auto right = helper(root->right);
        if (!left.isBalanced || !right.isBalanced)
            return ReturnType(false, 0);
        return ReturnType(abs(left.depth-right.depth)<=1, 1+max(left.depth, right.depth));
    }
};
```

o\(n\) worst

## \*\*\*\*901. Closest Binary Search Tree Value II

Given a non-empty binary search tree and a target value, find`k`values in the BST that are closest to the target.

##### Notice

* Given target value is a floating point.
* You may assume
  `k`
  is always valid, that is:
  `k ≤ total`
  nodes.
* You are guaranteed to have only one
  `unique`
  set of k values in the BST that are closest to the target.

Have you met this question in a real interview?

Yes

**Example**

Given root =`{1}`, target =`0.000000`, k =`1`, return`[1]`.

[**Challenge**](https://www.lintcode.com/en/problem/closest-binary-search-tree-value-ii/#challenge)

Assume that the BST is balanced, could you solve it in less than O\(n\) runtime \(where n = total nodes\)?

[https://www.lintcode.com/en/problem/closest-binary-search-tree-value-ii/](https://www.lintcode.com/en/problem/closest-binary-search-tree-value-ii/)

### 解题分析:

因为已经排好序了，转成vec然后二分搜索找到后向两边衍射。。。可惜肯定不是这么解的，先写一个解，然后优化空间复杂度

当然也可以在中序遍历中维护一个排好序的list,如果新的candidate好于当前任意一个数值则插入，且删除最大diff

### 代码:

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
     * @param target: the given target
     * @param k: the given k
     * @return: k values in the BST that are closest to the target
     */
    vector<int> closestKValues(TreeNode * root, double target, int k) {
        // write your code here
        vector<int> res; 
        if (!root || k<=0)
            return res;
        vector<int> vec;
        ToVec(root, vec);
        if (vec.size() <=1)
            return vec;
        int beg = 0, end =vec.size()-1;
        while(beg + 1 < end)
        {
            int mid = beg + (end-beg)/2;
            if (vec[mid]< target)
                beg = mid;
            else
                end = mid;
        }

        while(res.size() <k )
        {
            while(beg >= 0 && end < vec.size() && res.size() <k)
            {
                if (abs(vec[beg] - target) < abs(vec[end]-target))
                    res.push_back(vec[beg--]);
                else
                    res.push_back(vec[end++]);
            }
            while(beg >=0 && res.size() < k)
                res.push_back(vec[beg--]);
            while(end < vec.size() && res.size() <k)
                res.push_back(vec[end++]);
        }
        return res;
    }

    void ToVec(TreeNode* root, vector<int>& vec)
    {
        if (!root)
            return;
        ToVec(root->left, vec);
        vec.push_back(root->val);
        ToVec(root->right, vec );
    }
};
```

O\(n\)空间+O\(n\)时间复杂度

第二版

牺牲了时间复杂度，把空间复杂度降到o\(k\) 时间复杂度变成O\(KN\) ， 还是不是很划算

这个解法里面我终于搞懂了怎么往PQ里传带第三个参数的comparator。 算是个好的practice... 当然，一般在很大序列中维护一个最小pq， 也是解决这这种问题的最好的思路， 如果这个不是BFS 而是一般的tree, NLOG\(N\) vs O\(KN\) 也还好， 而且空间上占优。 但是比起网上log\(n\) 空间的不好，可惜我没看懂

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


struct cmp{

    cmp(double t = 0)
        :target(t)
        {}
    double target;
    bool operator()(TreeNode* left, TreeNode* right)
    {
        return abs(left->val - target) < abs(right->val-target);
    }
};

class Solution {
public:
    /**
     * @param root: the given BST
     * @param target: the given target
     * @param k: the given k
     * @return: k values in the BST that are closest to the target
     */

    typedef priority_queue<TreeNode*, vector<TreeNode*>, cmp> PQ;
    PQ pq;
    vector<int> closestKValues(TreeNode * root, double target, int k) {
        // write your code here
        pq = PQ(cmp(target));
        helper(root, target, k);
        vector<int> res;
        while(!pq.empty())
        {
            auto node = pq.top();
            pq.pop();
            res.push_back(node->val);
        }
        return res;
    }

    void helper(TreeNode* root, double target, int k)
    {
        if (!root)
            return;
        helper(root->left, target, k);

        if (pq.size() < k)
        {
            pq.push(root);
        }
        else
        {
            if (fabs(pq.top()->val - target) > fabs(root->val - target))
            {
                pq.pop();
                pq.push(root);
            }
        }
        helper(root->right, target, k);
    }
};
```




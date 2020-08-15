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

[https://leetcode.com/problems/binary-search-tree-iterator/](https://leetcode.com/problems/binary-search-tree-iterator/)

### 解题分析:

2020/08/14， 看了一眼以前的解法，画图后觉得以前的解有问题

### 代码:

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
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        while(root) {
            stk.push(root);
            root = root->left;
        }
    }
    
    /** @return the next smallest number */
    int next() {
        auto cur = stk.top();
        stk.pop();
        int v = cur->val;
        if (cur->right) {
            cur = cur->right;
            while(cur){
                stk.push(cur);
                cur = cur->left;
            }
        }
        return v;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !stk.empty();
    }
    stack<TreeNode*> stk;
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
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

## 95. Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree \(BST\).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys
  **less than**
  the node's key.
* The right subtree of a node contains only nodes with keys
  **greater than**
  the node's key.
* Both the left and right subtrees must also be binary search trees.
* A single node tree is a BST

Have you met this question in a real interview?

Yes

**Example**

An example:

```
  2
 / \
1   4
   / \
  3   5
```

The above binary tree is serialized as`{2,1,4,#,#,3,5}`\(in level order\).

[https://www.lintcode.com/en/problem/validate-binary-search-tree/](https://www.lintcode.com/en/problem/validate-binary-search-tree/)

### 解题分析:

写的漂亮挺难

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

struct ReturnType
{
    ReturnType(bool isbst, int minval, int maxval)
        :isBST(isbst), minVal(minval), maxVal(maxval){}
    bool isBST;
    int minVal;
    int maxVal;
};

class Solution {
public:
    /**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
    bool isValidBST(TreeNode * root) {
        // write your code here
        return helper(root).isBST;
    }

    ReturnType helper(TreeNode* root){

        // local
        if (!root)
            return ReturnType(true, INT_MAX, INT_MIN);

        ReturnType res(true, root->val, root->val);
        if (root->left)
        {
            auto left = helper(root->left);
            if (!left.isBST || left.maxVal >= root->val)
            {
                res.isBST = false;
                return res;
            }
            res.minVal = min(res.minVal, left.minVal);
        }

        if (root->right)
        {
            auto right = helper(root->right);
            if (!right.isBST || right.minVal <= root->val)
            {
                res.isBST = false;
                return res;
            }
            res.maxVal = max(res.maxVal, right.maxVal);
        }

        return res;

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




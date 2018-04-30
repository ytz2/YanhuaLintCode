## 597. Subtree with Maximum Average

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

##### Notice

LintCode will print the subtree which root is your return node.  
It's guaranteed that there is only one subtree with maximum average.

Have you met this question in a real interview?

Yes

**Example**

Given a binary tree:

```
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2
```

return the node`11`.

[https://www.lintcode.com/en/problem/subtree-with-maximum-average/](https://www.lintcode.com/en/problem/subtree-with-maximum-average/)

### 解题分析:

打擂台+post order

### 代码：

```
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
     * @return: the root of the maximum average of subtree
     */
    TreeNode * findSubtree2(TreeNode * root) {
        // write your code here
        node = nullptr;
        maxAvg = INT_MIN;
        int count, sum;
        helper(root, count, sum);
        return node;
    }

    void helper(TreeNode* root, int& count, int& sum)
    {
        if (!root)
        {
            count = 0;
            sum = 0;
            return;
        }
        count = 1;
        sum = root->val;
        if (root->left)
        {
            int leftCount, leftSum;
            helper(root->left, leftCount, leftSum);
            count+= leftCount;
            sum+= leftSum;
        }
        if (root->right)
        {
            int rightCount, rightSum;
            helper(root->right, rightCount, rightSum);
            sum+= rightSum;
            count+= rightCount;
        }
        double avg = double(sum)/double(count);
        if (avg > maxAvg)
        {
            maxAvg = avg;
            node = root;
        }
    }

    double maxAvg;
    TreeNode* node;
};
```

### 复杂度分析:

o\(n\)

## 474. Lowest Common Ancestor II

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor\(LCA\) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute`parent`which point to the father of itself. The root's parent is null.

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

[https://www.lintcode.com/en/problem/lowest-common-ancestor-ii/](https://www.lintcode.com/en/problem/lowest-common-ancestor-ii/)

### 解题分析:

因为这个有了parent, 所以找起来方便一些， 可以先分别从root 出发，找到A, B， 再从 A出发，向上搜索， 从parent出发找B

### 代码：

```
/**
 * Definition of ParentTreeNode:
 * class ParentTreeNode {
 * public:
 *     int val;
 *     ParentTreeNode *parent, *left, *right;
 * }
 */


class Solution {
public:
    /*
     * @param root: The root of the tree
     * @param A: node in the tree
     * @param B: node in the tree
     * @return: The lowest common ancestor of A and B
     */
    ParentTreeNode * lowestCommonAncestorII(ParentTreeNode * root, ParentTreeNode * A, ParentTreeNode * B) {
        // write your code here
        auto a = findNode(root, A);
        auto b = findNode(root, B);
        if (!a || !b)
            return nullptr;
        if (findNode(a, B))
            return a;
        if (findNode(b, A))
            return b;
        auto node = a;
        while(node != root)
        {
            if (node->parent == B)
                return node->parent;
            if (node->parent->left == node && findNode(node->parent->right, B))
                return node->parent;
            if (node->parent->right == node && findNode(node->parent->left, B))
                return node->parent;
            node = node->parent;
        }
        return root;
    }

    ParentTreeNode* findNode(ParentTreeNode* root, ParentTreeNode* obj)
    {
        if (!root)
            return nullptr;
        if (root == obj)
            return root;
        auto left = findNode(root->left, obj);
        if (left)
            return left;
        auto right= findNode(root->right, obj);
        if (right)
            return right;
        return nullptr;
    }
};
```

### 复杂度分析:

o\(n\)

## \*\*\*\*246. Binary Tree Path Sum II

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.

Have you met this question in a real interview?

Yes

**Example**

Given a binary tree:

```
    1
   / \
  2   3
 /   /
4   2
```

for target =`6`, return

```
[
  [2, 4],
  [1, 3, 2]
]
```

[https://www.lintcode.com/en/problem/binary-tree-path-sum-ii/](https://www.lintcode.com/en/problem/binary-tree-path-sum-ii/)

### 解题分析:

这道烂题花了好长时间， 刚开始觉得是回溯，就是把当前的combon放上去，然后重新起头，继续找。 但是最后发现总有重复，加了hash也还是老有错。

其实path sum就是preorder, tricky的地方是每次把当前node push上去后，然后以当前node为开头，找到所有的以当前Node为起点的combo

代码：

```
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
     * @param root: the root of binary tree
     * @param target: An integer
     * @return: all valid paths
     */
    vector<vector<int>> binaryTreePathSum2(TreeNode * root, int target) {
        // write your code here
        vector<int> buffer;
        vector<vector<int>> res;
        helper(root, target, res, buffer);
        return res;
    }

    void helper(TreeNode* root, int target, vector<vector<int>>& res, vector<int>& buffer)
    {
        if (!root)
            return;

        buffer.push_back(root->val);

        // put all the combos into res which start with root->val
        int i = buffer.size()-1;
        int sum = 0;
        while(i>=0)
        {
            sum += buffer[i];
            if (sum == target)
            {
                vector<int> t(buffer.begin()+i, buffer.end());
                res.push_back(move(t));
            }
            i--;
        }

        helper(root->left, target, res, buffer);
        helper(root->right, target, res, buffer);
        buffer.pop_back();
    }
};
```

### 复杂度分析:

NA

## 155. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Have you met this question in a real interview?

Yes

**Example**

Given a binary tree as follow:

```
  1
 / \ 
2   3
   / \
  4   5
```

The minimum depth is`2`.

[https://www.lintcode.com/en/problem/minimum-depth-of-binary-tree/](https://www.lintcode.com/en/problem/minimum-depth-of-binary-tree/)

### 解题分析:

如果没有子树，则不算树深

### 代码：

```
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
     * @param root: The root of binary tree
     * @return: An integer
        */
    int minDepth(TreeNode * root) {
        // write your code here
        if (!root)
            return 0;
        int left = minDepth(root->left);
        int right =minDepth(root->right);

        if (!left)
            return right+1;
        if (!right)
            return left+1;
        return min(left,right)+1;
    }
};
```

### 复杂度分析:

O（N\)

## 97. Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Have you met this question in a real interview?

Yes

**Example**

Given a binary tree as follow:

```
  1
 / \ 
2   3
   / \
  4   5
```

The maximum depth is`3`

[https://www.lintcode.com/en/problem/maximum-depth-of-binary-tree/](https://www.lintcode.com/en/problem/maximum-depth-of-binary-tree/)

### 解题分析:

NA

### 代码：

```
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
    int maxDepth(TreeNode * root) {
        // write your code here
        if (!root)
            return 0;
        return max(maxDepth(root->left), maxDepth(root->right))+1;
    }
};
```

### 复杂度分析:

O（N\)

#### Binary Tree Inorder Traversal & Binary Tree Preorder Traversal & Inorder Predecessor in BST

非递归版本，网上找下吧。。。没有意义

## 915. Inorder Predecessor in BST

Given a binary search tree and a node in it, find the in-order predecessor of that node in the BST.

##### Notice

If the given node has no in-order predecessor in the tree, return`null`

Have you met this question in a real interview?

Yes

**Example**

Given root =`{2,1,3}`, p = 1, return`null`.

[https://www.lintcode.com/en/problem/inorder-predecessor-in-bst/](https://www.lintcode.com/en/problem/inorder-predecessor-in-bst/)

### 解题分析:

BST lower\_bound问题， 重复了, 发现OJ的BST无重复元素， 要处理一下重复，比如有好多2， 2， 2,2 p指到中间， 那么当 root和p不是同一个node的时候，要往右一直走到root-right = p为止。 否则，root=p，则认为p是第一个元素，我们要从左面找一个比他小的。

这个是和lower-bound 不一样的地方

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
     * @param root: the given BST
     * @param p: the given node
     * @return: the in-order predecessor of the given node in the BST
     */
    TreeNode * inorderPredecessor(TreeNode * root, TreeNode * p) {
        // write your code here
        if (!root || !p )
            return nullptr;

        if (root->val == p->val && root != p)
        {
            while(root->right && root->right->val == p->val && root->right!=p)
            {
                root = root->right;
            }
            return root;
        }

        if (root->val >= p->val)
            return inorderPredecessor(root->left, p);
        // root->val < p->val
        auto node = inorderPredecessor(root->right, p);
        if (node)
            return node;
        return root;
    }
};
```

### 复杂度分析:

log\(n\)

## 448. Inorder Successor in BST

Given a binary search tree \([See Definition](https://www.lintcode.com/problem/validate-binary-search-tree/)\) and a node in it, find the in-order successor of that node in the BST.

If the given node has no in-order successor in the tree, return`null`.

##### Notice

It's guaranteed\_p\_is one node in the given tree. \(You can directly compare the memory address to find p\)

Have you met this question in a real interview?

Yes

**Example**

Given tree =`[2,1]`and node =`1`:

```
  2
 /
1
```

return node`2`.

Given tree =`[2,1,3]`and node =`2`:

```
  2
 / \
1   3
```

return node`3`.

[https://www.lintcode.com/en/problem/inorder-successor-in-bst/](https://www.lintcode.com/en/problem/inorder-successor-in-bst/)

### 解题分析:

BST upper\_bound问题， 这个比前序简单点， 因为当p-&gt;right-val == p-&gt;val的时候，必定是他的后序。 否则必然是p的upper bound

### 代码：

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */


class Solution {
public:
    /*
     * @param root: The root of the BST.
     * @param p: You need find the successor node of p.
     * @return: Successor of p.
     */
    TreeNode * inorderSuccessor(TreeNode * root, TreeNode * p) {
        // write your code here
        if (!root || !p)
            return nullptr;
        if (p->right && p->right->val == p->val)
            return p->right;
        if (root->val <= p->val)
            return inorderSuccessor(root->right, p);
        // root->val >= p->val
        auto node = inorderSuccessor(root->left, p);
        return node == nullptr? root : node;
    }
};
```

### 复杂度分析:

log\(n\)

## \*\*\*\*472. Binary Tree Path Sum III

Give a binary tree, and a target number, find all path that the sum of nodes equal to target, the path could be start and end at any node in the tree.

Have you met this question in a real interview?

Yes

**Example**

Given binary tree:

```
    1
   / \
  2   3
 /
4
```

and target =`6`. Return :

```
[
  [2, 4],
  [2, 1, 3],
  [3, 1, 2],
  [4, 2]
]
```

[https://www.lintcode.com/en/problem/binary-tree-path-sum-iii/](https://www.lintcode.com/en/problem/binary-tree-path-sum-iii/)

### 解题分析:

这道题只能暴力上了， 别想什么套路或者tricky的解法了。

前序遍历

从每一个点开始，向上，向左，向右搜索sum

因为要向上搜索，所以要避免递归的时候回到起点，保证只往上或者往下走，所以要加一个from作为阻止点。

### 代码：

```cpp
/**
 * Definition of ParentTreeNode:
 * class ParentTreeNode {
 * public:
 *     int val;
 *     ParentTreeNode *parent, *left, *right;
 * }
 */


class Solution {
public:
    /*
     * @param root: the root of binary tree
     * @param target: An integer
     * @return: all valid paths
     */
    vector<vector<int>> binaryTreePathSum3(ParentTreeNode * root, int target) {
        // write your code here
        vector<vector<int>> res;
        if (!root)
            return res;
        inOrder(root, target, res);
        return res;
    }
    
    void inOrder(ParentTreeNode* root, int target, vector<vector<int>>& res)
    {
        if (!root)
            return;
        vector<int> solution;
        findSum(root, target, nullptr, res, solution);
        inOrder(root->left, target, res);
        inOrder(root->right, target, res);
    }
    
    void findSum(ParentTreeNode* root, int target, ParentTreeNode* from, vector<vector<int>>& res, vector<int>& solution)
    {
        if (!root)
            return;
        target-=root->val;
        solution.push_back(root->val);
        if (target == 0)
            res.push_back(solution);
        // dfs p, l, r
        // but should avoid going back
        if (root->parent && root->parent != from)
            findSum(root->parent, target, root, res, solution);
        if (root->left && root->left != from)
            findSum(root->left, target, root, res, solution);
        if (root->right && root->right != from)
            findSum(root->right, target, root, res, solution);

        solution.pop_back();
    }
    
};
```

### 复杂度分析:

o\(nlog\(n\)\)


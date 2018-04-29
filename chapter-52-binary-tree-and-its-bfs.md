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

https://www.lintcode.com/en/problem/lowest-common-ancestor-ii/

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


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

https://www.lintcode.com/en/problem/subtree-with-maximum-average/

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


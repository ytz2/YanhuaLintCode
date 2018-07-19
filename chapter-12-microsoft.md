## 270. Closest Binary Search Tree Value

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

**Note:**

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

**Example:**

```
Input:
 root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3


Output:
 4
```

**Note:**  
这道题用的和BS有序数组一个思路，如val-target &gt; 0, 往小于0 （左边走） 否则右边

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
    int closestValue(TreeNode* root, double target) {
        long val = LONG_MAX;
        helper(root, target, val);
        return val;
    }
    
    void helper(TreeNode* root, double target, long& val)
    {
        if (!root)
            return;
        auto delta = target - root->val;
        if (fabs(val - target) > fabs(delta))
            val = root->val;
        if (delta > 0)
            helper(root->right, target, val);
        else
            helper(root->left, target, val);
    }
};
```




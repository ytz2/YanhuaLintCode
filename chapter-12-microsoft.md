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

## 106. Closest Binary Search Tree Value

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

**Note:**  
想明白inorder 左中右， postorder 左右中， 这样，postorder最后一个是root,找到Inorder的位置，就知道左右哪里开始哪里结束，然后就可以递归了

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

/*
*  The idea behind is 
*  Inorder : left mid right, Postorder: left, right mid 
*  this tells us the mid should always in the last element of postorder
*  post order [9, 15, 7, 20] 3
*  then go to inorder [9] 3 [15, 20, 7]
*  the size on left of inorder will be the same siaze of post order from begin
*  the size on right will be size from beg of right
*  do it recursively
*/
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return helper(inorder, 0, inorder.size()-1, postorder, 0, postorder.size()-1);
    }
    
    TreeNode* helper(vector<int>& inorder, int iStart, int iEnd, vector<int>& postorder, int pStart, int pEnd)
    {
        if (pStart > pEnd)
            return nullptr;
        if (pStart == pEnd)
            return new TreeNode(postorder[pEnd]);
        auto root = new TreeNode(postorder[pEnd]);
        int inorderPos = iEnd;
        while(inorder[inorderPos] != postorder[pEnd])
            inorderPos--;
        root->left = helper(inorder, iStart, inorderPos-1, postorder, pStart, pStart + inorderPos - iStart-1);
        root->right = helper(inorder, inorderPos+1, iEnd, postorder, pStart + inorderPos - iStart, pEnd-1);
        return root;
    }
};
```




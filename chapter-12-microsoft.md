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
inorder = [9,3,15,20,7]
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

## 73. Set Matrix Zeroes

Given a_m_x_n_matrix, if an element is 0, set its entire row and column to 0. Do it[**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

```
Input:
 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]

Output:
 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]

```

**Example 2:**

```
Input:
 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]

Output:
 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]

```

**Follow up:**

* A straight forward solution using O\(
  _m_
  _n_
  \) space is probably a bad idea.
* A simple improvement uses O\(
  _m_
  +
  _n_
  \) space, but still not the best solution.
* Could you devise a constant space solution?

**Note:**  
这道题的思路是In-place， 所以肯定要好几次循环用空间换时间

1. 第一次循环找左边和上边看要不要把他变成0
2. 第二次循环找从1-m, 1-n 用左边和上面做标记
3. 第三次循环1-m, 1-n用标记好的来设为0
4. 第四次循环，根据最早确定的是不是要把左边上边设为0来设置

```cpp
/*
* the idea behind here is to change the boundary only
*  x  0  x  0
*  0      
*  x 
*  0
* once boundary is marked, we can scan back to set it , this is so called in-place !
* so, leave [0][*] and [*][0] register for whehter to mark as 0, during run time do 1-m, 1-n map back to boundary
* special handle: any elme on first col or first row will mark it to 0, this needs to be done after looping 1-m, 1-n
*/

class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        if (0 == m)
            return;
        int n = matrix[0].size();
        if (0 == n)
            return;

        bool horizontal = matrix[0][0] == 0; 
        bool vertical = horizontal;
        
        for (int i = 0; i < m; i++)
            vertical = vertical || matrix[i][0] == 0;
        for (int j = 0; j < n; j++)
            horizontal = horizontal || matrix[0][j] == 0;
        
        for (int i = 1; i < m; i++)
        {
            for (int j = 1; j < n; j++)
            {
                if (matrix[i][j] == 0)
                {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m ; i++)
        {
            for (int j = 1; j < n; j++)
            {
                if (matrix[0][j] == 0 || matrix[i][0] == 0)
                    matrix[i][j] = 0;
            }
        }
        if (horizontal)
        {
            for (int j =0; j < n; j++)
                matrix[0][j] = 0;
        }
        if (vertical)
        {
            for (int i = 0; i < m; i++)
                matrix[i][0] = 0;
        }
    }
};
```




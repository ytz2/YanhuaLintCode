# LintCode Problems



## 433. Number of Islands

Given a boolean 2D matrix,`0`is represented as the sea,`1`is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.

Find the number of islands.

Have you met this question in a real interview?

Yes

**Example**

Given graph:

```
[
  [1, 1, 0, 0, 0],
  [0, 1, 0, 0, 1],
  [0, 0, 0, 1, 1],
  [0, 0, 0, 0, 0],
  [0, 0, 0, 0, 1]
]

```

return`3`.

http://www.lintcode.com/en/problem/number-of-islands/\#



### 解题分析:

这是一个由点及面的问题， 从一点找到周围邻居，并且mark为visited。 典型的BFS的问题

### 代码：

```
class Solution {
public:
    /**
     * @param grid: a boolean 2D matrix
     * @return: an integer
     */
    int numIslands(vector<vector<bool>> &grid) {
        // write your code here
        int count = 0;
        if (grid.empty() || grid[0].empty())
            return count;
        int m = grid.size(), n = grid[0].size();
        vector<int> dx{-1, 1, 0, 0};
        vector<int> dy{ 0, 0,-1, 1};
        queue<pair<int, int >> q;
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (!grid[i][j])
                    continue;
                count++;
                q.push(make_pair(i,j));
                while(!q.empty())
                {
                    auto node = q.front();
                    q.pop();
                    grid[node.first][node.second] = false;
                    for (int k = 0; k < 4; k++)
                    {
                        int new_i = node.first+dx[k];
                        int new_j = node.second+dy[k];
                        if (new_i>=0 && new_i < m && new_j >=0 && new_j <n && grid[new_i][new_j])
                            q.push(make_pair(new_i,new_j));
                    }
                }
            }
        }
        return count;
    }
};
```

### 复杂度分析:

o\(n\)



## 69. Binary Tree Level Order Traversal

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

Have you met this question in a real interview?

Yes

**Example**

Given binary tree `{3,9,20,#,#,15,7}`,

```
    3
   / \
  9  20
    /  \
   15   7

```



return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

http://www.lintcode.com/en/problem/binary-tree-level-order-traversal/\#



### 解题分析:

层级遍历问题

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
     * @param root: A Tree
     * @return: Level order a list of lists of integer
     */
    vector<vector<int>> levelOrder(TreeNode * root) {
        // write your code here
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if (!root)
            return res;
        q.push(root);
        while(!q.empty())
        {
            int n = q.size();
            vector<int> level;
            for (int i = 0; i < n ; i++)
            {
                auto node = q.front();
                level.push_back(node->val);
                q.pop();
                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
            res.push_back(level);
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)


# BFS

## 242. Convert Binary Tree to Linked Lists by Depth

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth \(e.g., if you have a tree with depth D, you'll have D linked lists\).

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

return

```
[
  1->null,
  2->3->null,
  4->null
]
```

[http://www.lintcode.com/en/problem/convert-binary-tree-to-linked-lists-by-depth/\#](http://www.lintcode.com/en/problem/convert-binary-tree-to-linked-lists-by-depth/#)

### 解题分析:

level traversal

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
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    /**
     * @param root the root of binary tree
     * @return a lists of linked list
     */
    vector<ListNode*> binaryTreeToLists(TreeNode* root) {
        // Write your code here
        vector<ListNode*> res;
        if (!root)
            return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            int n = q.size();
            ListNode dummy;
            ListNode* head = &dummy;
            for (int i = 0; i < n; i++)
            {
                auto node = q.front();
                q.pop();
                head->next= new ListNode(node->val);
                head = head->next;
                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
            head->next = nullptr;
            res.push_back(dummy.next);
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)

## \*\*\*\*624. Remove Substrings

Given a string`s`and a set of`n`substrings. You are supposed to remove every instance of those n substrings from s so that s is of the minimum length and output this minimum length.

Have you met this question in a real interview?

Yes

**Example**

Given s =`ccdaabcdbb`, substrs =`["ab", "cd"]`  
Return`2`

Explanation:  
`ccdaabcdbb`-&gt;`ccdacdbb`-&gt;`cabb`-&gt;`cb`\(length = 2\)

[http://www.lintcode.com/en/problem/remove-substrings/\#](http://www.lintcode.com/en/problem/remove-substrings/#)

### 解题分析:

这道题和shortest path edit那道题差不多， BFS搜索每一种可能。

### 代码：

```
class Solution {
public:
    /*
     * @param s: a string
     * @param dict: a set of n substrings
     * @return: the minimum length
     */
    int minLength(string &s, unordered_set<string> &dict) {
        // write your code here
        int minLen = INT_MAX;
        queue<string> q;
        q.push(s);
        unordered_set<string> visited;
        while(!q.empty())
        {
            auto sub = q.front();
            q.pop();
            minLen = min(minLen, (int)sub.size());
            for (const auto& each : dict)
            {
                // get every occurence of the dict element
                auto p = sub.find(each, 0);
                while( p != string::npos)
                {
                    auto val = sub.substr(0, p)+sub.substr(p+each.size());
                    if (visited.find(val) == visited.end())
                    {
                        q.push(val);
                        visited.insert(val);
                    }
                    p = sub.find(each, p+1);
                }
            }
        }
        return minLen;
    }
};
```

### 复杂度分析:

find o\(n + s\) , 平均出现k次， 字典m个， O\(m\*k\(n+s\)\)

### Note:

这道题提交了好几次， 开始以为很简单，结果要么是corner case没捕捉到，要么是剪枝没做好。 corner case是指的一个string可以在另外一个string中出现多次。 剪枝没做好是指的，处理过得数据不需要处理第二遍。

关键在这里：

```
                auto p = sub.find(each, 0);
                while( p != string::npos)
                {
                    auto val = sub.substr(0, p)+sub.substr(p+each.size());
                    if (visited.find(val) == visited.end())
                    {
                        q.push(val);
                        visited.insert(val);
                    }
                    p = sub.find(each, p+1);
                }
```

## 598. Zombie in Matrix

Given a 2D grid, each cell is either a wall`2`, a zombie`1`or people`0`\(the number zero, one, two\).Zombies can turn the nearest people\(up/down/left/right\) into zombies every day, but can not through wall. How long will it take to turn all people into zombies? Return`-1`if can not turn all people into zombies.



**Example**

Given a matrix:

```
0 1 2 0 0
1 0 0 2 1
0 1 0 0 0

```

return`2`

http://www.lintcode.com/en/problem/zombie-in-matrix/\#

### 解题分析:

Flood Fill， 没啥悬念的感觉。。。直接写吧，以前做graphics model的时候天天写

### 代码：

```
class Solution {
public:
    /**
     * @param grid: a 2D integer grid
     * @return: an integer
     */
    int zombie(vector<vector<int>> &grid) {
        // write your code here
        if (grid.empty() || grid[0].empty())
            return -1;
        
        int n_people = 0;
        int res = -1;
        queue<pair<int, int>> q;
        int m = grid.size();
        int n = grid[0].size();
        for (int i = 0; i < m; i++)
        {
            for (int j =0 ; j < n; j++)
            {
                int v = grid[i][j];
                if (v == 0)
                    n_people++;
                else if (v == 1)
                    q.push(make_pair(i, j));
            }
        }
        vector<int> delta_x{ 0, 0, 1, -1};
        vector<int> delta_y{ 1, -1,0,  0};
        while(!q.empty())
        {
            res++;
            int curS = q.size();
            for (int i = 0; i <curS; i++)
            {
                auto p = q.front();
                q.pop();
                for (int k = 0; k < 4; k++)
                {
                    int next_i = p.first + delta_x[k];
                    int next_j = p.second + delta_y[k];
                    if (next_i >= 0 && next_i < m &&
                        next_j >= 0 && next_j < n &&
                        grid[next_i][next_j] == 0)
                    {
                        grid[next_i][next_j] = 1;
                        q.push(make_pair(next_i, next_j));
                        n_people -= 1;
                    }
                }
            }
        }

        if (n_people != 0)
            return -1;
        return res;
    }
};
```

### 复杂度分析:

o\(n\)


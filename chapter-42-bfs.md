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

[http://www.lintcode.com/en/problem/zombie-in-matrix/\#](http://www.lintcode.com/en/problem/zombie-in-matrix/#)

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

## 531. Six Degrees

Six degrees of separation is the theory that everyone and everything is six or fewer steps away, by way of introduction, from any other person in the world, so that a chain of "a friend of a friend" statements can be made to connect any two people in a maximum of six steps.

Given a friendship relations, find the degrees of two people, return`-1`if they can not been connected by friends of friends.

Have you met this question in a real interview?

Yes

**Example**

Gien a graph:

```
1------2-----4
 \          /
  \        /
   \--3--/
```

`{1,2,3#2,1,4#3,1,4#4,2,3}`and s =`1`, t =`4`return`2`

Gien a graph:

```
1      2-----4
             /
           /
          3
```

`{1#2,4#3,4#4,2,3}`and s =`1`, t =`4`return`-1`

[http://www.lintcode.com/en/problem/six-degrees/\#](http://www.lintcode.com/en/problem/six-degrees/#)

### 解题分析:

图里面的最短路径问题，BFS

### 代码：

```
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */


class Solution {
public:
    /*
     * @param graph: a list of Undirected graph node
     * @param s: Undirected graph node
     * @param t: Undirected graph nodes
     * @return: an integer
     */
    int sixDegrees(vector<UndirectedGraphNode*> graph, UndirectedGraphNode* s, UndirectedGraphNode* t) {
        // write your code here
        if (graph.empty() || !s || !t)
            return -1;
        queue<UndirectedGraphNode*> q;
        int degrees = -1;
        q.push(s);
        unordered_set<int> visited;
        visited.insert(s->label);
        while(!q.empty())
        {
            degrees++;
            int n = q.size();
            for (int i = 0; i < n; i++)
            {
                auto node = q.front();
                q.pop();
                if (node->label == t->label )
                    return degrees;
                for (const auto& neighbor : node->neighbors)
                {
                    if (visited.find(neighbor->label) == visited.end())
                    {
                        visited.insert(neighbor->label);
                        q.push(neighbor);
                    }
                }
            }
        }
        return -1;
    }
};
```

### 复杂度分析:

o\(n\)

## \*\*\*\*178. Graph Valid Tree

Given`n`nodes labeled from`0`to`n - 1`and a list of`undirected`edges \(each edge is a pair of nodes\), write a function to check whether these edges make up a valid tree.

##### Notice

You can assume that no duplicate edges will appear in edges. Since all edges are`undirected`,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in edges.

Have you met this question in a real interview?

Yes

**Example**

Given`n = 5`and`edges = [[0, 1], [0, 2], [0, 3], [1, 4]]`, return true.

Given`n = 5`and`edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]`, return false.

[http://www.lintcode.com/en/problem/graph-valid-tree/\#](http://www.lintcode.com/en/problem/graph-valid-tree/#)

### 解题分析:

#### 思路1:

[https://zh.wikipedia.org/wiki/树\_\(图论\](https://zh.wikipedia.org/wiki/树_%28图论%29\)

如果无向简单图G有有限个顶点（设为n个顶点），那么G是一棵**树**还等价于：

* G是连通的，有n− 1条边，并且G没有简单回路。

那么第一种办法就是判断 edges的数目是不是= n-1

如果不是，那么必然不符合

如果是， 如果有回路，BFS遍历的时候必然不能遍历所有n个点

### 代码：

```
class Solution {
public:
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    bool validTree(int n, vector<vector<int>> &edges) {
        // write your code here
        if (edges.size() != n-1)
            return false;

        // rebuild graph
        unordered_map<int, unordered_set<int>> graph;
        for (const auto& edge : edges)
        {
            graph[edge[0]].insert(edge[1]);
            graph[edge[1]].insert(edge[0]);
        }

        queue<int> q;
        unordered_set<int> visited;
        visited.insert(0);
        q.push(0);
        while(!q.empty())
        {
            auto node = q.front();
            q.pop();
            for (const auto& neighbor : graph[node])
            {
                if (visited.find(neighbor) == visited.end())
                {
                    visited.insert(neighbor);
                    q.push(neighbor);
                }
            }
        }
        return visited.size() == n;
    }
};
```

### 复杂度分析:

o\(n\)

#### 思路2:

思路一种，检查是不是有简单环用了一个逻辑递推： 如果在有 n-1条边和n个节点的前提下，从任意一点开始不能遍历所有点，则不是树。

但是有一个检查图的公共祖先或者环的有一个万能大法： 并查集。 这是个小众数据结构但是在解决BFS闭环问题中很常见。

并查集有两个接口：

Find: 找到x的set root

Union\(x,y\) 把x, y 归一到同一个root.

那么在处理边的时候不停的union nodes, 如果发现边的两端已经属于同一个root，那么则已经联通，这条边为构成环的条件。

两个优化过程： 搜索中压缩和low rank merge to high rank, 这样amortized complexity： O\(1\)

![](/assets/uf1.png)

```
    UnionFind( int n)
    {
        parent_ = vector<int>(n, 0);
        rank_ = vector<int>(n, 0);
        for (int i = 0; i<n; i++)
            parent_[i] = i;
    }
```

![](/assets/uf2.png)

```
    int Find( int v)
    {
        // compression
        if (parent_[v] != v )
            parent_[v] = Find(parent_[v]);
        return parent_[v];
    }
```

![](/assets/uf3.png)

```
    bool Union(int x, int y)
    {
        int rx = Find(x);
        int ry = Find(y);
        if (rx == ry)
            return false;
        int rank_x = rank_[rx];
        int rank_y = rank_[ry];
        if (rank_x < rank_y)
        {
            parent_[rx] = ry;
        }
        else  if (rank_x > rank_y)
        {
            parent_[ry] = rx;
        }
        else
        {
            parent_[rx] = ry;
            rank_[ry]++;
        }
        return true;
    }
```

### 代码：

```
class UnionFind
{
public:
    UnionFind( int n)
    {
        parent_ = vector<int>(n, 0);
        rank_ = vector<int>(n, 0);
        for (int i = 0; i<n; i++)
            parent_[i] = i;
    }

    int Find( int v)
    {
        // compression
        if (parent_[v] != v )
            parent_[v] = Find(parent_[v]);
        return parent_[v];
    }

    bool Union(int x, int y)
    {
        int rx = Find(x);
        int ry = Find(y);
        if (rx == ry)
            return false;
        int rank_x = rank_[rx];
        int rank_y = rank_[ry];
        if (rank_x < rank_y)
        {
            parent_[rx] = ry;
        }
        else  if (rank_x > rank_y)
        {
            parent_[ry] = rx;
        }
        else
        {
            parent_[rx] = ry;
            rank_[ry]++;
        }
        return true;
    }

private:

    vector<int> parent_;
    vector<int> rank_;
};



class Solution {
public:
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    bool validTree(int n, vector<vector<int>> &edges) {
        // write your code here
        if (edges.size() != n-1)
            return false;
        UnionFind uf(n);
        for (const auto& edge : edges)
        {
            if (!uf.Union(edge[0], edge[1]))
                return false;
        }
        return true;
    }
};
```

### 复杂度分析:

o\(n\)

## 431. Connected Component in Undirected Graph

Find the number connected component in the undirected graph. Each node in the graph contains a label and a list of its neighbors. \(a connected component \(or just component\) of an undirected graph is a subgraph in which any two vertices are connected to each other by paths, and which is connected to no additional vertices in the supergraph.\)

##### Notice

Each connected component should sort by label.

Have you met this question in a real interview?

Yes

**Clarification**

[Learn more about representation of graphs](http://www.lintcode.com/help/graph)

**Example**

Given graph:

```
A------B  C
 \     |  | 
  \    |  |
   \   |  |
    \  |  |
      D   E
```

Return`{A,B,D}, {C,E}`. Since there are two connected component which is`{A,B,D}, {C,E}`

[http://www.lintcode.com/en/problem/connected-component-in-undirected-graph/\#](http://www.lintcode.com/en/problem/connected-component-in-undirected-graph/#)

### 解题分析:

每次做法都不一样， 放一个自己觉得confident的方法吧。

### 代码：

```
/**
 * Definition for Undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */


class Solution {
public:
    /*
     * @param nodes: a array of Undirected graph node
     * @return: a connected set of a Undirected graph
     */
    vector<vector<int>> connectedSet(vector<UndirectedGraphNode*> nodes) {
        // write your code here
        vector<vector<int>> res;
        if (nodes.empty())
            return res;
        unordered_set<UndirectedGraphNode*> graph;
        for (auto each: nodes)
            graph.insert(each);
        queue<UndirectedGraphNode*> q;
        unordered_set<UndirectedGraphNode*> visited;
        while(!graph.empty())
        {
            vector<int> loop;
            auto start = *graph.begin();
            q.push(start);
            visited.insert(start);
            while(!q.empty())
            {
                auto node = q.front();
                q.pop();
                graph.erase(node);
                loop.push_back(node->label);
                for (const auto& each : node->neighbors)
                {
                    if (visited.find(each) == visited.end())
                    {
                        visited.insert(each);
                        q.push(each);
                    }
                }
            }
            sort(loop.begin(), loop.end());
            res.push_back(loop);
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)

## 71. Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. \(ie, from left to right, then right to left for the next level and alternate between\).

Have you met this question in a real interview?

Yes

**Example**

Given binary tree `{3,9,20,#,#,15,7}`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```

[http://www.lintcode.com/en/problem/binary-tree-zigzag-level-order-traversal/\#](http://www.lintcode.com/en/problem/binary-tree-zigzag-level-order-traversal/#)

### 解题分析:

无聊类题目，直接写代码了

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
     * @return: A list of lists of integer include the zigzag level order traversal of its nodes' values.
     */
    vector<vector<int>> zigzagLevelOrder(TreeNode * root) {
        // write your code here
        vector<vector<int>> res;
        if (!root)
            return res;
        queue<TreeNode*> q;
        q.push(root);
        bool reverse = false;
        while(!q.empty())
        {
            vector<int> loop;
            int n = q.size();
            for (int i = 0; i<n; i++)
            {
                auto node = q.front();
                q.pop();
                if (reverse)
                    loop.insert(loop.begin(), node->val);
                else
                    loop.push_back(node->val);
                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
            res.push_back(loop);
            reverse = !reverse;
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)



## 70. Binary Tree Level Order Traversal II

Given a binary tree, return the bottom-up level order traversal of its nodes' values. \(ie, from left to right, level by level from leaf to root\).

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



return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```

http://www.lintcode.com/en/problem/binary-tree-level-order-traversal-ii/\#

### 解题分析:

无脑套模板了。

### 代码：

```
http://www.lintcode.com/en/problem/binary-tree-level-order-traversal-ii/#
```

### 复杂度分析:

o\(n\)


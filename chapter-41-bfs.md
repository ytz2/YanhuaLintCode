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

[http://www.lintcode.com/en/problem/number-of-islands/\#](http://www.lintcode.com/en/problem/number-of-islands/#)

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

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

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

return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

[http://www.lintcode.com/en/problem/binary-tree-level-order-traversal/\#](http://www.lintcode.com/en/problem/binary-tree-level-order-traversal/#)

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

## 615. Course Schedule

There are a total of n courses you have to take, labeled from`0`to`n - 1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: \[0,1\]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

Have you met this question in a real interview?

Yes

**Example**

Given n =`2`, prerequisites =`[[1,0]]`  
Return`true`

Given n =`2`, prerequisites =`[[1,0],[0,1]]`  
Return`false`

[http://www.lintcode.com/en/problem/course-schedule/\#](http://www.lintcode.com/en/problem/course-schedule/#)

### 解题分析:

依赖关系， schedule, workflow 都是用的拓扑排序， 并行计算里面的lazy compute也是用的这个方法。

基本套路：

optional: build graph with node based instead of side based

1. build indegrees table 依赖度
2. 遍历所有没有依赖度的放到Q里
3. BFS 并且降低被拿走的依赖度，如果为0继续遍历

这道题有个坑，就是找依赖度为0的时候不要用 graph的node， 而是用n来遍历， 因为有的node就是独立的点，谁也不依赖。

### 代码：

```
class Solution {
public:
    /*
     * @param numCourses: a total of n courses
     * @param prerequisites: a list of prerequisite pairs
     * @return: true if can finish all courses or false
     */
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        // write your code here

        // transform graph representation
        unordered_map<int, unordered_set<int>> graph;
        for (const auto& each : prerequisites)
            graph[each.second].insert(each.first);
        // get indegreees, statistics of neighbor of parents
        unordered_map<int, int> degrees;
        for (const auto& each : graph)
            for (const auto& neighbor : each.second)
                degrees[neighbor]++;
        // topological sort
        vector<int> res;
        queue<int> q;
        // get those without degrees and dump into q
        for (int i = 0; i < numCourses; i++)
            if (degrees.find(i) == degrees.end())
                q.push(i);
        // bfs with a q, search neighbors to deduct their degree, if 0 push to q        
        while(!q.empty())
        {
            auto outq = q.front();
            q.pop();
            res.push_back(outq);
            const auto& neighbors = graph[outq];
            for (const auto& neighbor : neighbors)
            {
                if (degrees.find(neighbor) != degrees.end() && --degrees[neighbor] == 0)
                {
                    degrees.erase(neighbor);
                    q.push(neighbor);
                }
            }
        }
        return res.size() == numCourses; 
    }
};
```

### 复杂度分析:

o\(n\)

## 616. Course Schedule II

There are a total of n courses you have to take, labeled from`0`to`n - 1`.  
Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair:`[0,1]`

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Have you met this question in a real interview?

Yes

**Example**

Given n =`2`, prerequisites =`[[1,0]]`  
Return`[0,1]`

Given n = 4, prerequisites =`[1,0],[2,0],[3,1],[3,2]]`  
Return`[0,1,2,3]`or`[0,2,1,3]`

[http://www.lintcode.com/en/problem/course-schedule-ii/\#](http://www.lintcode.com/en/problem/course-schedule-ii/#)

### 解题分析:

依赖关系， schedule, workflow 都是用的拓扑排序， 并行计算里面的lazy compute也是用的这个方法。

和615是一样的

### 代码：

```
class Solution {
public:
    /*
     * @param numCourses: a total of n courses
     * @param prerequisites: a list of prerequisite pairs
     * @return: the course order
     */
    vector<int> findOrder(int numCourses, vector<pair<int, int>> &prerequisites) {
        // write your code here
        // build graph
        unordered_map<int, unordered_set<int>> graph;
        for (const auto& required : prerequisites)
            graph[required.second].insert(required.first);
        // build indegrees
        unordered_map<int, int> indegrees;
        for (const auto& node : graph)
            for (const auto& neighbor : node.second)
                indegrees[neighbor]++;
        // prepare q;
        queue<int> q;
        for (int i = 0; i < numCourses; i++)
            if (indegrees.find(i) == indegrees.end())
                q.push(i);
        vector<int> res;
        while(!q.empty())
        {
            auto v = q.front();
            q.pop();
            res.push_back(v);
            if (graph.find(v) != graph.end())
            {
                for (const auto& neighbor : graph[v])
                {
                    if (indegrees.find(neighbor) != indegrees.end() && --indegrees[neighbor] == 0)
                    {
                        indegrees.erase(neighbor);
                        q.push(neighbor);
                    }
                }
            }
        }
        if (res.size() != numCourses)
            return vector<int>();
        return res;
    }           

};
```

### 复杂度分析:

o\(n\)



## 611. Knight Shortest Path

Given a knight in a chessboard \(a binary matrix with`0`as empty and`1`as barrier\) with a`source`position, find the shortest path to a`destination`position, return the length of the route.  
Return`-1`if knight can not reached.

##### Notice

source and destination must be empty.  
Knight can not enter the barrier.

Have you met this question in a real interview?

Yes

**Clarification**

If the knight is at \(_x_,_y_\), he can get to the following positions in one step:

```
(x + 1, y + 2)
(x + 1, y - 2)
(x - 1, y + 2)
(x - 1, y - 2)
(x + 2, y + 1)
(x + 2, y - 1)
(x - 2, y + 1)
(x - 2, y - 1)

```

**Example**

```
[[0,0,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return 2

[[0,1,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return 6

[[0,1,0],
 [0,0,1],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return -1
```

http://www.lintcode.com/en/problem/knight-shortest-path/\#

### 解题分析:

由点及面最短路径问题。

### 代码：

```
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */

class Solution {
public:
    /**
     * @param grid: a chessboard included 0 (false) and 1 (true)
     * @param source: a point
     * @param destination: a point
     * @return: the shortest path 
     */
    int shortestPath(vector<vector<bool>> &grid, Point &source, Point &destination) {
        // write your code here
        int m = grid.size();
        if ( m == 0)
            return -1;
        int n = grid[0].size();
        if (n == 0)
            return -1;
        if (grid[source.x][source.y] == 1)
            return -1;
        queue<Point> q;
        q.push(source);
        grid[source.x][source.y] = 1; 
        int res = -1;
        vector<int> dx{ 1, 1, -1, -1, 2, 2, -2, -2};
        vector<int> dy{ 2,-2,  2, -2, 1,-1,  1, -1};
        while(!q.empty())
        {
            res++;
            int size = q.size();
            for (int i = 0; i < size; i++)
            {
                auto p = q.front();
                q.pop();
                if (p.x == destination.x && p.y == destination.y)
                    return res;
                for (int k = 0; k < dx.size(); k++)
                {
                    auto next_x = p.x + dx[k];
                    auto next_y = p.y + dy[k];
                    if (next_x >= 0 && next_x < m && next_y >= 0 && next_y < n && grid[next_x][next_y] == 0)
                    {
                        q.emplace(next_x, next_y);
                        grid[next_x][next_y] = 1;
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


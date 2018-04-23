## 574. Build Post Office

Given a 2D grid, each cell is either an house`1`or empty`0`\(the number zero, one\), find the place to build a post office, the distance that post office to all the house sum is smallest. Return the smallest distance. Return`-1`if it is not possible.

##### Notice

* You can pass through house and empty.
* You only build post office on an empty.

Have you met this question in a real interview?

Yes

**Example**

Given a grid:

```
0 1 0 0
1 0 1 1
0 1 0 0
```

return`6`. \(Placing a post office at \(1,1\), the distance that post office to all the house sum is smallest.\)

[https://www.lintcode.com/en/problem/build-post-office/\#](https://www.lintcode.com/en/problem/build-post-office/#)

### 解题分析:

这是一道数学上的二维映射一维的问题， 因为x,y轴是独立自由度，把1分别投影到x, y轴上， 在x轴和y轴分别找最近的就是最后的答案。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param grid: a 2D grid
     * @return: An integer
     */
    int shortestDistance(vector<vector<int>> &grid) {
        // write your code here
        int m = grid.size();
        if (m == 0)
            return -1;
        int n = grid[0].size();
        if (n == 0)
            return -1;
        vector<int> xaxis(m, 0);
        for (int i = 0; i<m; i++)
            for (int j = 0; j < n; j++)
                xaxis[i] += grid[i][j];
        vector<int> yaxis(n, 0);
        for (int j = 0; j < n; j++)
            for (int i = 0; i < m; i++)
                yaxis[j]+= grid[i][j];
        int res = INT_MAX;

        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (grid[i][j] != 0)
                    continue;
                int val = 0;
                for (int k = 0; k < m; k++)
                    val += abs(i-k)*xaxis[k];
                for (int k = 0; k < n; k++)
                    val += abs(j-k)*yaxis[k];
                res = min(val, res);
            }
        }
        return res == INT_MAX? -1 : res;
    }
};
```

### 复杂度分析:

o\(\(m+n\)\*mn\) better than o\(m^2n^2\)

## \*\*\*\*573. Build Post Office II

Given a 2D grid, each cell is either a wall`2`, an house`1`or empty`0`\(the number zero, one, two\), find a place to build a post office so that the sum of the distance from the post office to all the houses is smallest.

Return the smallest sum of distance. Return`-1`if it is not possible.

##### Notice

* You cannot pass through wall and house, but can pass through empty.
* You only build post office on an empty.

Have you met this question in a real interview?

Yes

**Example**

Given a grid:

```
0 1 0 0 0
1 0 0 2 1
0 1 0 0 0
```

return`8`, You can build at`(1,1)`. \(Placing a post office at \(1,1\), the distance that post office to all the house sum is smallest.\)

[http://www.lintcode.com/en/problem/build-post-office-ii/\#](http://www.lintcode.com/en/problem/build-post-office-ii/#)

### 解题分析:

如何做到思路清晰和注意corner case是这个题的要点。

还是分步走：

第一步找出所有可能的candidates

第二步数出所有的house

第三步 对每一个candidate 看能否BFS遍历所有的house且记录distance

针对第三部单独写一个函数调用，这样条理清晰一些。

Note：

1. 如果访问过了，则mark 为2， 不需要单独hash
2. 犯了一个超级傻逼的错误，debug了半天， n = q.size\(\) 把 y维度变量冲掉了。。。。已经好几次了。。。下次起名上点心。。。。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param grid: a 2D grid
     * @return: An integer
     */
    int shortestDistance(vector<vector<int>> &grid) {
        // write your code here
        int m = grid.size();
        if (m == 0)
            return -1;
        int n = grid[0].size();
        if (n == 0)
            return -1;
        int count = 0;
        vector<Point> candidates;
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (grid[i][j] == 0)
                    candidates.emplace_back(i,j);
                else if (grid[i][j] == 1)
                    count++;
            }
        }
        if (candidates.empty() || count == 0)
            return -1;

        int res = INT_MAX;
        for (const auto& each : candidates)
        {
            int distance = 0;
            if (BFS(grid, each, count, distance))
            {
                res = min(res, distance);
            }
        }
        return res == INT_MAX? -1 : res;
    }

    bool BFS(vector<vector<int>> grid, Point start, int count, int& distance)
    {
        int m = grid.size();
        int n = grid[0].size();
        cout <<m<<","<<n<<endl;
        vector<int> dx{ 0 , 0, 1 , -1};
        vector<int> dy{-1 , 1, 0 ,  0};
        queue<Point> q;
        q.push(start);
        grid[start.x][start.y] = 2;
        int step = -1;
        while(!q.empty())
        {
            step++;
            int size = q.size();
            for (int i=0; i<size; i++)
            {
                auto point = q.front();
                q.pop();
                for (int j = 0; j < 4; j++)
                {
                    int next_x = point.x + dx[j];
                    int next_y = point.y + dy[j];
                    if (next_x < 0 || next_x >= m || next_y < 0 || next_y >=n)
                        continue;

                    int v = grid[next_x][next_y];
                    if (v == 2)
                        continue;
                    else if (v == 0)
                    {
                        grid[next_x][next_y] = 2;
                        q.push(Point(next_x, next_y));
                    }
                    else
                    {
                        count--;
                        grid[next_x][next_y] = 2;
                        distance += step + 1;
                    }
                }
            }
        }
        if (count > 0)
        {
            distance = INT_MAX;
            return false;
        }
        return true;
    }
};
```

### 复杂度分析:

O（kn\)

## 600. Smallest Rectangle Enclosing Black Pixels

An image is represented by a binary matrix with`0`as a white pixel and`1`as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location`(x, y)`of one of the black pixels, return the area of the smallest \(axis-aligned\) rectangle that encloses all black pixels.

Have you met this question in a real interview?

Yes

**Example**

For example, given the following image:

```
[
  "0010",
  "0110",
  "0100"
]
```

and x =`0`, y =`2`,  
Return`6`.

[https://www.lintcode.com/en/problem/smallest-rectangle-enclosing-black-pixels/\#](https://www.lintcode.com/en/problem/smallest-rectangle-enclosing-black-pixels/#)

### 解题分析:

一边BFS， 一边找最左边最右边最上边最下边

### 代码：

```cpp
class Solution {
public:
    /**
     * @param image: a binary matrix with '0' and '1'
     * @param x: the location of one of the black pixels
     * @param y: the location of one of the black pixels
     * @return: an integer
     */
    int minArea(vector<vector<char>> &image, int x, int y) {
        // write your code here
        int top = INT_MIN, down = INT_MAX, left = INT_MAX, right = INT_MIN;
        int m = image.size();
        if (m == 0)
            return 0;
        int n = image[0].size();
        if (n == 0)
            return 0;
        if (x < 0 || x >=m || y < 0 || y>= n || image[x][y] == '0')
            return 0;

        queue<Point> q;
        q.push(Point(x,y));
        image[x][y] = '0';
        vector<int> dx{ 0, 0 , -1, 1};
        vector<int> dy{-1, 1,  0,  0};
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            top = max( p.x, top);
            down = min(p.x, down);
            left = min(p.y, left);
            right = max(p.y, right);
            for (int i = 0; i < 4; i++)
            {
                int next_x = p.x + dx[i];
                int next_y = p.y + dy[i];
                if (next_x < 0 || next_x >= m || next_y < 0 || next_y>=n || image[next_x][next_y] == '0')
                    continue;
                q.push(Point(next_x, next_y));
                image[next_x][next_y] = '0';
            }
        }
        return (top-down+1)*(right-left+1);
    }
};
```

### 复杂度分析:

O（kn\)



## \*\*\*\*434. Number of Islands II

Given a n,m which means the row and column of the 2D matrix and an array of pair A\( size k\). Originally, the 2D matrix is all 0 which means there is only sea in the matrix. The list pair has k operator and each operator has two integer A\[i\].x, A\[i\].y means that you can change the grid matrix\[A\[i\].x\]\[A\[i\].y\] from sea to island. Return how many island are there in the matrix after each operator.

##### Notice

0 is represented as the sea, 1 is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.

Have you met this question in a real interview?

Yes

**Example**

Given`n`=`3`,`m`=`3`, array of pair A =`[(0,0),(0,1),(2,2),(2,1)]`.

return`[1,1,2,2]`.

https://www.lintcode.com/en/problem/number-of-islands-ii/\#

### 解题分析:

联通不连通， UNION FIND大法好。。。

有下面逻辑问题需要解决:

1. UnionFind大法对于一维比较方便，所以需要把island 变化到一维去。

   2. 如何更新岛： 

      a. 如果有一个新的进来， cache i, j 为岛，且增加技术

      b. 遍历四周，如果周围有岛， 每次如果他们没有共同的root via find ,那么因为这次插入，他们联通了 via union， 且count要减一

  3. corner case : 会有重复的op出现，所以加一个cache, 过滤掉一些操作，同时要保持count update.  

### 代码：

```cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
 
class UnionFind{
public:
    UnionFind(int n)
    {
        parent_ = vector<int>(n, 0);
        rank_ = vector<int>(n, 0);
        for (int i = 0; i < n; i++)
            parent_[i] = i;
    }
    
    int Find(int x)
    {
        if (parent_[x] != x)
            parent_[x] = Find(parent_[x]);
        return parent_[x];
    }
    
    bool Union(int x, int y)
    {
        int rx = Find(x);
        int ry = Find(y);
        if (rx == ry)
            return false;
        int rankx = rank_[rx];
        int ranky = rank_[ry];
        if (rankx < ranky)
        {
            parent_[rx] = ry;
        }
        else if (rankx > ranky)
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
     * @param m: An integer
     * @param operators: an array of point
     * @return: an integer array
     */
    vector<int> numIslands2(int n, int m, vector<Point> &operators) {
        // write your code here
        UnionFind uf(n*m);
        vector<int> res;
        vector<vector<bool>> islands(n, vector<bool>(m, false));
        unordered_set<int> done;
        int count = 0;
        vector<int> dx{0, 0 , 1, -1};
        vector<int> dy{-1,1,  0, 0};
        for (const auto& op : operators)
        {
            int ind = op.x*m+op.y;
            if (done.find(ind) != done.end())
            {
                res.push_back(count);
                continue;
            }
            islands[op.x][op.y] = true;
            done.insert(ind);
            count++;
            for(int i = 0; i<4; i++)
            {
                int next_x = op.x+dx[i];
                int next_y = op.y+dy[i];
                if (next_x < 0 || next_x >= n || next_y < 0 || next_y >=m || !islands[next_x][next_y])
                    continue;
                int next_ind = next_x*m+next_y;
                if (uf.Find(next_ind) != uf.Find(ind))
                {
                    count--;
                    uf.Union(next_ind, ind);
                }
            }
            res.push_back(count);
        }
        return res;
    }
};
```

### 复杂度分析:

O（kn\)


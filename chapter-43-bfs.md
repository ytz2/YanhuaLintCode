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

```
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

http://www.lintcode.com/en/problem/build-post-office-ii/\#

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




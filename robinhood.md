## 54 Spiral Matrix

Given an`m x nmatrix`, return_all elements of the_`matrix`_in spiral order_.

[https://leetcode.com/problems/spiral-matrix/](https://leetcode.com/problems/spiral-matrix/)[        
](https://leetcode.com/problems/spiral-matrix/)

![](/assets/import54.png)

感觉还是传统套路， 碰到二维矩阵想到的就是两点定标， \(x0, y0\), \(x1, y1\) ， 做这道题的时候因为是做的robinhood下面的tag所用模拟法都麻木了，两点定标+模拟

横着向右走，x0++, 竖着向下走，y1--, 横着想左走,x1--,竖着向上走，y0++, 每次移动点的时候注意check是不是越界，最后终止条件就是收缩到x0&lt;=x1 && y0&lt;=y1

```
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        if (matrix.empty() || matrix[0].empty())
            return result;
        int x0 = 0, y0 = 0, x1 = matrix.size() - 1, y1 = matrix[0].size() - 1;
        while(x0 <= x1 && y0 <= y1) {
            for (int i = y0; i <= y1; i++) {
                result.push_back(matrix[x0][i]);
            }
            if (++x0 > x1) break;
            for (int i = x0; i <= x1; i++) {
                result.push_back(matrix[i][y1]);
            }
            if (--y1 < y0) break;
            for (int i = y1; i>= y0; i--) {
                result.push_back(matrix[x1][i]);
            }
            if (--x1 < x0) break;
            for (int i = x1; i >= x0; i--) {
                result.push_back(matrix[i][y0]);
            }
            if (++y0 > y1) break;
        }
        return result;
    }
};
```

## 200 Number of Islands

Given an`m x n`2d`grid`map of`'1'`s \(land\) and`'0'`s \(water\), return_the number of islands_.

An**island**is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

万恶的数小岛， 10个公司8个考，关键是面的时候难道不知道这题大家实在是不想再做了么？

BFS, 碰到1就++, 然后设置为BFS起始点开始把相连的设为0， O\(N\)

```
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty() || grid[0].empty())
            return 0;
        int m = grid.size(), n = grid[0].size();
        queue<pair<int /*x*/, int /*y*/>> q;
        static vector<int> dx{-1, 1, 0, 0};
        static vector<int> dy{0, 0, -1, 1};
        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '0')
                    continue;
                count++;
                q.emplace(i, j);
                grid[i][j] = '0';
                while(!q.empty()) {
                    auto pos = q.front();
                    q.pop();
                    for (int k = 0; k < 4; k++) {
                        auto nx = pos.first + dx[k];
                        auto ny = pos.second + dy[k];
                        if (nx < 0 || nx >= m || ny < 0 || ny >= n || grid[nx][ny] == '0')
                            continue;
                        grid[nx][ny] = '0';
                        q.emplace(nx, ny);
                    }
                }
            }
        }
        return count;
    }
};
```

## 224 Basic Calculator

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open`(`and closing parentheses`)`, the plus`+`or minus sign`-`,**non-negative**integers and empty spaces.

这道题我刷的时候吭哧了将近半个小时才搞对， 那就意味着这题属于背诵类题目了，

流程如下：

遍历每个字母：

1. 空格跳过

   2 数字累计

   3  （  进操作stack， 注意，左括号是没有数字的

   4 + - ） 把数字进数字stack, 同时 + - 进操作stack , 数字归零 + - 继续

   5 单独处理）  一直往上找到（  进行计算， 注意因为 -的原因，不能用逆向计算， 碰到- 把当前数字变成负数，最后求和

   6。 注意由 ） 生成的数字不要上数字stack 因为）+（的情况，或者后面自然会去

   7 循环结束，把数字放上去，因为stack上不会再有括号，直接再call一遍eval就好

```
class Solution {
public:


    int calculate(string s) {
        stack<long> numbers;
        stack<char> ops;
        long num = 0;

        for(auto c : s) {
            if (c == ' ')
                continue;
            if (c >= '0' && c <= '9') {
                num = num * 10 + c -'0';
                continue;
            }
            if (c == '(') {
                ops.push(c);
                continue;
            }
            if (c == ')' || c == '+' || c=='-') {
                numbers.push(num);
                num = 0;
            }
            if (c == '+' || c == '-') {
                ops.push(c);
                continue;
            }
            if (c == ')') 
                num = eval(numbers, ops);
        }
        numbers.push(num);
        return eval(numbers, ops);
    }


    long eval(stack<long>& nums, stack<char>& operations) {
        long val = 0;
        while(!operations.empty() && operations.top() != '(') {
            auto c= operations.top();
            auto v = nums.top();
            operations.pop();
            nums.pop();
            if (c == '-')
                v = -v;
            val += v;
        }
        if (!operations.empty() && operations.top() == '(') {
            operations.pop();
        }
        val += nums.top();
        nums.pop();
        return val;
    }
};
```

## 786 K-th Smallest Prime Fraction

A sorted list`A`contains 1, plus some number of primes.  Then, for every p &lt; q in the list, we consider the fraction p/q.

What is the`K`-th smallest fraction considered?  Return your answer as an array of ints, where`answer[0] = p`and`answer[1] = q`.

```
Examples:
Input:
 A = [1, 2, 3, 5], K = 3

Output:
 [2, 5]

Explanation:

The fractions to be considered in sorted order are:
1/5, 1/3, 2/5, 1/2, 3/5, 2/3.
The third fraction is 2/5.


Input:
 A = [1, 7], K = 1

Output:
 [1, 7]
```

这道题有两种做法， 第一种方法好想但是超时了， 如果你把1-N 作为为分母 \[j, i\) 作为分子 那么世界上就是k路归并

这种解法\(K+N\)logN 的复杂度， K太大的时候超时了

```cpp
class Solution {
public:
    vector<int> kthSmallestPrimeFraction(vector<int>& A, int K) {
        // kway merge, think about elemtn 1->n are n -1 vectors, meaning, vectors using them as denom
        // so in each vector, they are sorted, then we can perform kway merge, straight forward
        auto cmp = [&A](const pair<int, int>& l, const pair<int, int>& r) {
            return A[l.first] * A[r.second] > A[r.first] * A[l.second];   
        };
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
        for (int j = 1; j < A.size(); j++) {
            pq.emplace(0, j);
        }

        while(--K) {
            auto node = pq.top();
            pq.pop();
            node.first++;
            if (node.first < node.second)
                pq.push(node);
        }
        return {A[pq.top().first], A[pq.top().second]};
    }
}
```

第二种解法用的是二分答案， 套路是必然存在一个数使的这一堆组合里有K个数字比他小 Nlog\(1e9\) 可以理解为常数 ， 我个人觉得不好理解为什么是N， 首先二分的log\(N\)没有争议， 对于N就很有意思了， 因为是二分，intuitive N, N/2, N/4 最终是2N

```cpp
class Solution {
public:
    struct CountRes {
        int size = 0;
        int nom = 0;
        int denom = 0;
    };
    vector<int> kthSmallestPrimeFraction(vector<int>& A, int K) {
        // there is away to use binary search answer 
        // 0, 1 as low and high then count how many below low/high to get ther answer

        double beg = 0, end = 1;
        while(beg + 1e-9 < end)
        {
            double mid = double(beg + end) / 2.;
            auto res = count(A, mid);
            if (res.size < K )
                beg = mid;
            else
                end = mid;
        }        
        auto res = count(A, beg);
        if (res.size == K)
            return {res.nom, res.denom};
        res = count(A, end);
        return {res.nom, res.denom};
    }

    CountRes count(vector<int>&A, double frac) {
        CountRes res;
        double maxFrac = 0;
        for (int j = 1; j < A.size(); j++) {
            int i = 0;
            while( i < j ){
                double cur = double(A[i])/ double(A[j]);
                if (cur > frac)
                    break;
                if (cur > maxFrac) {
                    maxFrac = cur;
                    res.nom = A[i];
                    res.denom = A[j];
                }
                i++;
                res.size++;
            }
        }
        return res;
    }
};
```

## 498 Diagonal Traverse

Given a matrix of M x N elements \(M rows, N columns\), return all elements of the matrix in diagonal order as shown in the below image..

![](/assets/import498.png)

第一种是沿着上和右边缘描线， 一遍描线一边翻转

```
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        if (matrix.empty() || matrix[0].empty())
            return result;
        auto add = [&matrix, &result](int i, int j, bool& flip) {
            int n = result.size();
            while(i < matrix.size() && j >=0)
                result.push_back(matrix[i++][j--]);
            if (flip)
               reverse(result.begin() + n, result.end());
            flip = !flip;
        };

        bool flip = true;
        for(int j = 0; j < matrix[0].size(); j++) {
            add(0, j, flip);
        }
        for (int i = 1; i < matrix.size(); i++) {
            add(i, matrix[0].size() -1, flip);          
        }

        return result;
    }
```

还有一种就是完全模拟法 ，不碰壁的时候按照右上 i--, j++和左下 i++, j--得走法

比较有趣的是右上角和左下角，右上角是往下， 同理碰到右边也是，左下角是往右，同理碰到下边也是

那么处理边界， 碰到右边往下，碰到上边往右 （右上） ，  碰到下边往右， 碰到左边往下 （左下）

```
class Solution {
public:

    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        if (matrix.empty() || matrix[0].empty()) 
            return result;
        int i = 0, j = 0, m = matrix.size(), n = matrix[0].size();
        bool isUp = true;
        while(i != m - 1 || j != n-1) {
            result.push_back(matrix[i][j]);
            if (isUp) {
                if (j == n -1) {
                    i++;
                    isUp = false;
                } else if (i == 0 ) {
                    j++;
                    isUp = false;
                } else {
                    i--;
                    j++;
                }
            }
            else {
                if (i == m -1) {
                    j++; 
                    isUp = true;
                } else if (j == 0) {
                    i++;
                    isUp = true;
                } else {
                    i++;
                    j--;
                }
            }
        }

        result.push_back(matrix[m-1][n-1]);
        return result;
    }
};
```

## 48 Rotate Image

You are given an_n_x_n_2D`matrix`representing an image, rotate the image by 90 degrees \(clockwise\).

You have to rotate the image[**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly.**DO NOT**allocate another 2D matrix and do the rotation.

![](/assets/import48.png)

robinhood和矩阵干上了，这道题最简单的方法叫做模拟法， 一圈一圈的砖，转到没圈， 其最终任意一圈中

i0, j0 -&gt; i1, j1 选取 最上面，i 计算offset,就可以把这个元素对应的其他三个位置算出来， hardcode遍历一条边就可以旋转一层



```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int x0 = 0, x1 = matrix.size() - 1, y0 = 0, y1=matrix[0].size()-1;
        while(x0 < x1) {
            for (int i = y0; i < y1; i++) {
                int dist = i - y0;                
                int a = matrix[x0][i];
                int b = matrix[x0 + dist][y1];
                int c = matrix[x1][y1-dist];
                int d = matrix[x1 - dist][y0];
                matrix[x0 + dist][y1] = a;
                matrix[x1][y1-dist] = b;
                matrix[x1 - dist][y0] = c;
                matrix[x0][i] = d;
            }
            x0++;x1--;y0++;y1--;
        }
    }
};
```

当然，这个题的数学解按照轴对称然后按照中轴这贴， 这个就太数学了。


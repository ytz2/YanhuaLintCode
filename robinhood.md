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




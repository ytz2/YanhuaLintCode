## 839. Merge Two Sorted Interval Lists

Merge two sorted \(ascending\) lists of interval and return it as a new sorted list. The new sorted list should be made by splicing together the intervals of the two lists and sorted in ascending order.

### Example

Given list1 =`[(1,2),(3,4)]`and list2 =`[(2,3),(5,6)]`, return`[(1,4),(5,6)]`.

[https://www.lintcode.com/problem/merge-two-sorted-interval-lists/description](https://www.lintcode.com/problem/merge-two-sorted-interval-lists/description)

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
/**
 * Definition of Interval:
 * classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param list1: one of the given list
     * @param list2: another list
     * @return: the new sorted list of interval
     */
    vector<Interval> mergeTwoInterval(vector<Interval> &list1, vector<Interval> &list2) {
        // write your code here
        vector<Interval> res;
        if (list1.empty() && list2.empty())
            return res;
        if (list2.size() < list1.size())
            swap(list1, list2);
        auto sortFunc = [](const Interval& l, const Interval& r){
            return l.start < r.start;
        };
        for (auto& elem : list2)
            list1.push_back(elem);
        sort(list1.begin(), list1.end(), sortFunc);

        auto prev = Interval(list1[0].start, list1[0].start);
        for (int i = 0; i < list1.size(); i++)
        {
            auto& node = list1[i];
            if (prev.end >= node.start)
            {
                if (node.end > prev.end)
                    prev.end = node.end;
            }
            else
            {
                res.push_back(prev);
                prev = node;
            }
        }
        res.push_back(prev);
        return res;
    }
};
```

## 547. Intersection of Two Arrays

Given two arrays, write a function to compute their intersection.

### Example

Given_nums1_=`[1, 2, 2, 1]`,_nums2_=`[2, 2]`, return`[2]`.

### 

[https://www.lintcode.com/problem/intersection-of-two-arrays/description](https://www.lintcode.com/problem/intersection-of-two-arrays/description)

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
class Solution {
public:

    /*
     * @param nums1: an integer array
     * @param nums2: an integer array
     * @return: an integer array
     */
    vector<int> intersection(vector<int> nums1, vector<int> nums2) {
        // write your code here
        vector<int> res;
        unordered_map<int, int> counter;
        if (nums2.size() > nums1.size())
            swap(nums1,nums2);
        for (auto& each : nums1)
            counter[each] = 1;
        for (auto& each : nums2)
        {
            if (!counter.count(each) )
                continue;
            if (counter[each] == 1)
            {
                counter[each]++;
                res.push_back(each);
            }
        }
        return res;
    }
};
```

## 944. Maximum Submatrix

Given an`n x n`matrix of`positive`and`negative`integers, find the submatrix with the largest possible sum.

### Example

```
Given matrix = 
[
[1,3,-1],
[2,3,-2],
[-1,-2,-3]
]
return 9.
Explanation:
the submatrix with the largest possible sum is:
[
[1,2],
[2,3]
]
```

[https://www.lintcode.com/problem/maximum-submatrix/description](https://www.lintcode.com/problem/maximum-submatrix/description)

### 解题分析:

最开始想复杂了，总想着把submatrix 求一个submatrix presum 来做DP，实际上不对，浪费类时间。。。。

其实就是 按照列压缩成一维数组， 然后在一维数组内求最大最小， 压缩的范围为列的 i, j， 代表从从i到j行上的和，遍历所有i,j的组合，最后就可以了。

kadene 算法求得的是单行最大，然后按照\(i,j\)打一个擂台就好了

o\(n^2\*m\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param matrix: the given matrix
     * @return: the largest possible sum
     */
    int maxSubmatrix(vector<vector<int>> &matrix) {
        // write your code here
        int m = matrix.size();
        if (!m)
            return 0;
        int n = matrix[0].size();
        if (!n)
            return 0;
        vector<int> buffer(m, 0);
        int maxVal = INT_MIN;
        auto eval = [](vector<int>& buffer){
          int res = INT_MIN;
          int sum = 0; 
          for (auto& each : buffer)
          {
              sum += each; 
              res = max(sum, res);
              if (sum < 0)
                sum = 0;
          }
          return res;
        };
        for (int j = 0; j < n; j++)
        {
            for (int k = j; k < n; k++)
            {
                for (int i = 0; i < m; i++)
                {
                    buffer[i] += matrix[i][k];
                }
                maxVal = max(maxVal, eval(buffer));
            }
            fill(buffer.begin(), buffer.end(), 0);
        }
        return maxVal;
    }
};
```



## 654. Maximum SubmatrixSparse Matrix Multiplication

Given two[Sparse Matrix](https://en.wikipedia.org/wiki/Sparse_matrix)A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

### Example

```
A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```

https://www.lintcode.com/problem/sparse-matrix-multiplication/description

### 解题分析:

被他名字吓住了，真跑去build row-col-v的稀疏矩阵，其实看了下别人的就是乘了一下。。。真是无聊。。。矩阵的乘法

### 代码：

```cpp



class Solution {
public:
    /**
     * @param A: a sparse matrix
     * @param B: a sparse matrix
     * @return: the result of A * B
     */
    vector<vector<int>> multiply(vector<vector<int>> &A, vector<vector<int>> &B) {
        // write your code here
        if (A.empty() || B.empty())
        {
            return vector<vector<int>>();
        }
        
        int m = A.size();
        int n = B[0].size();
        vector<vector<int>> res(m, vector<int>(n, 0));
        
        // build A 

        vector<vector<pair<int,int> >> sparseA(m,vector<pair<int,int>>());
        for (int i = 0; i < A.size(); i++)
        {
            for (int j = 0; j <A[0].size(); j++ )
            {
                if (A[i][j])
                {
                    sparseA[i].push_back(make_pair(j, A[i][j]));
                }
            }
        }
        
        for (int i = 0; i < m; i++)
        {
            if (sparseA.empty())
                continue;
            for (auto& each : sparseA[i])
            {
                int j = each.first;
                int v = each.second;
                for (int k = 0; k <n; k++)
                {
                    res[i][k] += B[j][k]*v;
                }
            }
        }
        return res;
        
    }
};
```




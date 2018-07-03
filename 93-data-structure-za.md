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

[https://www.lintcode.com/problem/sparse-matrix-multiplication/description](https://www.lintcode.com/problem/sparse-matrix-multiplication/description)

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

## 65. Median of two Sorted Arrays

There are two sorted arrays\_A\_and\_B\_of size\_m\_and\_n\_respectively. Find the**median**of the two sorted arrays.

### Example

Given`A=[1,2,3,4,5,6]`and`B=[2,3,4,5]`, the median is`3.5`.

Given`A=[1,2,3]`and`B=[4,5]`, the median is`3`.

### Challenge

The overall run time complexity should be`O(log (m+n))`.

[https://www.lintcode.com/problem/median-of-two-sorted-arrays/description](https://www.lintcode.com/problem/median-of-two-sorted-arrays/description)

### 解题分析:

算是套路题：

1. 每次去掉k/2， 比较两个数组的第k/2个元素，去掉小的左半部分
2. 退出条件： 有一个为空了，则却另外一个第k个

   ```
                       k=1, 取 两个 数组第一个元素最小

                      否认则服从1
   ```

   1. 有一个边界条件需要处理： 越界了怎么办，一个数组特别小， 不管他，因为他不足k/2， 而另外一个由k/2，直接去掉长的，因为肯定够不在那里面

      int a\_half = abeg + k/2-1 &gt; aend ? INT\_MAX : A\[abeg + k/2 - 1\];

      int b\_half = bbeg + k/2-1 &gt; bend ? INT\_MAX : B\[bbeg + k/2 - 1\];

### 代码：

```cpp
class Solution {
public:
    /*
     * @param A: An integer array
     * @param B: An integer array
     * @return: a double whose format is *.5 or *.0
     */
    double findMedianSortedArrays(vector<int> &A, vector<int> &B) {
        // write your code here
        int n = A.size()+B.size();
        if (n == 0)
            return 0;
        if (n == 1)
        {
            return A.empty()? B[0] : A[0];
        }
        if (n%2)
            return findK(A, 0, A.size()-1, B, 0, B.size()-1, n/2+1);
        return double(findK(A, 0, A.size()-1, B, 0, B.size()-1, n/2) 
                + findK(A, 0, A.size()-1, B, 0, B.size()-1, n/2+1) ) / 2.;
    }

    int findK(vector<int> &A, int abeg, int aend, vector<int> &B, int bbeg, int bend, int k)
    {

        if (bbeg > bend)
            return A[abeg + k-1];
        if (abeg > aend)
            return B[bbeg + k -1];
        if (k == 1)
            return min(A[abeg], B[bbeg]);

        int a_half = abeg + k/2-1 > aend ? INT_MAX : A[abeg + k/2 - 1];
        int b_half = bbeg + k/2-1 > bend ? INT_MAX : B[bbeg + k/2 - 1];

        if (a_half < b_half)
            return findK(A, abeg + k/2, aend, B, bbeg, bend, k-k/2);
        else
            return findK(A, abeg, aend, B, bbeg + k/2, bend, k-k/2);
    }
};
```

## 577. Merge K Sorted Interval Lists

Merge\_K\_sorted interval lists into one sorted interval list. You need to merge overlapping intervals too.

### Example

Given

```
[
  [(1,3),(4,7),(6,8)],
  [(1,2),(9,10)]
]
```

Return

```
[(1,3),(4,8),(9,10)]
```

[https://www.lintcode.com/problem/merge-k-sorted-interval-lists/description](https://www.lintcode.com/problem/merge-k-sorted-interval-lists/description)

### 解题分析:

了解lambda在pq里的用法， 其他都是套路了

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
     * @param intervals: the given k sorted interval lists
     * @return:  the new sorted interval list
     */
    vector<Interval> mergeKSortedIntervalLists(vector<vector<Interval>> &intervals) {
        // write your code here


        typedef pair<Interval, int> IntType;

        auto sortFunc = [](const IntType& left, const IntType& right){
            return left.first.start > right.first.start;
        };

        vector<int> indices(intervals.size(), 0);
        priority_queue<IntType, vector<IntType>, decltype(sortFunc)> pq(sortFunc);
        for (int i = 0; i < intervals.size(); i++)
        {
            if (intervals[i].empty())
                continue;
            auto& interv = intervals[i].front();
            pq.push(make_pair(interv, i));
        }

        vector<Interval> res;
        if (pq.empty())
            return res;
        auto& t = pq.top().first;
        Interval prev(t.start, t.start);
        while(!pq.empty())
        {
            auto node = pq.top();
            pq.pop();
            int ind = node.second;
            auto& intervalNode = node.first;

            if (intervalNode.start > prev.end)
            {
                res.push_back(prev);
                prev = intervalNode;
            }
            else if (intervalNode.end > prev.end)
            {
                prev.end = intervalNode.end;
            }
            int next = ++indices[ind];
            if (next < intervals[ind].size())
            {
                auto& nextInterval = intervals[ind][next];
                pq.push(make_pair(intervals[ind][next], ind));
            }
        }
        res.push_back(prev);
        return res;
    }
};
```

## 840. Range Sum Query - Mutable

Given an integer array nums, find the sum of the elements between indices i and j \(i ≤ j\), inclusive.

The update\(i, val\) function modifies nums by updating the element at index i to val.

### Example

```
Given nums = [1, 3, 5]

sumRange(0, 2) -
>
 9
update(1, 2)
sumRange(0, 2) -
>
 8
```

[https://www.lintcode.com/problem/range-sum-query-mutable/description](https://www.lintcode.com/problem/range-sum-query-mutable/description)

### 解题分析:

SegmentTree

### 代码：

```cpp
class SegNode{
public:
    SegNode(int l,int r, int v)
        : start(l), end(r),value(v), left(nullptr), right(nullptr)
    {}
    ~SegNode()
    {
        if (left)
            delete left;
        if (right)
            delete right;
    }
    int start, end, value;
    SegNode *left, *right;
};


class NumArray {
public:
    NumArray(vector<int> nums) {
        root_ = build(0, nums.size()-1,nums);
    }

    ~NumArray()
    {
        if (root_)
            delete root_;
    }
    void update(int i, int val) {
        update(root_, i, val);   
    }

    int sumRange(int i, int j) {

        return sumRange(root_, i, j);
    }


    void update(SegNode* node, int i, int val)
    {
        if (!node)
            return;
        if (node->start == node->end && node->start == i)
        {
            node->value = val;
            return;
        }
        int mid = (node->start + node->end) /2 ;
        if (i<=mid)
        {
            update(node->left, i, val);
        }
        else if (i>= mid+1)
        {
            update(node->right, i, val);
        }
        node->value = 0;
        if (node->left)
            node->value += node->left->value;
        if (node->right)
            node->value += node->right->value;
    }


    int sumRange(SegNode* node, int i, int j)
    {
        if (!node)
            return 0;

        if (i<= node->start && node->end <= j)
            return node->value;

        int mid = (node->start + node->end) / 2;
        int sum = 0;
        if (node->left && i<=mid)
            sum += sumRange(node->left, i, j);
        if (node->right && mid+1 <= j)
            sum += sumRange(node->right, i, j);

        return sum;
    }

    SegNode* root_;
    SegNode* build( int beg, int end, vector<int>& A)
    {
        if ( beg > end)
            return nullptr;
        SegNode* node = new SegNode( beg, end,A[beg]);
        if (beg == end)
            return node;
        int mid = (beg + end) / 2;

        node->left = build( beg, mid, A);
        node->right = build(mid+1, end, A);
        node->value = 0;
        if (node->left)
            node->value += node->left->value;
        if (node->right)
            node->value += node->right->value;
        return node;
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```

## 931. Median of K Sorted Arrays

There are`k`sorted arrays`nums`. Find the median of the given`k`sorted arrays.

### Example

Given nums =`[[1],[2],[3]]`, return`2.00`

https://www.lintcode.com/problem/median-of-k-sorted-arrays/description

### 解题分析:

BS

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: the given k sorted arrays
     * @return: the median of the given k sorted arrays
     */
    double findMedian(vector<vector<int>> &nums) {
        // write your code here
         int n = getTotal(nums);
        if (n == 0) {
            return 0;
        }
        
        if (n % 2 != 0) {
            return findKth(nums, n / 2 + 1);
        }
        
        return (findKth(nums, n / 2) + findKth(nums, n / 2 + 1)) / 2.0;
    }
    
    int getTotal(vector<vector<int>> & nums) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i].size();
        }
        return sum;
    }
    
    // k is not zero-based, it starts from 1.
    int findKth(vector<vector<int>> & nums, int k) {
        int start = 0, end = INT_MAX;
        
        // find the last number x that >= k numbers are >= x. 
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (getGTE(nums, mid) >= k) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (getGTE(nums, start) >= k) {
            return start;
        }
        
        return end;
    }
    
    // get how many numbers greater than or equal to val in 2d array
    int getGTE(vector<vector<int>> & nums, int val) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            sum += getGTE(nums[i], val);
        }
        return sum;
    }
    
    // get how many numbers greater than or equal to val in an array
    int getGTE(vector<int> & nums, int val) {
        if (nums.size() == 0) {
            return 0;
        }
        
        int start = 0, end = nums.size() - 1;
        
        // find first element >= val 
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] >= val) {
                end = mid;
            } else {
                start = mid;
            }
        }
        
        if (nums[start] >= val) {
            return nums.size() - start;
        }
        
        if (nums[end] >= val) {
            return nums.size() - end;
        }
        
        return 0;
    }
};
```




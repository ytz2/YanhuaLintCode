## 839. Merge Two Sorted Interval Lists

Merge two sorted \(ascending\) lists of interval and return it as a new sorted list. The new sorted list should be made by splicing together the intervals of the two lists and sorted in ascending order.

### Example

Given list1 =`[(1,2),(3,4)]`and list2 =`[(2,3),(5,6)]`, return`[(1,4),(5,6)]`.

[https://www.lintcode.com/problem/merge-two-sorted-interval-lists/description](https://www.lintcode.com/problem/merge-two-sorted-interval-lists/description)

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param list1: one of the given list
     * @param list2: another list
     * @return: the new sorted list of interval
     */
    vector<Interval> mergeTwoInterval(vector<Interval> &list1, vector<Interval> &list2) {
        // write your code here
        vector<Interval> result;
        int i = 0, j = 0;
        auto cmp = [](const Interval& l, const Interval& r) {
            if (l.start == r.start) return l.end < r.end;
            return l.start < r.start;
        };

        auto merge = [](vector<Interval>& res, const Interval& interval) {
          if (res.empty() || interval.start > res.back().end) 
            res.push_back(interval); 
          else {
              res.back().end = max(res.back().end, interval.end);
          }
        };

        while(i < list1.size() && j < list2.size()) {
            const auto& toMerge = cmp(list1[i], list2[j]) ? list1[i++] : list2[j++];
            merge(result, toMerge);
        }
        while(i < list1.size()) merge(result, list1[i++]);
        while(j < list2.size()) merge(result, list2[j++]);
        return result;
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
    vector<int> intersection(vector<int> &nums1, vector<int> &nums2) {
        // write your code here
        unordered_set<int> num1Set(nums1.begin(), nums1.end());
        set<int> res;
        for (auto each : nums2) {
            if (num1Set.count(each)) res.insert(each);
        }
        return vector<int>(res.begin(), res.end());
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

2020/09/20重新犯了一次错误。。。。 弄了一个prefix sum 和min subsum 然后尝试减掉， 但是如果你画画图就发现减出来的不是submatrix而是个L型的矩阵。

所以还是老老实实的写吧。

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
        if ( 0 == m ) return 0;
        int n = matrix[0].size();
        if ( 0 == n ) return 0;

        auto solve = [](const vector<int>& buff){
            int sum = 0, mVal = 0;
            int ret = INT_MIN;
            for (auto each : buff) {
                sum += each;
                ret = max(ret, sum - mVal);
                mVal = min(mVal, sum);
            }
            return ret;
        };
        int res = INT_MIN;
        vector<int> buff(m, 0);
        for (int j = 0; j < n; j++) {
            for (int k = j; k < n; k++) {
                for (int i = 0; i < m; i++) {
                    buff[i] += matrix[i][k];
                }
                // must be here, it should be line by line scan
                res = max(res, solve(buff));
            }
            fill(buff.begin(), buff.end(), 0);
        }
        return res;
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
    vector<vector<int>> multiply(vector<vector<int>> &A, vector<vector<int>> &B) {
        // write your code here
        static vector<vector<int>> def;
        if (A.empty() || A[0].empty() || B.empty() || B[0].empty()) return def;
        int m1 = A.size();
        int n1 = A[0].size();
        int m2 = B.size();
        int n2 = B[0].size();
        if (m2 != n1) return def;
        vector<vector<int>> result(m1, vector<int>(n2, 0));
        for (int i = 0; i < m1; i++) {
            for (int j = 0; j < n2; j++) {
                for (int k = 0; k < n1; k++) {
                    result[i][j] += A[i][k]*B[k][j];
                }
            }
        }
        return result;
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
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();;
        int sum = m + n;
        if (sum % 2) return (double)helper(nums1, nums2, 0, 0, sum / 2 + 1);
        auto s = helper(nums1, nums2, 0, 0, sum/2) + helper(nums1, nums2, 0, 0, sum/2+1);
        return s/2.0;
    }

    int helper(vector<int>& nums1, vector<int>& nums2, int beg1, int beg2, int k) {
        // left is shorter
        if ( nums1.size() - beg1 > nums2.size() - beg2) {
            return helper(nums2, nums1, beg2, beg1, k);
        }
        // exit conditions
        if (nums1.size() == beg1) return nums2[beg2 + k -1];
        if (nums2.size() == beg2) return nums1[beg1 + k -1];
        if (k == 1) return min(nums1[beg1], nums2[beg2]);
        int left =  beg1 + k/2 -1;
        if (left >= nums1.size()) left = nums1.size() - 1;
        int right = beg2 + (k - k/2) - 1;
        if (nums1[left] < nums2[right]) return helper(nums1, nums2, left + 1, beg2,  k - (left - beg1 + 1));
        return helper(nums1, nums2, beg1, right + 1, k/2);
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
class Solution {
public:
    vector<Interval> mergeKSortedIntervalLists(vector<vector<Interval>> &intervals) {
        // write your code here
        auto cmp = [&](const pair<int, int>& l, const pair<int,int>&r) {
          auto& li = intervals[l.first][l.second];
          auto& ri = intervals[r.first][r.second];
          if (li.start == ri.start) return li.end > ri.end;
          return li.start > ri.start;
        };

        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
        for (int i = 0; i < intervals.size(); i++) {
            if (intervals[i].empty()) continue;
            pq.push(make_pair(i, 0));
        }
        vector<Interval> result;
        auto merge = [&](const pair<int,int>& cur) {
            const auto& interval = intervals[cur.first][cur.second];
            if (result.empty() || result.back().end < interval.start) {
                result.push_back(interval);
            } else {
                result.back().end = max(result.back().end, interval.end);   
            }
        };
        while(!pq.empty()) {
            auto cur = pq.top(); pq.pop();
            merge(cur);
            if (++cur.second != intervals[cur.first].size()) pq.push(cur);
        }
        return result;
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
class NumArray {
public:
    NumArray(vector<int> nums) {
        if (nums.empty()) return;
        root = build(nums, 0, nums.size() - 1);
    }

    ~NumArray() {
        if (root) delete root;
    }

    void update(int i, int val) {
        if (root) modify(root, i, val);
    }

    int sumRange(int i, int j) {
        return query(root, i, j);
    }

private:
    struct Node {
      int start = 0;
      int end = 0;
      Node* left = nullptr;
      Node* right = nullptr;
      int sum = 0;
      Node(int s, int e, int sumv=0) : start(s), end(e), sum(sumv){};
      ~Node() {
          if (left) delete left;
          if (right) delete right;
      };
    };
    Node* build(vector<int>& nums, int start, int end) {
        if (start > end) return nullptr;
        auto node = new Node(start, end, nums[start]);
        if (start == end) return node;
        int mid = (start + end) / 2;
        node->left = build(nums, start, mid);
        node->right = build(nums, mid + 1, end);
        node->sum = 0;
        if (node->left) node->sum += node->left->sum;
        if (node->right) node->sum += node->right->sum;
        return node;
    }

    void modify(Node* node, int i, int val) {
        if (!node || i < node->start || i > node->end) return;
        if (node->start == node->end && i == node->start) {
            node->sum = val;
            return;
        }
        int mid = (node->start + node->end) / 2;
        if (i <= mid) modify(node->left, i, val);
        else modify(node->right, i, val);
        node->sum = 0;
        if (node->left) node->sum += node->left->sum;
        if (node->right) node->sum += node->right->sum;
    }

    int query(Node* root, int start, int end) {
        if (!root) return 0;
        if (start <= root->start && root->end <= end) return root->sum;
        int mid = (root->start + root->end) / 2;
        int res = 0;
        if (root->left && start <= mid) res += query(root->left, start, end);
        if (root->right && mid + 1 <= end) res += query(root->right, start, end);
        return res;
    }
private:
    Node* root = nullptr;
};
```

## 931. Median of K Sorted Arrays

There are`k`sorted arrays`nums`. Find the median of the given`k`sorted arrays.

### Example

Given nums =`[[1],[2],[3]]`, return`2.00`

[https://www.lintcode.com/problem/median-of-k-sorted-arrays/description](https://www.lintcode.com/problem/median-of-k-sorted-arrays/description)

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




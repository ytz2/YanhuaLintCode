## 457 Classical Binary Search

Find any position of a target number in a sorted array. Return -1 if target does not exist.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 2, 2, 4, 5, 5]`.

For target =`2`, return 1 or 2.

For target =`5`, return 4 or 5.

For target =`6`, return -1.

[http://www.lintcode.com/en/problem/classical-binary-search/](http://www.lintcode.com/en/problem/classical-binary-search/)

### 解题分析:

不用分析了

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int findPosition(vector<int> &nums, int target) {
        // write your code here
        int n = nums.size();
        if (!n)
            return -1;
        int beg = 0, end = n - 1;

        while(beg+1 < end)
        {
            int mid = beg + (end - beg) / 2;
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                beg = mid;
            else 
                end = mid;
        }

        if (nums[beg] == target)
            return beg;
        if (nums[end] == target)
            return end;
        return -1;
    }
};
```

### 复杂度分析:

log\(n\)

## 141 Sqrt\(x\)

Implement int`sqrt(int x)`.

Compute and return the square root of_x_.

**Example**

sqrt\(3\) = 1

sqrt\(4\) = 2

sqrt\(5\) = 2

sqrt\(10\) = 3

[http://www.lintcode.com/en/problem/sqrtx/](http://www.lintcode.com/en/problem/sqrtx/)

### 解题分析:

求解sqrt就是找一个OOXX的位置，左面的平方小于x,右面的平方大于x，中间要注意的是大数的溢出。再就是注意等于号

### 代码：

```cpp
class Solution {
public:
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    int sqrt(int x) {
        // write your code here
        if (x<0)
            return INT_MAX;
        if (x==0)
            return 0;
        long beg = 1, end = x, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (mid*mid <= x)
                beg = mid;
            else
                end = mid;
        }
        if (end*end <= x)
            return end;
        return beg;
    }
};
```

### 复杂度分析:

log\(x\)

## 586 Sqrt\(x\) II

Implement`double sqrt(double x)`and`x >= 0`.

Compute and return the square root of x.

##### Notice

You do not care about the accuracy of the result, we will help you to output results.

**Example**

Given`n`=`2`return`1.41421356`

[http://www.lintcode.com/en/problem/sqrtx-ii/](http://www.lintcode.com/en/problem/sqrtx-ii/)

### 解题分析:

求解sqrt就是找一个OOXX的位置, 但是稍微有点无聊的就是delta，这个是靠试出来的， 否则过不了，题目应该加个精度。 ooxx的精髓就是保证两边在分歧位置的两侧， 所以mid\*mid &gt; x

### 代码：

```cpp
class Solution {
public:
    /*
     * @param x: a double
     * @return: the square root of x
     */
    double sqrt(double x) {
        // write your code here
        static double delta = 1e-12;
        if (x <= 0 )
            return 0;

        double beg = 0, end = x < 1 ? 1: x;
        while( end - beg > delta )
        {
            double mid = (beg + end)/2;
            if ( mid*mid > x)
                end = mid;
            else 
                beg = mid;
        }
        if (end*end <= x)
            return end;
        return beg;
    }
};
```

### 复杂度分析:

log\(x/delta\)

## 160 Find Minimum in Rotated Sorted Array II

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2\).

Find the minimum element.

##### Notice

The array may contain duplicates.

**Example**

Given`[4,4,5,6,7,0,1,2]`return`0`.

[http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array-ii/\#](http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array-ii/#)

### 解题分析:

OOXX问题， 所以上来先判断是不是符合OOXX， 另外和上一题不一样的是相等的时候如何处理， 相等的时候不能再任性左右移动了，而是要判断移动beg/ end， 这个要根据情况来写了。最坏就是o（n\)了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    int findMin(vector<int> &nums) {
        // write your code here
        if (nums.empty())
            return -1;
        if (nums[0] < nums[nums.size()-1])
            return nums[0];
        int beg = 0, end = nums.size() - 1, mid = 0;

        while (beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (nums[mid] < nums[0])
                end = mid;
            else if (nums[mid] > nums[0])
                beg = mid;
            else
            {
                if (nums[beg] < nums[end])
                    end--;
                else 
                    beg++;
            }
        }
        return nums[beg] < nums[end]? nums[beg]:nums[end];
    }
};
```

log\(x\) , worst o\(n\)

## 63 Search in Rotated Sorted Array II

Follow up for[Search in Rotated Sorted Array](http://www.lintcode.com/problem/search-in-rotated-sorted-array/):

What if**duplicates**are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 1, 0, 1, 1, 1]`and target =`0`, return`true`.  
Given`[1, 1, 1, 1, 1, 1]`and target =`0`, return`false`.

[http://www.lintcode.com/en/problem/search-in-rotated-sorted-array-ii/\#](http://www.lintcode.com/en/problem/search-in-rotated-sorted-array-ii/#)

### 解题分析:

还是类似的问题，先二分为mid在上面还是在下面，然后解决能解决的问题，target在beg,mid中间还是target在mid, end中间的问题

不一样的是a\[mid\] = a\[beg\]， 这个时候就把beg往mid方向移动一下，其实无聊，直接扫一遍算了，反正最坏也是O（N\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: an integer ratated sorted array and duplicates are allowed
     * @param target: An integer
     * @return: a boolean 
     */
    bool search(vector<int> &A, int target) {
        // write your code here
        if (A.empty())
            return false;
        int beg = 0, end = A.size()-1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg) /2 ;
            if (A[mid] == target)
                return true;
            if ( A[mid] > A[beg])
            {
                if (A[beg] <= target && A[mid] >=target)
                    end = mid;
                else
                    beg = mid;
            }
            else if (A[mid] < A[beg])
            {
                if (A[mid] <= target && A[end] >= target)
                    beg = mid;
                else
                    end = mid;
            }
            else
            {
                beg++;
            }
        }
        return A[beg] == target || A[end]==target;
    }
};
```

### 复杂度分析:

log\(x\) , worst o\(n\)

## \*\*\*617 Maximum Average Subarray II

Given an array with positive and negative numbers, find the`maximum average subarray`which length should be greater or equal to given length`k`.

##### Notice

It's guaranteed that the size of the array is greater or equal to_k_.

Have you met this question in a real interview?

Yes

**Example**

Given nums =`[1, 12, -5, -6, 50, 3]`, k =`3`

Return`15.667`// \(-6 + 50 + 3\) / 3 = 15.667

[http://www.lintcode.com/en/problem/maximum-average-subarray-ii/](http://www.lintcode.com/en/problem/maximum-average-subarray-ii/)

### 解题分析:

二分答案：

二分答案的题都挺难的，难在想到用二分答案，而且二分答案通常都是由一个 true or false的子问题来判定的。 比如这道题目，求解最值得问题，一般想到的就是BFS/DP， 但是明显不可解。

先说二分答案是这怎么分吧：

若果a\[0\] - a\[n\]中存在任意一个subarray 大于或等于k的长度，sum \( a\[i\] - A\) &gt;0 那么 则认为A 或者比A大的数字为解， 如果所有的子数组的sum\(a\[i\]-A\) 都小于 0， 那么 证明A 偏大，需要调小A的取值。

那么A的取值如何来取？ 数组中最大和最小中间

子问题：

如何证明在给定数组中 ，input A, k, 判定 至少有一组长度大于k的子数组其sum\(a\[i\]-A\) &gt; 0 这个算是第二个难点吧。

看了下别人的答案，大家都说简单。。。 用到的是一个滚动数组的概念

B= 【a\[0\]-A, a\[1\]-A, ......a\[n-1\]-A】

S  = \[ 0, s\[0\] + B\[1\], s\[1\] + B\[2\].... s\[n-2\]+ a\[n-1\]+A\]

求i, j 使得 s\[j\]-s\[i\] &gt;0 存在，其 i-j  &gt;= k

那么滚动的使得s\[j\]最大， s\[i\]最小即可。

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: an array with positive and negative numbers
     * @param k: an integer
     * @return: the maximum average
     */
    double maxAverage(vector<int> &nums, int k) {
        // write your code here
        double beg =  INT_MAX;
        double end =  INT_MAX;
        for (const auto& each: nums)
        {
            beg = min(beg, (double)each);
            end = max(end, (double)each);
        }

        static double eps = 1e-8;
        while( beg + eps < end)
        {
            double mid = (beg + end)/2.;
            if (check(nums, mid, k))
                beg = mid;
            else
                end = mid;
        }
        return beg;
    }

    // 给定数组，判断数组中是否存在子数组的平均数大于mid,且长度大于k    
    bool check(const vector<int>& nums, double mid, int k)
    {
        // 滚动数组
        vector<double> s(nums.size()+1, 0);
        for (int i = 0; i < nums.size(); i++)
            s[i+1] = s[i]+nums[i] - mid;

        double prevMin = 0, si = INT_MIN;
        for (int i = k; i < s.size(); i++)
        {
            si = max(si, s[i] - prevMin);
            prevMin = min(s[i-k+1], prevMin);
        }
        //只要最大子数组大于0，则必存在至少一个，若最大子数组小于0，则都不存在。
        return si > 0; 
    }

};
```

### 复杂度分析:

nlog\(\(max-min\)/eps\)

## \*\*\*437 Copy Books

Given\_n\_books and the\_i\_th book has`A[i]`pages. You are given\_k\_people to copy the\_n\_books.

\_n\_books list in a row and each person can claim a continous range of the\_n\_books. For example one copier can copy the books from\_i\_th to\_j\_th continously, but he can not copy the 1st book, 2nd book and 4th book \(without 3rd book\).

They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?

**Example**

Given array A =`[3,2,4]`, k =`2`.

Return`5`\( First person spends 5 minutes to copy book 1 and book 2 and second person spends 4 minutes to copy book 3. \)

[http://www.lintcode.com/en/problem/copy-books/\#](http://www.lintcode.com/en/problem/copy-books/#)

### 解题分析:

在连续区间求最优解（最大或者最小）问题，那么和上面的max subarray 是类似的，还是二分答案。

二分：

存在一个解x, 如果给定k个人，能在x以内的工作量完成则x偏大，可以调小x， 如果不能完成，则x偏小，则调大x

解的范围

min\(A\) to sum\(A\)

子问题：

如果给每个人分配最多x 份工作，那么k个人能不能干完。

bool check\( A, k, x\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param pages: an array of integers
     * @param k: An integer
     * @return: an integer
     */
    int copyBooks(vector<int> &pages, int k) {
        // write your code here

        // corner case check
        if (pages.empty())
            return 0;
        if (k <= 0)
            return INT_MAX;
        if (pages.size()==1)
            return pages[0];

        // binary answer search
        int beg = 0;
        int end = 0;
        for (const auto& each : pages)
        {
            beg = min(beg, each);
            end += each;
        }

        while( beg + 1 < end)
        {
            int mid = beg + (end - beg)/2;
            if (check(pages, k, mid))
                end = mid;
            else
                beg = mid;
        }
        if (check(pages, k, end))
            return end;
        return beg;
    }

    bool check(const vector<int>& pages, int k, int val)
    {
        int sum = pages[0];
        int counter = 1;
        for (int i = 1; i < pages.size(); i++)
        {
            if (pages[i] > val)
                return false;

            if ( sum + pages[i] <= val)
                sum += pages[i];
            else
            {
                sum = pages[i];
                counter++;
            }
        }
        return counter <= k;
    }
};
```

nlog\(n\)


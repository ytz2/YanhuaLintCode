# Binary Search and log\(n\) problems

## 458 Last Position of Target

Find the last position of a target number in a sorted array. Return -1 if target does not exist.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 2, 2, 4, 5, 5]`.

For target =`2`, return 2.

For target =`5`, return 5.

For target =`6`, return -1.

[http://www.lintcode.com/en/problem/longest-palindrome/](http://www.lintcode.com/en/problem/last-position-of-target/#)

### 解题分析:

关键词：sorted array, last position, -1

典型的二分查找加线性搜索， 直接写代码就行了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int lastPosition(vector<int> &nums, int target) {
        // write your code here
        if (nums.empty())
            return -1;
        size_t beg = 0, end = nums.size() - 1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (nums[mid] >= target)
                end = mid;
            else
                beg = mid;
        }
        int ind = -1;
        if (nums[end] == target)
            ind = end;
        else if (nums[beg] == target)
            ind = beg;
        else 
            return -1;
        while(ind + 1 < nums.size() && nums[ind] == nums[ind+1])
            ind++;
        return ind;
    }
};
```

### 复杂度分析:

O\(log\(n\)\), n = nums.size\(\), 最坏的情况是o\(n\)

## 585 Maximum Number in Mountain Sequence

Given a mountain sequence of`n`integers which increase firstly and then decrease, find the mountain top.

Have you met this question in a real interview?

Yes

**Example**

Given`nums`=`[1, 2, 4, 8, 6, 3]`return`8`  
Given`nums`=`[10, 9, 8, 7]`, return`10`

[http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/\#](http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/#)

### 解题分析:

二分法的第二种应用OOOXXX， 找分界点问题。

任何一类问题，当左边是O， 右边是X的时候找最后一个O，或者第一个X的时候可以用二分法的思想来解决。

找第一个X伪码如下：

for \( beg : end\)

mid = beg + \(end - beg\) /2

if  isX\(mid\):

```
   beg = mid
```

else

```
   end = mid
```

if \(isX\(end\)\)

```
  return end
```

return beg

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: a mountain sequence which increase firstly and then decrease
     * @return: then mountain top
     */
    int mountainSequence(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if ( 0 == n)
            return INT_MIN;
        if ( 1 == n)
            return nums[0];

        int beg = 0, end = n-1, mid = 0;
        while( beg + 1 < end)
        {
            mid = beg + (end-beg)/2;
            if (nums[mid] > nums[mid+1]) // decreasing
                end = mid;
            else // increasing
                beg = mid;
        }
        if (nums[beg] > nums[beg+1])
            return nums[beg];
        return nums[end];
    }
};
```

### 复杂度分析:

O\(log\(n\)\)

## 460 Find K Closest Elements

Given a target number, a non-negative integer`target`and an integer array A sorted in ascending order, find the`k`closest numbers to target in A, sorted in ascending order by the difference between the number and target. Otherwise, sorted in ascending order by number if the difference is same.

Have you met this question in a real interview?

Yes

**Example**

Given A =`[1, 2, 3]`, target =`2`and k =`3`, return`[2, 1, 3]`.

Given A =`[1, 4, 6, 8]`, target =`3`and k =`3`, return`[4, 1, 6]`.

[http://www.lintcode.com/en/problem/find-k-closest-elements/\#](http://www.lintcode.com/en/problem/find-k-closest-elements/#)

### 解题分析:

算是一道二分法的变种

关键词： sorted, closest, ascending

二分法的时候不但可以用来找给定数字，而且可以用来找最近。因为本质上二分是用的夹逼， 同微积分里面算极限类似。如果用基本二分法的通用模板的话，我们可以发现，最后beg和end的index所代表的值是最逼近所需要查找的值的。

那么从beg 或者 end开始， 向周围扩散k个数字则是我们期望的答案。  以\[ 1, 4, 6, 8\] ， target = 3, k =3 为例子， beg = 0, end = 1, 因为a\[1\] = 4 最接近3，从end 开始，两边用双指针法逐步更新答案 \[4, 1, 6\] 为所得。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: an integer array
     * @param target: An integer
     * @param k: An integer
     * @return: an integer array
     */
    vector<int> kClosestNumbers(vector<int> &A, int target, int k) {
        // write your code here
        vector<int> result;
        if (A.empty() || k == 0)
            return result;
        // what if k > A.size()?

        int beg = 0, end = A.size() - 1, mid = 0;
        while (beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (A[mid] >= target)
                end = mid;
            else
                beg = mid;
        }

        int left = beg, right = end;
        for (int i = 0; i < k ; i++)
        {
            if (left < 0 && right >= A.size())
                break;

            if (left < 0)
            {
                result.push_back(A[right++]);
                continue;
            }
            if (right >= A.size())
            {
                result.push_back(A[left--]);
                continue;
            }
            if ( abs(A[left] - target) <= abs(A[right] - target) )
            {
                result.push_back(A[left--]);
            }
            else
            {
                result.push_back(A[right++]);
            }
        }
        return result;
    }
};
```

### 复杂度分析:

O\(log\(n\) + k\), n = A.size\(\)

## 447 Search in a Big Sorted Array

Given a big sorted array with positive integers sorted by ascending order. The array is so big so that you can not get the length of the whole array directly, and you can only access the kth number by`ArrayReader.get(k)`\(or ArrayReader-&gt;get\(k\) for C++\). Find the first index of a target number. Your algorithm should be in O\(log k\), where k is the first index of the target number.

Return -1, if the number doesn't exist in the array.

##### Notice

If you accessed an inaccessible index \(outside of the array\), ArrayReader.get will return`2,147,483,647`.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 3, 6, 9, 21, ...]`, and target =`3`, return`1`.

Given`[1, 3, 6, 9, 21, ...]`, and target =`4`, return`-1`.

### 解题分析:

Keywords: sorted array, search

明显还是二分法， 但是与一般的二分不一样的地方是这道题目不知道终点在哪里，所以就要二分的办法找到第一个大于target的位置，在进行二分搜索。 越界条件很有意思， 是INT\_MAX，所以肯定不是target， 否则无法搜索。

1. 找到终点
2. 二分搜索

### 代码：

```cpp
class Solution {
public:
    /*
     * @param reader: An instance of ArrayReader.
     * @param target: An integer
     * @return: An integer which is the first index of target.
     */
    int searchBigSortedArray(ArrayReader * reader, int target) {
        // write your code here
        if (!reader)
            return -1;
        int beg = 0, end = 1;

        while(reader->get(end) < target)
            end *= 2;

        while(beg + 1 < end)
        {
            int mid = beg + (end-beg)/2;
            int val = reader->get(mid);
            if (target <= val)
                end = mid;
            else
                beg = mid;
        }
        if (reader->get(beg) == target)
            return beg;
        else if (reader->get(end) == target)
            return end;
        return -1;
    }
};
```

### 复杂度分析:

O\(log\(k\) \)

## 428 Pow\(x, n\)

Implement pow\(x, n\).

##### Notice

You don't need to care about the precision of your answer, it's acceptable if the expected answer and your answer 's difference is smaller than`1e-3`.

Have you met this question in a real interview?

Yes

**Example**

```
Pow(2.1, 3) = 9.261
Pow(0, 1) = 0
Pow(1, 0) = 1
```

[http://www.lintcode.com/en/problem/powx-n/](http://www.lintcode.com/en/problem/powx-n/)

### 解题分析:

求power肯定就是二分了 ， 关键是有几个corner case

x= 0

n = 0

n是负数

考虑清楚这几个边界条件后，直接写代码

### 代码：

递归版本：

```cpp
class Solution {
public:
    /*
     * @param x: the base number
     * @param n: the power number
     * @return: the result
     */
    double myPow(double x, int n) {
        // write your code here
        if (n == 0)
            return 1;
        if (n == 1)
            return x;
        if (n == -1)
            return 1./x;
        double val = myPow(x, n/2);
        if (n%2)
            return val*val* (n<0? 1./x:x);
        return val*val;
    }
};
```

非递归版本：非递归版本看的答案，我觉得很巧妙。 有一个地方要想到： 就是如果要以log\(n\)的复杂度来解决问题需要从n收敛到n=0,在收敛过程中不行的n/=2,同时不停的ans\*= tmp, tmp的初始值为x， 这样在幂指数收敛过程中答案也在以log\(n\)的速度给出， 我觉得很巧妙, 可惜我没看懂lol中间的一个trick, 放弃非递归的解法

### 复杂度分析:

O\(log\(n\) \)

## 140 Fast Power

Calculate the **a^n % b **where a, b and n are all 32bit integers.

Have you met this question in a real interview?

Yes

**Example**

For 231% 3 = 2

For 1001000% 1000 = 0

[http://www.lintcode.com/en/problem/fast-power/\#](http://www.lintcode.com/en/problem/fast-power/#)

### 解题分析:

这道题就是一边求power一边取模， 和拿到用robin-karp做strstr做法一样。原因就是取模怎么取都行， 取几次都可以，这样就不会overflow.

Corner case

返回的时候不要偷懒， 应该返回 1%b, a%b, res%b

注意中间可能会有overflow, 所以需要用long long

### 代码：

递归版本：

```cpp
class Solution {
public:
    /**
     * @param a: A 32bit integer
     * @param b: A 32bit integer
     * @param n: A 32bit integer
     * @return: An integer
     */
    int fastPower(int a, int b, int n) {
        // write your code here
        if (n == 0)
            return 1%b;
        if (n == 1)
            return a%b;

        long long val = fastPower(a, b, n/2);
        long long  res = ((val%b)*(val%b))%b;
        if (n%2)
            return res*a%b;
        return res;
    }
};
```

### 复杂度分析:

O\(log\(n\) \)

## 159 Find Minimum in Rotated Sorted Array

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`\).

Find the minimum element.

##### Notice

You may assume no duplicate exists in the array.

Have you met this question in a real interview?

Yes

**Example**

Given`[4, 5, 6, 7, 0, 1, 2]`return`0`

### 解题分析:

因为是rotated array, 那么最小的值就是rotated的位置符合前面描述的OOXX的标准：

左面大于A\[0\], 右面小于A\[0\]

如图![](/assets/159.png)

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
        int n = nums.size();
        if (0 == n)
            return INT_MAX;
        if (1 == n)
            return nums[0];
        if (nums[0] < nums[n-1])
            return nums[0];
        int beg = 0, end = n-1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (nums[mid] < nums[0])
                end = mid;
            else
                beg = mid;
        }
        return nums[beg] < nums[end]? nums[beg] : nums[end];
    }
};
```

### 复杂度分析:

O\(log\(n\) \)

### 笔记:

这个题目在升序的情况下出错了， \[1, 2, 3\]， 后来临时加了一个判断来判定是不是升序。 原因是OOXX假设了先升序后降序，这样代码就不work, 所以OOXX的套路下， 做好先判断是不是符合再OOXX

## 75 Find Peak Element

There is an integer array which has the following features:

* The numbers in adjacent positions are different.
* A\[0\] &lt;A\[1\] && A\[A.length - 2\] &gt; A\[A.length - 1\].

We define a position P is a peak if:

```
A[P] > A[P-1] && A[P] >A[P+1]
```

Find a peak element in this array. Return the index of the peak.

##### Notice

* It's guaranteed the array has at least one peak.
* The array may contain multiple peeks, find any of them.
* The array has at least 3 numbers in it.

**Example**

Given`[1, 2, 1, 3, 4, 5, 7, 6]`

Return index`1`\(which is number 2\) or`6`\(which is number 7\)

[http://www.lintcode.com/en/problem/find-peak-element/\#](http://www.lintcode.com/en/problem/find-peak-element/#)

### 解题分析:

还是OOXX问题， 只不过是可以有多个peak

先考虑只有一个peak的问题：

如果mid为升序，则start = mid, else end =mid; 最终 peak在start, end中选取

再考虑有多个peak的问题：

mid 肯定在 0， n-1中间产生， 0为升序，n-1为降序， 如果mid为升序， start = mid,  在mid 和 end 中产生， 如果为降序，则在start和mid中产生，依然满足了二分的原则。 因为题目只要求求出其中一个，所以可解。

### 代码：

```cpp
class Solution {
public:
    /*
     * @param A: An integers array.
     * @return: return any of peek positions.
     */
    int findPeak(vector<int>& A) {
        // write your code here
        int beg = 0, end = A.size() - 1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (A[mid] > A[mid-1])
                beg = mid;
            else
                end = mid;
        }
        if (A[beg] > A[beg-1] && A[beg] > A[beg-1])
            return beg;
        return end;
    }
};
```

### 复杂度分析:

O\(log\(n\) \)

## 74 First Bad Version

The code base version is an integer start from 1 to n. One day, someone committed a bad version in the code case, so it caused this version and the following versions are all failed in the unit tests. Find the first bad version.

You can call`isBadVersion`to help you determine which version is the first bad one. The details interface can be found in the code's annotation part.

##### Notice

Please read the annotation in code area to get the correct way to call isBadVersion in different language. For example, Java is`SVNRepo.isBadVersion(v)`

Have you met this question in a real interview?

Yes

**Example**

Given n =`5`:

```
isBadVersion(3) -
>
 false
isBadVersion(5) -
>
 true
isBadVersion(4) -
>
 true
```

Here we are 100% sure that the 4th version is the first bad version.

[http://www.lintcode.com/en/problem/first-bad-version/\#](http://www.lintcode.com/en/problem/first-bad-version/#)

### 解题分析:

这道题比数据流搜索简单多了， 就是一个OOXX，由于前面一题的教训， OOXX之前要先判断一下

### 代码：

```cpp
/**
 * class SVNRepo {
 *     public:
 *     static bool isBadVersion(int k);
 * }
 * you can use SVNRepo::isBadVersion(k) to judge whether 
 * the kth code version is bad or not.
*/


class Solution {
public:
    /**
     * @param n: An integers.
     * @return: An integer which is the first bad version.
     */
    int findFirstBadVersion(int n) {
        // write your code here
        if (n<=0)
            return -1;
        if (SVNRepo::isBadVersion(1))
            return 1;
        int beg = 1, end = n;
        while(beg + 1 < end)
        {
            int mid = beg + (end - beg) / 2;
            if (SVNRepo::isBadVersion(mid))
                end = mid;
            else
                beg = mid;
        }
        return SVNRepo::isBadVersion(beg)? beg : end;
    }
};
```

### 复杂度分析:

O\(log\(n\) \)

## 62 159 Find Minimum in Rotated Sorted Array \(需要重点再看下，每次都错\)

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`\).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Have you met this question in a real interview?

Yes

**Example**

For`[4, 5, 1, 2, 3]`and`target=1`, return`2`.

For`[4, 5, 1, 2, 3]`and`target=0`, return`-1`.

### 解题分析:

做这道题做了第三次了，每次再做还是错的，我都郁闷了。 每次二分的时候我都是用 A\[beg\] &lt; A\[end\] 来做二分，如果成立，说明在左右两区间正常二分，但是处理中间区间的时候逻辑务必混乱。

看来只能靠背了， 用mid作为二分的标准，通过判断mid在上面还是在下面，只解决可解的情况： 去掉\[beg, mid\] 或者\[mid, end\]的情形。 郁闷！！！！

![](/assets/159 - Copy.png)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: an integer rotated sorted array
     * @param target: an integer to be searched
     * @return: an integer
     */
    int search(vector<int> &A, int target) {
        // write your code here
        if (A.empty())
            return -1;
        int beg = 0, end = A.size()-1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (A[beg] < A[mid])
            {
                if (A[beg] <= target && A[mid] >= target)
                    end = mid;
                else
                    beg = mid;
            }
            else
            {
                if (A[end] >= target && A[mid] <= target )
                    beg = mid;
                else
                    end = mid;
            }
        }
        if (A[beg] == target)
            return beg;
        if (A[end] == target)
            return end;
        return -1;
    }
};
```

### 复杂度分析:

O\(log\(n\) \)

## 235 Prime Factorization

Prime factorize a given integer.

##### Notice

You should sort the factors in ascending order.

**Example**

Given`10`, return`[2, 5]`.

Given`660`, return`[2, 2, 3, 5, 11]`.

[http://www.lintcode.com/en/problem/prime-factorization/](http://www.lintcode.com/en/problem/prime-factorization/)

### 解题分析:

这是一道数学题， 分析如下：

要求1： 求质因子

要求2： 从小到大排

解决要求1： a\) 需要知道一个数字是否为另一个数字整除 mod = 0

```
                  b\) 如果该质因子可以被除多次，则除到mod！=0为止
```

解决要求2： 从小到大排，就要从小到大除，这样做有个好处就是8除以2除三次终止，而不是会先除以4

Corner case: 如果给定数字本身就位质数， 那么就是他本身

### 代码：

### 

```cpp
class Solution {
public:
    /**
     * @param num: An integer
     * @return: an integer array
     */
    vector<int> primeFactorization(int num) {
        // write your code here
        vector<int> res;

        if (num <= 1)
            return res;

        for (int i = 2; i <= num; i++)
        {
            while(num % i == 0)
            {
                num /= i;
                res.push_back(i);
            }
        }
        return res;
    }
};
```

### 复杂度分析:

最坏情况nlog\(n\)

## 462 total Occurrence of Target

Given a target number and an integer array sorted in ascending order. Find the total number of occurrences of target in the array.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 3, 3, 4, 5]`and target =`3`, return`2`.

Given`[2, 2, 3, 4, 6]`and target =`4`, return`1`.

Given`[1, 2, 3, 4, 5]`and target =`6`, return`0`.

[http://www.lintcode.com/en/problem/total-occurrence-of-target/\#](http://www.lintcode.com/en/problem/total-occurrence-of-target/#)

### 解题分析:

挺简单的一道题，找最左面，再找最右面，两边end-beg+1就是了，这道题不要当二分来做，要当OOXX来做就行了。

### 代码：

### 

```cpp
class Solution {
public:
    /**
     * @param A: A an integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */

    int totalOccurrence(vector<int> &A, int target) {
        // write your code here
        if (A.empty())
            return 0;
        int beg = search(A, target, Solution::FIRST);
        if (beg == -1)
            return 0;
        int end = search(A, target, Solution::LAST);
        return end-beg+1;
    }
private:

    enum SearchPattern
    {
        FIRST = 0,
        LAST = 1
    };
    int search(vector<int>& A, int target, SearchPattern pattern)
    {
        int beg = 0, end = A.size()-1;
        while(beg + 1 < end)
        {
            int mid = beg + (end - beg) / 2;
            if (pattern == FIRST)
            {
                if (A[mid] >= target)
                    end = mid;
                else
                    beg = mid;
            }
            else
            {
                if (A[mid] <= target)
                    beg = mid;
                else
                    end = mid;
            }
        }

        if (pattern == FIRST)
        {
            if (A[beg] == target)
                return beg;
            if (A[end] == target)
                return end;
        }
        else
        {
            if (A[end] == target)
                return end;
            if (A[beg] == target)
                return beg;
        }

        return -1;
    }


};
```

### 复杂度分析:

log\(n\)

### 459. Closest Number in Sorted Array

Given a target number and an integer array A sorted in ascending order, find the index`i`in A such that A\[i\] is closest to the given target.

Return -1 if there is no element in the array.

##### Notice

There can be duplicate elements in the array, and we can return any of the indices with same value.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 2, 3]`and target =`2`, return`1`.

Given`[1, 4, 6]`and target =`3`, return`1`.

Given`[1, 4, 6]`and target =`5`, return`1`or`2`.

Given`[1, 3, 3, 4]`and target =`2`, return`0`or`1`or`2`.

### 

### 代码：

### 

```cpp
class Solution {
public:
    /*
     * @param A: an integer array sorted in ascending order
     * @param target: An integer
     * @return: an integer
     */
    int closestNumber(vector<int> &A, int target) {
        // write your code here
        if (A.empty())
            return -1;
        int beg = 0, end = A.size()-1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (A[mid] >= target)
                end = mid;
            else
                beg = mid;
        }
        int ind = abs(A[beg] -target) < abs(A[end] - target)? beg:end;
        return ind;
    }
};
```

### 254. Drop Eggs

There is a building of`n`floors. If an egg drops from the\_k\_th floor or above, it will break. If it's dropped from any floor below, it will not break.

You're given two eggs, Find\_k\_while minimize the number of drops for the worst case. Return the number of drops in the worst case.

Have you met this question in a real interview?

Yes

**Clarification**

For n = 10, a naive way to find\_k\_is drop egg from 1st floor, 2nd floor ... kth floor. But in this worst case \(k = 10\), you have to drop 10 times.

Notice that you have two eggs, so you can drop at 4th, 7th & 9th floor, in the worst case \(for example, k = 9\) you have to drop 4 times.

**Example**

Given n =`10`, return`4`.  
Given n =`100`, return`14`.

### 

### 代码：

### 

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: The sum of a and b
     */
    int dropEggs(int n) {
        // write your code here
        long  beg = 1;
        long  end = 1;
        while(end *(end+1) <2*(long)n)
            end*=2;
        while(beg+1<end)
        {
            long mid = beg + (end-beg)/2;
            if (mid*(mid+1)<2*(long)n)
                beg = mid;
            else
                end = mid;
        }
        if (beg*(beg+1) >= 2*(long)n)
            return beg;
        return end;
    }
};
```

### 

### 28. Search a 2D Matrix

Write an efficient algorithm that searches for a value in an\_m\_x\_n\_matrix.

This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

Have you met this question in a real interview?

Yes

**Example**

Consider the following matrix:

```
[
    [1, 3, 5, 7],
    [10, 11, 16, 20],
    [23, 30, 34, 50]
]
```

Given`target = 3`, return`true`.

### 

### 代码：

### 

```cpp
class Solution {
public:
    /**
     * @param matrix: matrix, a list of lists of integers
     * @param target: An integer
     * @return: a boolean, indicate whether matrix contains target
     */
    bool searchMatrix(vector<vector<int>> &matrix, int target) {
        // write your code here
        if (matrix.empty() || matrix[0].empty())
            return false;
        int m = matrix.size();
        int n = matrix[0].size();
        int beg=0, end = m*n-1;
        while(beg + 1 < end)
        {
            int mid = beg + (end - beg)/2;
            int i = mid / n;
            int j = mid % n ;
            if (matrix[i][j] <= target)
                beg = mid;
            else
                end = mid;
        }

        if (matrix[beg/n][beg%n] == target)
            return true;
        if (matrix[end/n][end%n] == target)
            return true;
        return false;
    }
};
```

### 

#### 14. First Position of Target

For a given sorted array \(ascending order\) and a`target`number, find the first index of this number in`O(log n)`time complexity.

If the target number does not exist in the array, return`-1`.

Have you met this question in a real interview?

Yes

**Example**

If the array is`[1, 2, 3, 3, 4, 5, 10]`, for given target`3`, return`2`.

### 

### 代码：

### 

```cpp
class Solution {
public:
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    int binarySearch(vector<int> &nums, int target) {
        // write your code here
        if (nums.empty())
            return -1;
        int beg = 0, end = nums.size()-1, mid = 0;
        while(beg+1<end)
        {
            mid = beg + (end - beg) /2;
            if (nums[mid] >= target )
                end = mid;
            else
                beg = mid;
        }
        if (nums[beg] == target)
            return beg;
        if (nums[end] == target)
            return end;
        return -1;
    }
};
```

### 

## 414 Divide Two Integers

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return`2147483647`

Have you met this question in a real interview?

Yes

**Example**

Given dividend =`100`and divisor =`9`, return`11`.

[http://www.lintcode.com/en/problem/divide-two-integers/](http://www.lintcode.com/en/problem/divide-two-integers/)

### 解题分析:

y/x不能用乘法， 那就只能用位操作 x&lt;&lt;1 = x\*2， 所以把y表示成 y= 2^c1\*x + 2^c2\*x +....

做法就是不断的更新x&lt;&lt;1， 然后系数从c=1开始也不断的c &lt;&lt;1, 那么一直到x&lt;&lt;1 &gt; y的时候c1可得 ，然后继续求\(y-c1x\) /x,直到 y =0为止。中间要注意的是overflow,再就是要会用x^y 与或非符号。

同理，乘法也是这样的 x\*y , 相对简单一点

count = 0;

val = 0;

while count &lt; x;

```
int tmp = 1;

int tmpres = y

while \(tmp&lt;&lt;1 &lt;= x\)

        tmp &lt;&lt;=1

        res &lt;&lt;=1

count += tmp

val += res
```

return val

### 代码：

### 

```cpp
class Solution {
public:
    /**
     * @param dividend: the dividend
     * @param divisor: the divisor
     * @return: the result
     */
    int divide(int dividend, int divisor) {
        // write your code here
        long long dividendVal = abs((long long) dividend);
        long long divisorVal = abs((long long) divisor);
        bool isNegative = dividend > 0 ^ divisor > 0;
        long long res = 0;
        while( dividendVal >= divisorVal)
        {
            long long tmp = divisorVal;
            long long count = 1;
            while( dividendVal >= tmp<<1)
            {
                tmp <<=1;
                count <<=1;
            }
            dividendVal -= tmp;
            res += count;
        }
        if (isNegative)
            return -res < INT_MIN? INT_MIN:-res;
        return res > INT_MAX ? INT_MAX:res;
    }
};
```

### 复杂度分析:

log\(n\)

## 61 Search for a Range

Given a sorted array of\_n\_integers, find the starting and ending position of a given target value.

If the target is not found in the array, return`[-1, -1]`.

Have you met this question in a real interview?

Yes

**Example**

Given`[5, 7, 7, 8, 8, 10]`and target value`8`,  
return`[3, 4]`.

[http://www.lintcode.com/en/problem/search-for-a-range/\#](http://www.lintcode.com/en/problem/search-for-a-range/#)

### 解题分析:

先找到一遍，另外一遍向外衍射就好了，当然end也可以用 二分找，但其实没啥必要，因为当数组内重复元素过高后，复杂度就不算log\(n\)了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: an integer sorted array
     * @param target: an integer to be inserted
     * @return: a list of length 2, [index1, index2]
     */
    vector<int> searchRange(vector<int> &A, int target) {
        // write your code here
        vector<int> res(2, -1);
        if (A.empty())
            return res;

        int beg = 0, end = A.size()-1, mid = 0;
        while( beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (A[mid] >= target)
                end = mid;
            else 
                beg = mid;
        }
        int ind = -1;
        if (A[beg] == target)
            ind = beg;
        else if ( A[end] == target )
            ind = end;
        else
            return res;
        res[0] = ind;
        while(ind+1 < A.size() && A[ind+1] == A[ind])
            ind++;
        res[1] = ind;
        return res;
    }
};
```

### 复杂度分析:

O\(log\(n\) + k\), k为重复大小



## 38 Search a 2D Matrix II

Write an efficient algorithm that searches for a value in an m x n matrix, return the occurrence of it.

This matrix has the following properties:

* Integers in each row are sorted from left to right.
* Integers in each column are sorted from up to bottom.
* No duplicate integers in each row or column.

Have you met this question in a real interview?

Yes

**Example**

Consider the following matrix:

```
[
  [1, 3, 5, 7],
  [2, 4, 7, 8],
  [3, 5, 9, 10]
]
```

Given target =`3`, return`2`.

[http://www.lintcode.com/en/problem/search-a-2d-matrix-ii](http://www.lintcode.com/en/problem/search-a-2d-matrix-ii)

### 解题分析:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]。
```

复杂度要求——O\(m+n\) time and O\(1\) extra space，同时输入只满足自顶向下和自左向右的升序，行与行之间不再有递增关系，与上题有较大区别。时间复杂度为线性要求，因此可从元素排列特点出发，从一端走向另一端无论如何都需要m+n步，因此可分析对角线元素。

首先分析如果从左上角开始搜索，由于元素升序为自左向右和自上而下，因此如果target大于当前搜索元素时还有两个方向需要搜索，不太合适。

如果从右上角开始搜索，由于左边的元素一定不大于当前元素，而下面的元素一定不小于当前元素，因此每次比较时均可排除一列或者一行元素（大于当前元素则排除当前行，小于当前元素则排除当前列，由矩阵特点可知），可达到题目要求的复杂度。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param matrix: A list of lists of integers
     * @param target: An integer you want to search in matrix
     * @return: An integer indicate the total occurrence of target in the given matrix
     */
    int searchMatrix(vector<vector<int>> &matrix, int target) {
        // write your code here
        int m = matrix.size();
        if (m==0)
            return 0;
        int n = matrix[0].size();
        if (n==0)
            return 0;
        int i = m-1, j=0;
        int res = 0;
        while(i >= 0 && j <n)
        {
            auto val = matrix[i][j];
            if (val == target)
            {
                res++;
                i--;j++;
            }
            else if (val < target)
            {
                j++;
            }
            else
            {
                i--;
            }
        }
        return res;
    }
};
```

### 复杂度分析:

O（m+n\)

## 600 Smallest Rectangle Enclosing Black Pixels

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

[http://www.lintcode.com/en/problem/smallest-rectangle-enclosing-black-pixels/\#](http://www.lintcode.com/en/problem/smallest-rectangle-enclosing-black-pixels/#)

### 解题分析:

写了一个BFS， 虽然题目要求是用BS， 我觉得BS写出来和狗屎差不多， BFS就够了

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
        int m = image.size();
        if (0==m)
            return 0;
        int n = image[0].size();
        if (0==n)
            return 0;
        if (x <0 || x >=m || y<0 || y>=n || image[x][y] == '0')
            return 0;
        queue<pair<int,int>> q;
        q.push(make_pair(x,y));
        image[x][y] = '0';
        int top = INT_MAX, right = INT_MIN, down = INT_MIN, left = INT_MAX;  
        vector<int> dx{-1, 1, 0, 0};
        vector<int> dy{ 0, 0, -1,1};
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            top = min(top, p.second);
            down = max(down, p.second);
            left = min(left, p.first);
            right = max(right, p.first);
            for (int i = 0; i < 4; i++)
            {
                int new_x = p.first + dx[i];
                int new_y = p.second +dy[i];
                if (new_x >= 0 && new_x < m && new_y >= 0 && new_y < n && image[new_x][new_y] == '1')
                {
                    q.push(make_pair(new_x,new_y));
                    image[new_x][new_y] = '0';
                }
            }
        }

        return (right-left+1)*(down-top+1);
    }
};
```

### 复杂度分析:

O（s\)

Note:

二分搜索实际上是在找行上的最左面，最右面，列上的最左面，最右面， 二分的时候就是把点压缩到一维，ooxx的找左右边界，有点二分答案的味道。

```
class Solution {
public:
    /**
     * @param image a binary matrix with '0' and '1'
     * @param x, y the location of one of the black pixels
     * @return an integer
     */
    int minArea(vector<vector<char>>& image, int x, int y) {
        // Write your code here
        int m = image.size();
        if (m == 0)
            return 0;
        int n = image[0].size();
        if (n == 0)
            return 0;

        int start = y;
        int end = n - 1;
        int mid;
        while (start < end) {
            mid = start + (end - start) / 2 + 1;
            if (checkColumn(image, mid)) {
                start = mid;
            } else {
                end = mid - 1;
            }
        }
        int right = start;

        start = 0;
        end = y;
        while (start < end) {
            mid = start + (end - start) / 2;
            if (checkColumn(image, mid)) {
                end = mid;
            } else {
                start = mid + 1;
            }
        }
        int left = start;

        start = x;
        end = m - 1;
        while (start < end) {
            mid = start + (end - start) / 2 + 1;
            if (checkRow(image, mid)) {
                start = mid;
            } else {
                end = mid - 1;
            }
        }
        int down = start;

        start = 0;
        end = x;
        while (start < end) {
            mid = start + (end - start) / 2;
            if (checkRow(image, mid)) {
                end = mid;
            } else {
                start = mid + 1;
            }
        }
        int up = start;

        return (right - left + 1) * (down - up + 1);
    }

    bool checkColumn(vector<vector<char>>& image, int col) {
        for (int i = 0; i < image.size(); i++) {
            if (image[i][col] == '1') {
                return true;
            }
        }
        return false;
    }

    bool checkRow(vector<vector<char>>& image, int row) {
        for (int j = 0; j < image[0].size(); j++) {
            if (image[row][j] == '1') {
                return true;
            }
        }
        return false;
    }
};
```




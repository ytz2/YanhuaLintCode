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

[https://www.lintcode.com/problem/last-position-of-target/description](https://www.lintcode.com/problem/last-position-of-target/description)

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

2020/07/20

通过控制等于号的处理， 就可以找出最左最右的位置了，不需要 太麻烦了

golang

```cpp
/**
 * @param nums: An integer array sorted in ascending order
 * @param target: An integer
 * @return: An integer
 */
func lastPosition (nums []int, target int) int {
    // write your code here
    n := len(nums)
    if n == 0 {
        return -1
    }

    b, e := 0, n-1
    for {
        if b + 1 >= e {
            break
        } 
        mid := b + (e - b) / 2
        if nums[mid] <= target {
            b = mid
        } else {
            e = mid
        }
    }
    if nums[e] == target {
        return e
    } else if nums[b] == target {
        return b
    }
    return -1
}
```

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
            if (nums[mid] <= target)
                beg = mid;
            else
                end = mid;
        }
        if (nums[end] == target)
            return end;
        else if (nums[beg] == target)
            return beg;
        return -1;
    }
};
```

## 585 Maximum Number in Mountain Sequence

Given a mountain sequence of`n`integers which increase firstly and then decrease, find the mountain top.

Have you met this question in a real interview?

Yes

**Example**

Given`nums`=`[1, 2, 4, 8, 6, 3]`return`8`  
Given`nums`=`[10, 9, 8, 7]`, return`10`

[http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/\#](http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/#)

[https://leetcode.com/problems/peak-index-in-a-mountain-array/submissions/](https://leetcode.com/problems/peak-index-in-a-mountain-array/submissions/)

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

2020/07/20

当今天下， 非杨即墨 ， 求墨子是谁。。。

```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& A) {
        int beg = 0;
        int end = A.size();
        auto isDescending = [&A](int mid) {
          return A[mid] > A[mid+1];  
        };
        while( beg + 1 < end ) {
            int mid = beg + ( end - beg ) / 2;
            if (isDescending(mid))
                end = mid;
            else
                beg = mid;
        }
        if (isDescending(end))
            return end;
        return beg;
    }
};
```

```go
// golang
func peakIndexInMountainArray(A []int) int {
    b, e := 0, len(A)
    for {
        if  b + 1 >= e {
            break
        }
        mid := b + ( e - b ) /2
        if A[mid] > A[mid+1] {
            e = mid
        } else {
            b = mid
        }
    }
    if A[e] > A[e+1] {
        return e
    }
    return b
}
```

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

2020/07/19 leetcode has similar problem

[https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if (nums.empty()) 
            return {-1, -1};
        vector<int> res{-1,-1};
        int beg =0, end = nums.size() - 1;

        // leftmost first
        while(beg + 1 < end) {
            int mid = beg + (end - beg) / 2;
            if (nums[mid] >= target )
                end = mid;
            else 
                beg = mid;
        } 
        if (nums[beg] == target)
            res[0] = beg;
        else if (nums[end] == target)
            res[0] = end;
        else
            return res;
        beg = 0;
        end = nums.size() - 1;
        while( beg + 1 < end) {
            int mid = beg + (end - beg) / 2;
            if (nums[mid] > target)
                end = mid;
            else
                beg = mid;
        }
        if (nums[end] == target)
            res[1] = end;
        else if (nums[beg] == target)
            res[1] = beg;
        return res;
    }
};
```

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

[https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)

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

2020/07/20

Leetcode上的表述很有意思，提出一个条件是范围在1-1e4之内，所以回头再写这一版的时候就直接上了把end放到1e4上去。 当然，有启发意义的还是第一版的做饭，找到未知边界，更有趣

```cpp
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * class ArrayReader {
 *   public:
 *     int get(int index);
 * };
 */

class Solution {
public:
    int search(const ArrayReader& reader, int target) {
        const static int nan = 2147483647;
        int beg = 0;
        int end  = 1e4;
        while( beg + 1 < end ) {
            int mid = beg + ( end - beg ) / 2;
            auto v = reader.get(mid);
            if (v == nan || v  > target) {
                end = mid;
            } else  {
                beg = mid;
            }
        }
        if (reader.get(beg) == target)
            return beg;
        if (reader.get(end) == target)
            return end;
        return -1;
    }
};
```

```go
// GOLANG
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * type ArrayReader struct {
 * }
 *
 * func (this *MountainArray) get(index int) int {}
 */

func search(reader ArrayReader, target int) int {
    b, e := 0, 1
    for ;reader.get(e) < target; e = e*2 {

    } 
    for {
        if b + 1 >= e {
            break
        }
        mid := b + ( e - b ) / 2
        if reader.get(mid) < target {
            b = mid
        } else {
            e = mid
        }
    }
    if reader.get(b) == target {
        return b
    }
    if reader.get(e) == target {
        return e
    }
    return -1
}
```

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

[https://leetcode.com/problems/powx-n/submissions/](https://leetcode.com/problems/powx-n/submissions/)

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

非递归版本：非递归版本看的答案，我觉得很巧妙。 有一个地方要想到： 就是如果要以log\(n\)的复杂度来解决问题需要从n收敛到n=0,在收敛过程中不行的n/=2,同时不停的ans\*= tmp, tmp的初始值为x， 这样在幂指数收敛过程中答案也在以log\(n\)的速度给出， 我觉得很巧妙, 可惜我没看懂lol中间的一个trick, 放弃非递归的解

### 复杂度分析:

O\(log\(n\) \)

2020/07/21

```go
//GOLANG
func myPow(x float64, n int) float64 {
    if x == 0 {
        return 0
    }
    if n == 0 {
        return 1
    }
    if n == 1 {
        return x
    }
    if n < 0 {
        return 1 / myPow(x, -n)
    }
    v := myPow(x, n/2)
    if n%2 == 1 {
        return v*v*x
    }
    return v*v
}
```

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

2020/07/21

\(a_b\)%c = \(a%c\)_\(b%c\)%c

```go
/**
 * @param a: A 32bit integer
 * @param b: A 32bit integer
 * @param n: A 32bit integer
 * @return: An integer
 */
func fastPower (a int, b int, n int) int {
    // write your code here
    if n == 0 {
        return 1 % b;
    }
    if n == 1 {
        return a % b;
    }
    v := fastPower(a, b, n/2) 
    res := ((v%b) * (v%b)) % b 
    if n%2 == 1 {
        return (res * a)%b
    }
    return res
}
```

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

2020/07/21

没看笔记，重写的时候又错了一遍。。。

```go
func findMin(nums []int) int {
    n := len(nums)
    if nums[0] <= nums[n-1] {
        return nums[0]
    }
    b, e := 0, n-1
    for {
        if b+1 >= e {
            break
        }
        mid := b + (e-b)/2
        if nums[mid] < nums[0] {
            e = mid
        } else {
            b = mid
        }
    }
    if nums[b] < nums[e] {
        return nums[b]
    }
    return nums[e]
}
```

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

[https://leetcode.com/problems/find-peak-element/submissions/](https://leetcode.com/problems/find-peak-element/submissions/)

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

2020/07/21

当今天下，非杨即墨 所以收敛就是了，这是个数学问题。  刚开始我还test 1个元素还有单序问题， 因为是和邻居比所以总是顺杆爬的。 最后一行解决了非杨即墨的问题，不需要太复杂的逻辑了

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        int beg = 0;
        int end = n - 1;
        while(beg + 1 < end) {
            int mid = beg + (end-beg)/2;
            if (nums[mid] > nums[mid+1])
                end = mid;
            else
                beg = mid;
        }
        return nums[beg] > nums[end]? beg:end;
    }
};
```

```go
// GOLANG
func findPeakElement(nums []int) int {
    b, e := 0, len(nums) - 1
    for {
        if b + 1 >= e {
            break
        }
        mid := b + (e - b) / 2
        if nums[mid] > nums[mid+1] {
            e = mid
        } else {
            b = mid
        }
    }
    if nums[b] > nums[e] {
        return b
    }
    return e
}
```

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

[https://leetcode.com/problems/first-bad-version/submissions/](https://leetcode.com/problems/first-bad-version/submissions/)

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

OOXX 问题目标找X先看beg再看end， 因为很有可能只是一个单调问题，先看beg就解决了单调问题

```go
/** 
 * Forward declaration of isBadVersion API.
 * @param   version   your guess about first bad version
 * @return                true if current version is bad 
 *                      false if current version is good
 * func isBadVersion(version int) bool;
 */

func firstBadVersion(n int) int {
    b, e := 1, n
    for {
        if b + 1 >= e {
            break
        }
        m := b + (e - b) / 2
        if isBadVersion(m) {
            e = m
        } else {
            b = m
        }
    }
    if isBadVersion(b) {
        return b
    }
    return e
}
```

## 62 Search in Rotated Sorted Array \(需要重点再看下，每次都错\)

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`\).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Have you met this question in a real interview?

Yes

**Example**

For`[4, 5, 1, 2, 3]`and`target=1`, return`2`.

For`[4, 5, 1, 2, 3]`and`target=0`, return`-1`.



[https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/)

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

2020/07/21

二分的思想是在不确定中找确定。 确定的是在上面还是在下面， 单调增

```go
//GOLANG
func search(nums []int, target int) int {
    if len(nums) == 0 {
        return -1
    }
    b, e := 0, len(nums) - 1
    for {
        if b + 1 >= e {
            break
        }
        mid := b + ( e - b ) / 2
        if nums[mid] > nums[0] {
            if nums[mid] >= target && nums[b] <= target {
                e = mid;
            } else {
                b = mid;
            }
        } else {
            if nums[mid] <= target && nums[e] >= target {
                b = mid
            } else {
                e = mid
            }
        }
    }
    if nums[b] == target {
        return b
    }
    if nums[e] == target {
        return e
    }
    return -1
}
```

## 




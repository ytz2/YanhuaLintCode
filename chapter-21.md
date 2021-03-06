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

2020/07/22

质因数的处理上多了个sqrt\(n\) 和最后剩下的渣渣，这样可以过test case， 用sqrt算是数学上一个argument， 也就是sqrt可以被找出余数那么实际上所有的已经被找过了

```go
/**
 * @param num: An integer
 * @return: an integer array
 */
import "math"
func primeFactorization (num int) []int {
    // write your code here
    res := make([]int,0)
    if num <= 1 {
        res = append(res, num)
        return res
    }
    n := num
    for i := 2; i <= int(math.Sqrt(float64(n))); i++ {
        for {
            if num%i == 0 {
                res = append(res, i)
                num = num / i
            } else {
                break
            }
        }
    }
    if num != 1 {
        res = append(res, num)
    }
    return res
}
```

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

2020/07/22

如果没记错这是quant面试的经典题， 第一个决定后面多少

```go
/**
 * @param n: An integer
 * @return: An integer
 */

 // find x to make x + x-1 + x-2 + 1 >= n
 // solve (1+x)x / 2 >= n
 // eg. n = 10 -> x^2+x -10 >=0 ==> (x+5)(x-4) >=0 , x >=4 or x >= -5
func dropEggs (n int) int {
    // write your code here
    b, e := 1, n
    for {
        if b + 1 >= e {
            break
        }
        m := b + (e - b) / 2
        if m*(m+1) / 2 < n {
            b = m
        } else {
            e = m
        }
    }
    if b*(b+1)/2 >= n {
        return b
    }
    return e
}
```

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

[https://leetcode.com/problems/search-a-2d-matrix/](https://leetcode.com/problems/search-a-2d-matrix/)

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

2020/07/22

傻了， 写了个复杂的算法 用了两次二分， logm+logn  == log\(m\*n\)

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty())
           return false;
        int beg = 0;
        int end = matrix.size() - 1;
        while ( beg + 1 < end) {
            int mid = beg + (end - beg)/2;
            if (matrix[mid][0] > target)
                end = mid;
            else  
                beg = mid;
        }
        if (matrix[beg][0] > target)
            return false;
        beg = matrix[end][0] <= target? end:beg;
        int b = 0;
        int e = matrix[0].size() - 1;
        while (b + 1 < e) {
            int mid = b + ( e - b ) / 2;
            if (matrix[beg][mid] < target)
                b = mid;
            else 
                e = mid;
        }
        return matrix[beg][b] == target || matrix[beg][e] == target;
    }
};
```

```go
// GOLANG
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    m, n := len(matrix), len(matrix[0])
    b, e := 0, m*n - 1
    for {
        if b + 1 >= e {
            break
        }
        mid := b + (e - b)/2
        if matrix[mid/n][mid%n] < target {
            b = mid
        } else {
            e = mid
        }
    }
    return matrix[b/n][b%n] == target || matrix[e/n][e%n] == target;
}
```

#### 14. First Position of Target

For a given sorted array \(ascending order\) and a`target`number, find the first index of this number in`O(log n)`time complexity.

If the target number does not exist in the array, return`-1`.

Have you met this question in a real interview?

Yes

**Example**

If the array is`[1, 2, 3, 3, 4, 5, 10]`, for given target`3`, return`2`.

[https://www.lintcode.com/problem/first-position-of-target/description](https://www.lintcode.com/problem/first-position-of-target/description)

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

### 2020/07/22

```go
/**
 * @param nums: The integer array.
 * @param target: Target to find.
 * @return: The first position of target. Position starts from 0.
 */
func binarySearch (nums []int, target int) int {
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
        m := b + ( e - b ) / 2
        if (nums[m] >= target) {
            e = m
        } else {
            b = m
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

[https://leetcode.com/problems/divide-two-integers/submissions/](https://leetcode.com/problems/divide-two-integers/submissions/)

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

2020/07/22

```cpp
class Solution {
public:
    int divide(int dividend , int divisor) {
        bool isNegative = (dividend < 0) ^ (divisor < 0);
        long up = dividend < 0 ? -(long)dividend : dividend;
        long down = divisor < 0 ? -(long)divisor : divisor;
        long res = 0;
        while( up >= down ) {
            auto tmp = down;
            auto count = 1;
            while (up >= tmp) {
                up -= tmp;
                res += count;
                count <<= 1;
                tmp <<= 1;
            }
        }
        res = isNegative? -res : res;
        if (res < INT_MIN || res > INT_MAX) 
            return INT_MAX;
        return res;
    }
};
```

```go
// GOLANG
func divide(dividend int, divisor int) int {
    INT_MAX, INT_MIN := 2147483647, -2147483648
    isNegative := (dividend < 0  ||  divisor < 0  ) && !(divisor < 0 && dividend < 0 ) // golang has no xor, simulate it
    if dividend < 0 {
        dividend = -dividend
    }
    if divisor < 0 {
        divisor = -divisor
    }

    res := 0
    for {
        if dividend < divisor {
            break
        }
        tmp := divisor 
        count := 1
        for {
            if dividend < tmp {
                break
            }
            dividend -= tmp
            res += count
            tmp = tmp << 1
            count = count << 1
        }
    }
    if isNegative {
        res = -res
    }
    if res < INT_MIN || res > INT_MAX {
        return INT_MAX
    }
    return res
}
```

## 61 Search for a Range

Given a sorted array of\_n\_integers, find the starting and ending position of a given target value.

If the target is not found in the array, return`[-1, -1]`.

Have you met this question in a real interview?

Yes

**Example**

Given`[5, 7, 7, 8, 8, 10]`and target value`8`,  
return`[3, 4]`.

[http://www.lintcode.com/en/problem/search-for-a-range/\#](http://www.lintcode.com/en/problem/search-for-a-range/#)

[https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/submissions/](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/submissions/)

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

2020/07/22

动手写一遍就发现为啥以前横着扫一遍了，因为一旦有重复元素，二分搜素效率不是很高

```go
func searchRange(nums []int, target int) []int {
    res := make([]int, 2)
    res[0], res[1] = -1, -1
    n := len(nums)
    if n == 0 {
        return res
    }
    b, e := 0, n-1
    for {
        if b + 1 >= e {
            break
        }
        m := b + ( e - b ) / 2
        if nums[m] >= target {
            e = m
        } else {
            b = m
        }
    }
    if nums[b] == target {
        res[0] = b
    } else if nums[e] == target {
        res[0] = e
    } else {
        return res
    }

    b, e = 0, n-1
    for {
        if b + 1 >= e {
            break
        }
        m := b + ( e - b ) / 2
        if nums[m] <= target {
            b = m
        } else {
            e = m
        }
    }
    if nums[e] == target {
        res[1] = e
    } else if nums[b] == target {
        res[1] = b
    } 
    return res
}
```

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

[https://leetcode.com/problems/search-a-2d-matrix-ii/](https://leetcode.com/problems/search-a-2d-matrix-ii/)

解题分析:

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

2020/07/22

不走回头路所以才有这个解法

```go
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    m, n := len(matrix), len(matrix[0])
    i, j := m-1, 0
    for {
        if i < 0 || j >= n {
            break
        }
        if matrix[i][j] == target {
            return true
        } else if target < matrix[i][j] {
            i--
        } else {
            j++
        }

    }
    return false
}
```

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

[https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/submissions/](https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/submissions/)

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

2020/07/23

二维的二分答案， 回顾起来觉得很有意思mlog\(n\) +nlog\(m\) 压缩的时候不可避免有个线性。 当然最Naive 的还是bfs, 现在回头看以前写的二分是错的， 当时还是没有理解二分的精髓啊 ：p

```go
func minArea(image [][]byte, x int, y int) int {
    // corner case
    if len(image) == 0 || len(image[0]) == 0 {
        return 0
    }
    if image[x][y] == '0' {
        return 0
    }

    l, r, u, d := y, y, x, x

    // zip vertically
    checkRow := func(image [][]byte,  y int) bool {
        for i := 0; i < len(image); i++ {
            if image[i][y] == '1' {
                return true
            }   
        }
        return false
    }

    checkCol := func(image [][]byte,  x int) bool {
        for i := 0; i < len(image[0]); i ++ {
            if image[x][i] == '1' {
                return true
            }
        }
        return false
    }

    //1 fix row, search col, left most
    b, e := 0, y
    for {
        if b + 1 >= e {
            break
        }
        m := b + ( e - b ) / 2
        if checkRow(image, m) {
            e = m
        } else {
            b = m
        }
    }
    if checkRow(image, b) {
        l = b
    } else if checkRow(image, e) {
        l = e
    }

    // 2 fix row, search col, right most
    b, e = y, len(image[0]) - 1
    for {
        if b + 1 >= e {
            break
        }        
        m := b + ( e - b ) / 2
        if checkRow(image, m) {
            b = m
        } else {
            e = m
        }
    }
    if checkRow(image, e) {
        r = e
    } else if checkRow(image, b) {
        r = b
    }

    //3 fix  col, search row, upper most
    b, e  = 0, x
    for {
        if b + 1 >= e {
            break
        }        
        m := b + ( e - b ) / 2
        if checkCol(image, m) {
            e = m
        } else {
            b = m
        }
    }
    if checkCol(image, b) {
        u = b
    } else if checkCol(image, e) {
        u = e
    }

    // 4 fix col, search row, down most
    b, e  = x, len(image) - 1
    for {
        if b + 1 >= e {
            break
        }        
        m := b + ( e - b ) / 2
        if checkCol(image, m) {
            b = m
        } else {
            e = m
        }
    }
    if checkCol(image, e) {
        d = e
    } else if checkCol(image, b) {
        d = b
    }
    return (r- l + 1) * (d -u + 1)
}
```




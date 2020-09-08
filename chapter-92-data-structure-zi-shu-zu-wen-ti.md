![](/assets/subarray.png)

## 138. Subarray Sum

Given an integer array, find a subarray where the sum of numbers is**zero**. Your code should return the index of the first number and the index of the last number.

### Example

Given`[-3, 1, 2, -3, 4]`, return`[0, 2]`or`[1, 3]`.

[https://www.lintcode.com/problem/subarray-sum/description](https://www.lintcode.com/problem/subarray-sum/description)

### 解题分析:

套公式 ， 求一个s\[j+1\] = s\[i\] 的解， 返回的是 \[i,j\],所以用一个hashtable，初始为0， 如果前面没有相等的，则插入i+1, 注意， i+1是因为我们从0 开始

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    vector<int> subarraySum(vector<int> &nums) {
        // write your code here
        unordered_map<int, int> tbl;
        int sum = 0;
        tbl[0] = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            sum += nums[i];
            if (tbl.count(sum))
                return vector<int>{tbl[sum], i};
            tbl[sum] = i+1;
        }
        return vector<int>{-1,-1};
    }
};
```

## 139. Subarray Sum Closest

Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.

### Example

Given`[-3, 1, 1, -3, 5]`, return`[0, 2]`,`[1, 3]`,`[1, 1]`,`[2, 2]`or`[0, 4]`.

### Challenge

O\(nlogn\) time

[https://www.lintcode.com/problem/subarray-sum-closest/description](https://www.lintcode.com/problem/subarray-sum-closest/description)

### 解题分析:

套路是一样的， 区别是要sort一下打擂台

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    vector<int> subarraySumClosest(vector<int> &nums) {
        // write your code here
        vector<int> result;
        vector<pair<int, int>> sum{make_pair(0, 0)};
        for (auto& num : nums) sum.push_back(make_pair(sum.size(), sum.back().second + num));
        sort(sum.begin(), sum.end(), [](const pair<int,int>& l, const pair<int, int>& r){
           return  l.second < r.second;
        });
        int diff = INT_MAX;
        int ind = 0;
        for(int i = 0; i <sum.size() - 1; i++) {
            auto dif = abs(sum[i].second - sum[i+1].second);
            if (dif < diff) {
                ind = i;
                diff = dif;
            }
        }
        if (sum[ind].first < sum[ind+1].first) 
            return {sum[ind].first, sum[ind+1].first-1};
        return { sum[ind+1].first, sum[ind].first -1};
    }
};
```

## 41. Maximum Subarray

Given an array of integers, find a contiguous subarray which has the largest sum.

### Example

Given the array`[−2,2,−3,4,−1,2,1,−5,3]`, the contiguous subarray`[4,−1,2,1]`has the largest sum =`6`.

### Challenge

Can you do it in time complexity O\(n\)?

[https://www.lintcode.com/problem/maximum-subarray/description](https://www.lintcode.com/problem/maximum-subarray/description)

### 解题分析:

最大前缀和减去最小前缀和就是结果了，打一个擂台

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    int maxSubArray(vector<int> &nums) {
        // write your code here
        int minV = 0, maxV = INT_MIN;
        int sum = 0; 
        for (int i = 0; i < nums.size(); i++)
        {
            sum+= nums[i];
            maxV = max(maxV, sum - minV);
            minV = min(minV, sum);
        }
        return maxV;
    }
};
```

## 620. Maximum Subarray IV

Given an integer arrays, find a contiguous subarray which has the largest sum and length should be greater or equal to given length`k`.  
Return the largest sum, return 0 if there are fewer than k elements in the array.

### Example

Given the array`[-2,2,-3,4,-1,2,1,-5,3]`and k =`5`, the contiguous subarray`[2,-3,4,-1,2,1]`has the largest sum =`5`.

[https://www.lintcode.com/problem/maximum-subarray-iv/description](https://www.lintcode.com/problem/maximum-subarray-iv/description)

### 解题分析:

老套路，维护一个maxV, minV, 但是minV延迟k个元素计算， 这样 保证 一个最大减一个最小就好

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an array of integer
     * @param k: an integer
     * @return: the largest sum
     */
    int maxSubarray4(vector<int> &nums, int k) {
        // write your code here
        if (nums.size() < k)
            return 0;
        int sum = 0, minSum = 0;
        int maxV = INT_MIN, minV = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            sum += nums[i];
            if (i+1 < k)
                continue;
            maxV = max(maxV, sum - minV);
            minSum += nums[i-k+1];
            minV = min(minV, minSum);
        }
        return maxV;
    }
};
```

## 617. Maximum Average Subarray II

Given an array with positive and negative numbers, find the`maximum average subarray`which length should be greater or equal to given length`k`.

### Example

Given nums =`[1, 12, -5, -6, 50, 3]`, k =`3`

Return`15.667`// \(-6 + 50 + 3\) / 3 = 15.667

[https://www.lintcode.com/problem/maximum-average-subarray-ii/description](https://www.lintcode.com/problem/maximum-average-subarray-ii/description)

### 解题分析:

复杂度：O\(nlog\(max + min\)\)，其中n是nums的长度，max和min分别是nums中的最大值和最小值。这里用了“二分答案”思想。

所求的最大平均值一定是介于原数组的最大值和最小值之间，所以我们的目标是用二分法来快速的在这个范围内找到我们要求的最大平均值，初始化left为原数组的最小值，right为原数组的最大值，然后mid就是left和right的中间值，难点就在于如何得到mid和要求的最大平均值之间的大小关系，从而判断二分的方向。我们想，如果我们已经算出来了这个最大平均值maxAvg，那么对于任意一个长度大于等于k的数组，如果让每个数字都减去maxAvg，那么得到的累加差值一定是小于等于0的。所以我们通过left和right值算出来的mid，可以看作是maxAvg的一个candidate，所以我们就让数组中的每一个数字都减去mid，然后算差值的累加和，一旦发现累加和大于0了，那么说明我们mid比maxAvg小，这样就可以判断方向了。

具体步骤：  
1.每次进入循环时，我们建立一个前缀和数组prefixSums，然后求出原数组中最小值赋给left，最大值赋给right，题目中说了误差是1e-6，所以我们的循环条件就是right比left大1e-6；  
2.然后我们算出来mid，prefixSumMin初始为0，maxSum初始化为INT\_MIN。然后开始遍历数组，先更新prefixSums，注意prefixSums是它们和mid相减的差值累加。我们的目标是找长度大于等于k的子数组的平均值大于mid，由于我们每个数组都减去了mid，那么就转换为找长度大于等于k的子数组的差累积值大于0。然后问题转变成了“最大 &gt;k 的 sum range 要小于0”的问题。如果确实小于0，则end = mid，否则start = mid。

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
        double l = INT_MAX, r = INT_MIN;
        for ( auto& num : nums)
        {
            l = min(l, (double)num);
            r = max(r, (double)num);
        }
        vector<double> sumarr(nums.size()+1, 0);
        while( l + 1e-6 < r)
        {
            double mid = (l + r)/2;
            for (int i = 0; i< nums.size(); i++)
                sumarr[i+1] = sumarr[i]+nums[i] - mid;

            double maxV = INT_MIN;
            double preMin = 0;
            for (int i = k; i<sumarr.size(); i++)
            {
                maxV = max(maxV, sumarr[i] - preMin);
                preMin = min(preMin, sumarr[i-k+1]);
            }

            if (maxV >= 0)
                l = mid;
            else
                r = mid;
        }
        return l;
    }
};
```

## 191. Maximum Product Subarray

Find the contiguous subarray within an array \(containing at least one number\) which has the largest product.

### Example

For example, given the array`[2,3,-2,4]`, the contiguous subarray`[2,3]`has the largest product =`6`.

[https://www.lintcode.com/problem/maximum-product-subarray/description](https://www.lintcode.com/problem/maximum-product-subarray/description)

### 解题分析:

最后一步: 考虑最后一个数A\[n\], 那么乘积最大的就是前面以n - 1为结尾的最大乘积, 再乘上这个数

1. 转移方程:

   * 维护两个数组, f\[i\] 和 g\[i\], f\[i\]用于记录最大值, g\[i\]用于记录最小值.
   * A\[i\]代表数组中的第i个数
   * 转移方程: f\[i\] = max\(i -  
     &gt;  
      1...n \| f\[i - 1\] \* A\[i\], g\[i - 1\] \* A\[i\], A\[i\]\)

     g\[i\] = min\(i -  
     &gt;  
      1...n \| f\[i - 1\] \* A\[i\], g\[i - 1\] \* A\[i\], A\[i\]\)

2. 初始条件与边界情况:

   * f\[0\] = A\[0\], g\[0\] = A\[0\]

3. 计算顺序:

   * 从左往右
   * 最终结果max\(i -
     &gt;
      0...n \| f\[i\]\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: An array of integers
     * @return: An integer
     */
    int maxProduct(vector<int> &nums) {
        // write your code here
        // state definition and initial condition
        vector<int> maxDp{1}, minDp{1};
        int res = INT_MIN;
        // transition
        for (int i = 0; i < nums.size(); i++)
        {
            int num = nums[i];
            if (num > 0)
            {
                maxDp.push_back(max(maxDp[i] * num, num));
                minDp.push_back(min(minDp[i] * num, num));
            }
            else
            {
                minDp.push_back(min(maxDp[i] * num, num));
                maxDp.push_back(max(minDp[i] * num, num));
            }
            res = max(res, maxDp.back());
        }
        return res;
    }
};
```

## 45. Maximum Subarray Difference

Given an array with integers.

Find two_non-overlapping\_subarrays\_A\_and\_B_, which`|SUM(A) - SUM(B)|`is the largest.

Return the largest difference.

### Example

For`[1, 2, -3, 1]`, return`6`.

### Challenge

O\(n\) time and O\(n\) space.

[https://www.lintcode.com/problem/maximum-subarray-difference/description](https://www.lintcode.com/problem/maximum-subarray-difference/description)

### 解题分析:

用的DP， 有点像best time to buy sell stock

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: An integer indicate the value of maximum difference between two substrings
     */
    int maxDiffSubArrays(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        vector<int> leftMax(n, 0), leftMin(n, 0), rightMax(n,0), rightMin(n,0);
        leftMax[0] = leftMin[0] = nums[0];
        rightMax[n-1] = rightMin[n-1] = nums[n-1];
        int leftSum = nums[0], leftMinv = 0, leftMaxv = 0;
        for (int i = 1; i < n; i++)
        {
            leftMinv = min(leftMinv, leftSum);
            leftMaxv = max(leftMaxv, leftSum);
            leftSum += nums[i];
            leftMax[i] = max(leftMax[i-1], leftSum - leftMinv);
            leftMin[i] = min(leftMin[i-1], leftSum - leftMaxv);
        }
        int rightSum = nums[n-1], rightMinv = 0, rightMaxv = 0;
        for (int i = n-2; i >= 0; i--)
        {
            rightMinv = min(rightMinv, rightSum);
            rightMaxv = max(rightMaxv, rightSum);
            rightSum += nums[i];
            rightMax[i] = max(rightMax[i+1], rightSum -rightMinv);
            rightMin[i] = min(rightMin[i+1], rightSum - rightMaxv);
        }

        int res = INT_MIN;
        for (int i =0; i<n-1; i++)
        {
            res = max(res, max(abs(leftMax[i] - rightMin[i+1]), abs(leftMin[i] - rightMax[i+1])));
        }
        return res;
    }
};
```

## 42. Maximum Subarray II

Given an array of integers, find two non-overlapping subarrays which have the largest sum.  
The number in each subarray should be contiguous.  
Return the largest sum.

### Example

For given`[1, 3, -1, 2, -1, 2]`, the two subarrays are`[1, 3]`and`[2, -1, 2]`or`[1, 3, -1, 2]`and`[2]`, they both have the largest sum`7`.

### Challenge

Can you do it in time complexity O\(_n_\) ?

[https://www.lintcode.com/problem/maximum-subarray-ii/description](https://www.lintcode.com/problem/maximum-subarray-ii/description)

### 解题分析:

用的DP， 有点像best time to buy sell stock

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers
     * @return: An integer denotes the sum of max two non-overlapping subarrays
     */
    int maxTwoSubArrays(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        vector<int> leftMax(n, 0), rightMax(n,0);
        leftMax[0] = nums[0];
        rightMax[n-1] = nums[n-1];

        int leftSum = nums[0], prevLeftMin = min(0,nums[0]);
        for (int i = 1; i < n; i++)
        {
            leftSum+= nums[i];
            leftMax[i] = max(leftMax[i-1], leftSum-prevLeftMin);
            prevLeftMin = min(prevLeftMin, leftSum);
        }
        int rightSum = nums[n-1], prevRightMin = min(0,nums[n-1]);
        for (int i = n-2; i >= 0; i--)
        {
            rightSum += nums[i];
            rightMax[i] = max(rightMax[i+1] , rightSum - prevRightMin);
            prevRightMin = min(prevRightMin, rightSum);
        }
        int res = INT_MIN;
        for (int i = 0; i<n-1; i++)
            res = max(res, leftMax[i] + rightMax[i+1]);
        return res;
    }
};
```




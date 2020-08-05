## 607 Two Sum - Data structure design

Design and implement a TwoSum class. It should support the following operations:`add`and`find`.

`add`- Add the number to an internal data structure.  
`find`- Find if there exists any pair of numbers which sum is equal to the value.

Have you met this question in a real interview?

Yes

**Example**

```
add(1); add(3); add(5);
find(4) // return true
find(7) // return false
```

[http://www.lintcode.com/en/problem/two-sum-data-structure-design/](http://www.lintcode.com/en/problem/two-sum-data-structure-design/)

[https://leetcode.com/problems/two-sum-iii-data-structure-design/](https://leetcode.com/problems/two-sum-iii-data-structure-design/)

### 解题分析:

挺好玩的一个题目， two sum算是经典题了，但这道题是让选数据结构。 什么样的数据结构插入简单搜索也简单？

如果从two sum那道题过来，要求storage是排好的， 且允许重复出现。（multiset\) . 而且搜索的最优定为 o\(n\) 两根指针

### 代码：

```cpp
class TwoSum {
public:
    /*
     * @param number: An integer
     * @return: nothing
     */
    void add(int number) {
        // write your code here
        store.insert(number);
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    bool find(int value) {
        // write your code here
        auto beg = store.begin();
        auto end = --store.end();
        while(beg != end)
        {
            int sum = *beg+*end;
            if (sum == value)
                return true;
            else if (sum > value)
                --end;
            else
                ++beg;
        }
        return false;
    }

    multiset<int> store;
};
```

### 复杂度分析:

add: o\(log\(n\)\) find o\(n\)

或者不指望排序，但是用unordered\_map&lt;int,int&gt; key是数字，value是count，这样就可以存多个数字了，insert是O（1）, find O\(n\)

```
class TwoSum {
public:
    /*
     * @param number: An integer
     * @return: nothing
     */
    void add(int number) {
        // write your code here
        store[number]++;
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    bool find(int value) {
        // write your code here
        for (auto each : store)
        {
            auto expected = value - each.first;
            if (expected == each.first)
            {
                if (each.second > 1)
                    return true;
                continue;
            }

            if (store.find(expected) !=store.end())
                return true;
        }
        return false;
    }

    unordered_map<int, int> store;
};
```

2020/08/03

```go
type TwoSum struct {
    dict map[int]int
}


/** Initialize your data structure here. */
func Constructor() TwoSum {
    return TwoSum {
        dict: make(map[int]int, 0),
    }
}


/** Add the number to an internal data structure.. */
func (this *TwoSum) Add(number int)  {
    this.dict[number] = this.dict[number] + 1       
}


/** Find if there exists any pair of numbers which sum is equal to the value. */
func (this *TwoSum) Find(value int) bool {
    for k, v := range this.dict {
        t := value - k
        if t == k {
            if v > 1 {
                return true
            } else {
                continue
            }
        }

        if _, ok := this.dict[t]; ok {
            return true
        }
    }
    return false
}


/**
 * Your TwoSum object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Add(number);
 * param_2 := obj.Find(value);
 */
```

## 539 Move Zeroes

Given an array`nums`, write a function to move all`0`'s to the end of it while maintaining the relative order of the non-zero elements.

##### Notice

1. You must do this
   **in-place**
   without making a copy of the array.
2. Minimize the total number of operations.

Have you met this question in a real interview?

Yes

**Example**

Given`nums = [0, 1, 0, 3, 12]`, after calling your function,`nums`should be`[1, 3, 12, 0, 0]`.

[http://www.lintcode.com/en/problem/move-zeroes/\#](http://www.lintcode.com/en/problem/move-zeroes/#)

[https://leetcode.com/problems/move-zeroes/submissions/](https://leetcode.com/problems/move-zeroes/submissions/)

### 解题分析:

快慢指针

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an integer array
     * @return: nothing
     */
    void moveZeroes(vector<int> &nums) {
        // write your code here
        if (nums.empty())
            return;
        int slow = 0, fast =0;
        while(fast < nums.size())
        {
            if (nums[fast] ==0 )
            {
                fast++;
                continue;
            }

            while(slow<fast && nums[slow]!=0)
                slow++;
            swap(nums[slow],nums[fast]);
            fast++;
        }
    }
};
```

### 复杂度分析:

o\(n\)

2020/08/02

```go
func moveZeroes(nums []int)  {
    for s, f := 0, 0; f < len(nums); f++ {
        if nums[f] != 0 {
            nums[s], nums[f] = nums[f], nums[s]
            s++
        }
    }
}
```

#### 608. Two Sum - Input array is sorted

Given an array of integers that is already_sorted in ascending order_, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers \(both index1 and index2\) are not zero-based.

##### Notice

You may assume that each input would have exactly one solution.

Have you met this question in a real interview?

Yes

**Example**

Given nums =`[2, 7, 11, 15]`, target =`9`  
return`[1, 2]`

[http://www.lintcode.com/en/problem/two-sum-input-array-is-sorted/](http://www.lintcode.com/en/problem/two-sum-input-array-is-sorted/)

### 解题分析:

相向指针

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an array of Integer
     * @param target: target = nums[index1] + nums[index2]
     * @return: [index1 + 1, index2 + 1] (index1 < index2)
     */
    vector<int> twoSum(vector<int> &nums, int target) {
        // write your code here
        vector<int> res(2,-1);
        if (nums.empty())
            return res;
        int beg =0, end = nums.size()-1;
        while(beg != end)
        {
            int sum = nums[beg] + nums[end];
            if (sum == target)
            {
                res[0] = beg+1;
                res[1] = end+1;
                return res;
            }
            else if (sum < target)
                beg++;
            else
                end--;
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)

2020/08/03

```go
func twoSum(numbers []int, target int) []int {
    res := make([]int, 2)
    b, e := 0, len(numbers) - 1
    for b < e {
        s := numbers[b] + numbers[e]
        if s < target {
            b++
        } else if s > target{
            e--
        } else {
            break
        }
    }
    res[0] = b + 1
    res[1] = e + 1
    return res
}
```

#### 57. 3Sum

Given an array_S\_of n integers, are there elements\_a_,_b_,\_c\_in\_S\_such that`a + b + c = 0`? Find all unique triplets in the array which gives the sum of zero.

##### Notice

Elements in a triplet \(a,b,c\) must be in non-descending order. \(ie, a ≤ b ≤ c\)

The solution set must not contain duplicate triplets.

Have you met this question in a real interview?

Yes

**Example**

For example, given array S =`{-1 0 1 2 -1 -4}`, A solution set is:

```
(-1, 0, 1)
(-1, -1, 2)
```

[http://www.lintcode.com/en/problem/3sum/\#](http://www.lintcode.com/en/problem/3sum/#)

[https://leetcode.com/problems/3sum/](https://leetcode.com/problems/3sum/)

### 解题分析:

去重的金句，只允许第一次出现被搜索

```cpp
                if (beg > i+1 && numbers[beg-1] == numbers[beg])
                {
                    beg++; continue;
                }
```

### 代码：

```cpp
class Solution {
public:
    /**
     * @param numbers: Give an array numbers of n integer
     * @return: Find all unique triplets in the array which gives the sum of zero.
     */
    vector<vector<int>> threeSum(vector<int> &numbers) {
        // write your code here
        vector<vector<int>> res;
        if (numbers.size() < 3)
            return res;
        sort(numbers.begin(), numbers.end());
        int i = 0;
        while( i < numbers.size() - 2)
        {
            if (i>0 && numbers[i] == numbers[i-1])
            {
                i++;
                continue;
            }
            int beg = i+1, end =numbers.size()-1;
            int target = - numbers[i];
            while(beg < end)
            {
                if (beg > i+1 && numbers[beg-1] == numbers[beg])
                {
                    beg++; continue;
                }
                int sum = numbers[beg] + numbers[end];
                if (sum == target)
                {
                    res.push_back(vector<int>{numbers[i], numbers[beg], numbers[end]});
                    beg++; end--;
                }
                else if (sum < target)
                    beg++;
                else
                    end--;
            }
            while(i+1 <numbers.size()-2 && numbers[i] == numbers[i+1])
                i++;
            i++;
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n^2\)

2020/08/04

```go
import "sort"
func threeSum(nums []int) [][]int {
    sort.Ints(nums)
    res := make([][]int, 0)
    for i := 0; i < len(nums) - 2; i++ {
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }
        b, e := i + 1, len(nums) - 1;
        for b < e {
            if b > i + 1&& nums[b] == nums[b-1] {
                b++
                continue
            }
            t := nums[i] + nums[b] + nums[e]
            if t < 0 {
                b++
            } else if t > 0{
                e--
            } else {
                res = append(res, []int{nums[i], nums[b], nums[e]})
                b++
            }
        }
    }
    return res
}
```

#### 31. Partition Array

Given an array`nums`of integers and an int`k`, partition the array \(i.e move the elements in "nums"\) such that:

* All elements 
  &lt;
  _k_
  are moved to the
  _left_
* All elements 
  &gt;
  =
  _k_
  are moved to the
  _right_

Return the partitioning index, i.e the first index_i\_nums\[\_i_\] &gt;=_k_.

##### Notice

You should do really partition in array\_nums\_instead of just counting the numbers of integers smaller than k.

If all elements in_nums\_are smaller than\_k_, then return_nums.length_

Have you met this question in a real interview?

Yes

**Example**

If nums =`[3,2,2,1]`and`k=2`, a valid answer is`1`.

[http://www.lintcode.com/en/problem/partition-array/\#](http://www.lintcode.com/en/problem/partition-array/#)

### 解题分析:

相向指针， 基本功题目

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: The integer array you should partition
     * @param k: An integer
     * @return: The index after partition
     */
    int partitionArray(vector<int> &nums, int k) {
        // write your code here
        int beg = 0, end = nums.size()-1;
        while( beg <= end)
        {
            while(beg <= end && nums[beg] < k) beg++;
            while(beg <= end && nums[end] >=k) end--;
            if (beg<=end) swap(nums[beg++], nums[end--]);
        }
        return beg;
    }
};
```

### 复杂度分析:

o\(n\)

2020/08/04

```go
/**
 * @param nums: The integer array you should partition
 * @param k: An integer
 * @return: The index after partition
 */
func partitionArray (nums []int, k int) int {
    // write your code here
    if len(nums) <= 1 {
        return 0
    }
    s := 0
    for f := 0; f < len(nums); f++ {
        if nums[f] >= k {
            continue
        }
        nums[s], nums[f] = nums[f], nums[s]
        s++
    } 
    return s
}
```

#### 609. Two Sum - Less than or equal to target

Given an array of integers, find how many pairs in the array such that their sum is`less than or equal to`a specific target number. Please return the number of pairs.

Have you met this question in a real interview?

Yes

**Example**

Given nums =`[2, 7, 11, 15]`, target =`24`.  
Return`5`.  
2 + 7 &lt; 24  
2 + 11 &lt; 24  
2 + 15 &lt; 24  
7 + 11 &lt; 24  
7 + 15 &lt; 25

[http://www.lintcode.com/en/problem/two-sum-less-than-or-equal-to-target/\#](http://www.lintcode.com/en/problem/two-sum-less-than-or-equal-to-target/#)

解题分析:

nlog\(n\)

用了一个逻辑：

beg最小，end最大， 如果 sum&gt;target,那么end--直到找到第一个 nums\[beg\]+nums\[end\] &lt;= target. 这个时候end不会变了， 因为下一个beg要++,结果只会变大，我们只需要从上次的end开始继续搜索即可。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an array of integer
     * @param target: an integer
     * @return: an integer
     */
    int twoSum5(vector<int> &nums, int target) {
        // write your code here
        if (nums.empty())
            return 0;
        sort(nums.begin(), nums.end());
        int res = 0;
        int beg = 0, end = nums.size()-1;
        while(beg < end)
        {
            int sum = nums[beg]+nums[end];
            if (sum > target)
                end--;
            else
            {
                res += end-beg;
                beg++;
            }
        }
        return res;
    }
};
```

### 复杂度分析:

nlog\(n\)

#### 587. Two Sum - Unique pairs

Given an array of integers, find how many`unique pairs`in the array such that their sum is equal to a specific target number. Please return the number of pairs.

Have you met this question in a real interview?

Yes

**Example**

Given nums =`[1,1,2,45,46,46]`, target =`47`  
return`2`

1 + 46 = 47  
2 + 45 = 47

[http://www.lintcode.com/en/problem/two-sum-unique-pairs/](http://www.lintcode.com/en/problem/two-sum-unique-pairs/)

### 解题分析:

换了一个思路，用map来做， 和经典的two sum一个思路， 一遍找一遍插。 但是因为要找unique的，加一个flag,只要用过了就不再用了， 所以：

找到pair, 把两个都设为用过了，下次这两个数都不会被用到

找不到， 如果这个数已经有了且被用过了，不做任何处理，否则加一个新的并mark为可以被用到。 O（n\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an array of integer
     * @param target: An integer
     * @return: An integer
     */
    int twoSum6(vector<int> &nums, int target) {
        // write your code here
        int res = 0;
        unordered_map<int, bool /*not used*/> counter;
        for (int i = 0; i < nums.size(); i++)
        {
            auto it = counter.find(target-nums[i]);
            if ( it != counter.end() && it->second)
            {
                res++;
                it->second = false;
                counter[nums[i]] = false;
                continue;
            }

            if (counter.find(nums[i]) != counter.end() && !counter[nums[i]])
                continue;
            counter[nums[i]] = true;
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)

当然nlog\(n\)的解法也可以

```cpp
class Solution {
public:
    /**
     * @param nums: an array of integer
     * @param target: An integer
     * @return: An integer
     */
    int twoSum6(vector<int> &nums, int target) {
        // write your code here
        if (nums.empty())
            return 0;
        sort(nums.begin(), nums.end());
        int beg = 0, end = nums.size()-1;
        int res=0;
        while(beg < end)
        {
            if (beg != 0 & nums[beg-1] == nums[beg])
            {
                beg++;
                continue;
            }
            int sum = nums[beg] + nums[end];
            if (sum == target)
            {
                res++;
                beg++;
            }
            else if (sum < target)
            {
                beg++;
            }
            else
            {
                end--;
            }

        }
        return res;
    }
};
```

#### 533. Two Sum - Closest to target

Given an array`nums`of_n\_integers, find two integers in\_nums\_such that the sum is closest to a given number,\_target_.

Return the difference between the sum of the two integers and the target.

Have you met this question in a real interview?

Yes

**Example**

Given array`nums`=`[-1, 2, 1, -4]`, and_target_=`4`.

The minimum difference is`1`. \(4 - \(2 + 1\) = 1\).

[http://www.lintcode.com/en/problem/two-sum-closest-to-target/](http://www.lintcode.com/en/problem/two-sum-closest-to-target/)

### 解题分析:

逼近法， 排序后逼近， 如果diff &lt;0 那么就要往diff&gt;0走， 反之亦依然，这样保证我们始终在0附近徘徊

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an integer array
     * @param target: An integer
     * @return: the difference between the sum and the target
     */
    int twoSumClosest(vector<int> &nums, int target) {
        // write your code here
        if (nums.size() < 2)
            return INT_MAX;
        sort(nums.begin(), nums.end());
        int beg = 0, end = nums.size()-1;
        int res = INT_MAX;
        while(beg < end)
        {
            int val = target - (nums[beg]+ nums[end]);
            res = min(res, abs(val));
            if (val > 0)
                beg++;
            else 
                end--;
        }
        return res; 
    }
};
```

### 复杂度分析:

nlog\(n\)

#### 443. Two Sum - Greater than target

Given an array of integers, find how many pairs in the array such that their sum is bigger than a specific target number. Please return the number of pairs.

Have you met this question in a real interview?

Yes

**Example**

Given numbers =`[2, 7, 11, 15]`, target =`24`. Return`1`. \(11 + 15 is the only pair\)

[http://www.lintcode.com/en/problem/two-sum-greater-than-target/\#](http://www.lintcode.com/en/problem/two-sum-greater-than-target/#)

### 解题分析:

还是求pair得问题， 左右移动搜索咯。。。greater, less total, 固定一个统计，更新下一个指针使得和变化继续搜索。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an integer array
     * @param target: An integer
     * @return: the difference between the sum and the target
     */
    int twoSumClosest(vector<int> &nums, int target) {
        // write your code here
        if (nums.size() < 2)
            return INT_MAX;
        sort(nums.begin(), nums.end());
        int beg = 0, end = nums.size()-1;
        int res = INT_MAX;
        while(beg < end)
        {
            int val = target - (nums[beg]+ nums[end]);
            res = min(res, abs(val));
            if (val > 0)
                beg++;
            else 
                end--;
        }
        return res; 
    }
};
```

### 复杂度分析:

nlog\(n\)

#### 461. Kth Smallest Numbers in Unsorted Array

Find the kth smallest numbers in an unsorted integer array.

Have you met this question in a real interview?

Yes

**Example**

Given`[3, 4, 1, 2, 5]`, k =`3`, the 3rd smallest numbers are`[1, 2, 3]`.

[http://www.lintcode.com/en/problem/kth-smallest-numbers-in-unsorted-array/](http://www.lintcode.com/en/problem/kth-smallest-numbers-in-unsorted-array/)

### 解题分析:

quick select, 花O\(n\)的时间把问题变成T（n/2）, o\(n\)

代码：

```cpp
class Solution {
public:
    /**
     * @param k: An integer
     * @param nums: An integer array
     * @return: kth smallest element
     */
    int kthSmallest(int k, vector<int> &nums) {
        // write your code here
        if (nums.size() < k || k <= 0)
            return INT_MAX;
        return quickSelect(nums,0, nums.size()-1, k);
    }

    int quickSelect(vector<int>& nums, int beg, int end, int k)
    {
        if (end <= beg)
            return nums[end];
        int pivot = nums[beg + (end - beg) /2] ;
        int i = beg, j = end;
        while(i <= j)
        {
            while(i<=j && nums[i] < pivot)
                i++;
            while(i<=j && nums[j] > pivot)
                j--;
            if (i <= j)
                swap(nums[i++], nums[j--]);
        }
        // compare k 
        if (beg + k - 1 <= j)
            return quickSelect(nums, beg, j, k);
        if (beg + k - 1 >= i)
            return quickSelect(nums, i, end, k-i + beg);
        return nums[j+1];
    }
};
```

### 复杂度分析:

o\(n\)

#### 382. Triangle Count

Given an array of integers, how many three numbers can be found in the array, so that we can build an triangle whose three edges length is the three numbers that we find?

Have you met this question in a real interview?

Yes

**Example**

Given array S =`[3,4,6,7]`, return`3`. They are:

```
[3,4,6]
[3,6,7]
[4,6,7]
```

Given array S =`[4,4,4,4]`, return`4`. They are:

```
[4(1),4(2),4(3)]
[4(1),4(2),4(4)]
[4(1),4(3),4(4)]
[4(2),4(3),4(4)]
```

[http://www.lintcode.com/en/problem/triangle-count/\#](http://www.lintcode.com/en/problem/triangle-count/#)

### 解题分析:

求A+B&gt;c的组合， 用上面一道题的结论，只是固定最大端，扫完一次后，移动最大端，再次扫描

代码：

```cpp
class Solution {
public:
    /**
     * @param S: A list of integers
     * @return: An integer
     */
    int triangleCount(vector<int> &S) {
        // write your code here
        if (S.size() < 3)
            return 0;
        sort(S.begin(), S.end());
        int end = S.size()-1;
        int count = 0;
        while(end >= 2)
        {
            int i = 0, j=end-1;
            while(i < j)
            {
                int val = S[i] + S[j];
                if (val <= S[end])
                    i++;
                else
                {
                    count += j-i;
                    j--;
                }
            }
            end--;
        }
        return count;
    }
};
```

### 复杂度分析:

o\(n^2\)

2020/08/06

```go
/**
 * @param S: A list of integers
 * @return: An integer
 */
import "sort"
func triangleCount (S []int) int {
    // write your code here
    n := len(S)
    if n < 3 {
        return 0
    }
    c := 0
    sort.Ints(S)
    for end := n-1; end >=2; end--{
        b, e := 0, end -1
        for b < e {
            s := S[b] + S[e]
            if s <= S[end] {
                b++
            } else {
                c += e - b
                e--
            }
        }
    }
    return c
}

```




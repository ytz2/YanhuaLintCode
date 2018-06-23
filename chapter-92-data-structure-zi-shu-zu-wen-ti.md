![](/assets/subarray.png)



## 138. Subarray Sum

Given an integer array, find a subarray where the sum of numbers is**zero**. Your code should return the index of the first number and the index of the last number.

### Example

Given`[-3, 1, 2, -3, 4]`, return`[0, 2]`or`[1, 3]`.

https://www.lintcode.com/problem/subarray-sum/description

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




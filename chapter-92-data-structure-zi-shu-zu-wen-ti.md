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

https://www.lintcode.com/problem/subarray-sum-closest/description



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
        if (nums.size()<=1)
            return vector<int>{0,0};
        vector<pair<int,int>> sumarr{make_pair(0,0)};
        int sum = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            sum += nums[i];
            sumarr.push_back(make_pair(sum, i+1));
        }
        sort(sumarr.begin(), sumarr.end(),[](const pair<int,int>& left, const pair<int, int>&right){
            return left.first < right.first;
        });
        int minInd = 0, minVal = INT_MAX;
        for (int i = 1; i < sumarr.size(); i++)
        {
            int diff = abs(sumarr[i].first - sumarr[i-1].first);
            if (diff < minVal)
            {
                minInd = i;
                minVal = diff;
            }
        }
        int ind1 = sumarr[minInd].second , ind2 =  sumarr[minInd-1].second;
        return ind1 > ind2?vector<int>{ind2, ind1-1} : vector<int>{ind1, ind2-1};
    }
};
```




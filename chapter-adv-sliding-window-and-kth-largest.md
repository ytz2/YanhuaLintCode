## 第一部分

当给定一个**一维数组**，求**子序**列**满足条件**的**最**大最小最短中最长的问题， 使用这个方法需要证明j不需要往回重置到i. 

一般跑几个例子就可以看出来

![](/assets/slidewindowtemplate.png)



## 

## 406. Minimum Size Subarray Sum

Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return -1 instead.

### Example

Given the array`[2,3,1,2,4,3]`and s =`7`, the subarray`[4,3]`has the minimal length under the problem constraint.

### Challenge

If you have figured out the O\(n\) solution, try coding another solution of which the time complexity is O\(n log n\).

**Note:**  
跑一个暴力的例子

for i-n

  for j -n

       if sum &gt;= target

          break



i = 0:  2, 5, 6, 8 j = 3 

i = 1:  3, 4, 6 j = 3 j不需要动， 因为j以前的已经被上次遍历证明小于target了。  那么就可以用滑动窗口了。



```cpp
class Solution {
public:
    /**
     * @param nums: an array of integers
     * @param s: An integer
     * @return: an integer representing the minimum size of subarray
     */
    int minimumSize(vector<int> &nums, int s) {
        // write your code here
        int sum = 0;
        int len = INT_MAX;
        int j = 0;
        int n = nums.size();
        for (int i = 0; i < n; i++) // start of window
        {
            while(j < n) // no need to go back 
            {
                if (sum >= s) // if condition break
                    break;
                else // update local window
                    sum += nums[j++];
            }
            // update state of i; 
            if (sum >= s) 
                len = min(len, j-i);
            sum -= nums[i];
        }
        return len == INT_MAX? -1:len;
    }
};
```




## 81. Find Median from Data Stream

Numbers keep coming, return the median of numbers at every time a new number added.

### Example

For numbers coming list:`[1, 2, 3, 4, 5]`, return`[1, 1, 2, 2, 3]`.

For numbers coming list:`[4, 5, 1, 3, 2, 6, 0]`, return`[4, 4, 4, 3, 3, 3, 3]`.

For numbers coming list:`[2, 20, 100]`, return`[2, 2, 20]`.

### Challenge

Total run time in O\(_nlogn_\).

https://www.lintcode.com/problem/find-median-from-data-stream/description

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: the median of numbers
     */
    vector<int> medianII(vector<int> &nums) {
        // write your code here
        vector<int> results;
        // corner case
        if (nums.empty())
            return results;
        priority_queue<int> left; 
        priority_queue<int, vector<int>, std::greater<int>> right;
        
        left.push(nums[0]);
        results.push_back(nums[0]);
        for (int i = 1; i < nums.size(); i++){
            if (nums[i] < left.top())
                left.push(nums[i]);
            else
                right.push(nums[i]);
            
            if (left.size() < right.size())
            {
                left.push(right.top());
                right.pop();
            }
            if (left.size()-2 == right.size())
            {
                right.push(left.top());
                left.pop();
            }
            results.push_back(left.top());
        }
        return results;
    }
};
```




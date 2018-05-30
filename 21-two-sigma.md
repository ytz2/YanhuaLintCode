## 81. Find Median from Data Stream

Numbers keep coming, return the median of numbers at every time a new number added.

### Example

For numbers coming list:`[1, 2, 3, 4, 5]`, return`[1, 1, 2, 2, 3]`.

For numbers coming list:`[4, 5, 1, 3, 2, 6, 0]`, return`[4, 4, 4, 3, 3, 3, 3]`.

For numbers coming list:`[2, 20, 100]`, return`[2, 2, 20]`.

### Challenge

Total run time in O\(_nlogn_\).

[https://www.lintcode.com/problem/find-median-from-data-stream/description](https://www.lintcode.com/problem/find-median-from-data-stream/description)

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

## Random Weighted

Design a data structure to hold objects with a corresponding integer weight. It should

support:

Obtain an object randomly with probability equal to

\(weight of the element\) / \(sum of the weights\).

Set an object-weight pair. If the object is already in the structure, its weight will

be updated. Otherwise, the object will be inserted and set to its weight. If the weight

is zero, the object can be removed.

### 代码：

```cpp
/******************************************************************************

                              Online C++ Compiler.
               Code, Compile, Run and Debug C++ program online.
Write your code in this editor and press "Run" button to compile and execute it.

*******************************************************************************/

#include <iostream>
#include <stdlib.h>
#include <unordered_map>
using namespace std;

template <class T>
class Weighted{
public:
  Weighted(){
      total = 0;
  }  

  T getRandom(){
      int r = std::rand() % total;
      for (const auto& pair : store)
      {
          r -= pair.second;
          if (r < 0)
            return pair.first;
      }
      return 0;
  }

  void setRandom(T obj, int weight){
    auto it = store.find(obj);
    if (it == store.end())
    {
        total += weight;
        store[obj] = weight;
    }
    else
    {
        total += weight - store[obj];
        store[obj] = weight;
    }
  }

private:
 int total;
 unordered_map<int, T> store;
};

int main()
{
    Weighted<int> test;
    test.setRandom(0, 3);
    test.setRandom(1, 27);
    double count = 0;
    for (int i = 0; i < 10000; i++)
    {
        int res = test.getRandom();
        if (res == 1)
            count += 1;
    }

    cout<<count/float(10000)<<endl;

    return 0;
}
```



## 342 Power of Four

Given an integer \(signed 32 bits\), write a function to check whether it is a power of 4.

**Example:**  
Given num = 16, return true. Given num = 5, return false.

**Follow up**: Could you solve it without loops/recursion?

### 代码：

```cpp
class Solution {
public:
    bool isPowerOfFour(int num) {
        if (num <= 0)
            return false;
        // p of two
        if ((num & num-1) != 0)
            return false;
        // p of 4
        // 10101010101 -> 0x55555555
        return (num & 0x55555555) == num; 
    }
};
```




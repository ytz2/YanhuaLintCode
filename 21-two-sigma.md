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

class Solution {
public:
    bool isPowerOfFour(int num) {
        while (num && (num % 4 == 0)) {
            num /= 4;
        }
        return num == 1;
    }
};
```

## 121 Best Time to Buy and Sell Stock

Say you have an array for which the_i\_thelement is the price of a given stock on day\_i_.

If you were only permitted to complete at most one transaction \(i.e., buy one and sell one share of the stock\), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example 1:**

```
Input:
 [7,1,5,3,6,4]

Output:
 5

Explanation:
 Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

**Example 2:**

```
Input:
 [7,6,4,3,1]

Output:
 0

Explanation:
 In this case, no transaction is done, i.e. max profit = 0.
```

### 代码：

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() <= 1)
            return 0;
        int minP = prices[0];
        int profit = 0;
        for (int i = 0; i<prices.size(); i++)
        {
            minP = min(prices[i], minP);
            profit = max(profit, prices[i]-minP);
        }
        return profit;
    }
};

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        if (n<=1)
            return 0;
        int total=0;

        
        for (int i=0; i<n; i++){
            int myMin=0; 
            int myMax=0;
            for (; i<n-1 && prices[i]>prices[i+1]; i++);
            myMin=prices[i];
            for (; i<n-1 && prices[i]<prices[i+1]; i++);
            myMax=prices[i];
            total+= myMax-myMin;
        }
        return total;
    }
};
```

```
// C/C++ program for maximum contiguous circular sum problem
#include<stdio.h>

// Standard Kadane's algorithm to find maximum subarray
// sum
int kadane(int a[], int n);

// The function returns maximum circular contiguous sum
// in a[]
int maxCircularSum(int a[], int n)
{
// Case 1: get the maximum sum using standard kadane'
// s algorithm
int max_kadane = kadane(a, n);

// Case 2: Now find the maximum sum that includes
// corner elements.
int max_wrap = 0, i;
for (i=0; i<n; i++)
{
        max_wrap += a[i]; // Calculate array-sum
        a[i] = -a[i]; // invert the array (change sign)
}

// max sum with corner elements will be:
// array-sum - (-max subarray sum of inverted array)
max_wrap = max_wrap + kadane(a, n);

// The maximum circular sum will be maximum of two sums
return (max_wrap > max_kadane)? max_wrap: max_kadane;
}

// Standard Kadane's algorithm to find maximum subarray sum
// See https://www.geeksforgeeks.org/archives/576 for details
int kadane(int a[], int n)
{
    int max_so_far = 0, max_ending_here = 0;
    int i;
    for (i = 0; i < n; i++)
    {
        max_ending_here = max_ending_here + a[i];
        if (max_ending_here < 0)
            max_ending_here = 0;
        if (max_so_far < max_ending_here)
            max_so_far = max_ending_here;
    }
    return max_so_far;
}

/* Driver program to test maxCircularSum() */
int main()
{
    int a[] = {11, 10, -20, 5, -3, -5, 8, -13, 10};
    int n = sizeof(a)/sizeof(a[0]);
    printf("Maximum circular sum is %dn",
                            maxCircularSum(a, n));
    return 0;
}
```

## 192 Wildcard Matching

Implement wildcard pattern matching with support for`'?'`and`'*'`.

* `'?'`
  Matches any single character.
* `'*'`
  Matches any sequence of characters \(including the empty sequence\).

The matching should cover the entire input string \(not partial\).

### Example

```
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: A string 
     * @param p: A string includes "?" and "*"
     * @return: is Match?
     */
    bool isMatch(string &s, string &p) {
        // write your code here
        if (p.empty())
            return s.empty();
        vector<vector<bool>> dp(s.size()+1, vector<bool>(p.size()+1, false));
        dp[0][0] = true;
        for (int j = 1; j <= p.size(); j++)
            dp[0][j] = dp[0][j-1] && p[j-1] == '*';
        for (int i = 1; i <= s.size(); i++)
        {
            for (int j = 1; j <= p.size(); j++)
            {
                if (p[j-1] == '*')
                    dp[i][j] = dp[i][j-1] || dp[i-1][j]; // ignore i or ignore j
                else if (p[j-1] == '?' || s[i-1] == p[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = false;
            }
        }
        return dp[s.size()][p.size()];
    }
};
```

## LRU

```cpp
#include <list>
class LRUCache {
public:

    struct Node{
        int key;
        int val;
        Node( int k, int v)
            : key(k), val(v)
            {}
    };
    /*
    * @param capacity: An integer
    */LRUCache(int capacity) {
        // do intialization if necessary
        capacity_=capacity;
    }

    /*
     * @param key: An integer
     * @return: An integer
     */
    int get(int key) {
        // write your code here
        auto it = map_.find(key);
        if (it == map_.end())
            return -1;
        int res = (*map_[key])->val;
        moveToHead(key);
        return res;
    }

    /*
     * @param key: An integer
     * @param value: An integer
     * @return: nothing
     */
    void set(int key, int value) {
        // write your code here
        if (map_.find(key) != map_.end())
        {
            (*map_[key])->val = value;
            moveToHead(key);
            return;
        }

        auto node = new Node(key, value);
        lru_.push_front(node);
        map_[key] = lru_.begin();
        if (lru_.size() > capacity_)
        {
            auto v = lru_.back();
            lru_.pop_back();
            map_.erase(v->key);
            delete v;
        }
    }

private:
    int capacity_;
    list<Node*> lru_;
    unordered_map<int, list<Node*>::iterator> map_;
private:
    void moveToHead(int key)
    {
        if (map_.find(key) == map_.end())
            return;
        auto val = *map_[key];
        lru_.erase(map_[key]);
        lru_.push_front(val);
        map_[key] = lru_.begin();
    }
};
```




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

[https://leetcode.com/problems/minimum-size-subarray-sum/submissions/](https://leetcode.com/problems/minimum-size-subarray-sum/submissions/)

**Note:**  
跑一个暴力的例子

for i-n

for j -n

```
   if sum &gt;= target

      break
```

i = 0:  2, 5, 6, 8 j = 3

i = 1:  3, 4, 6 j = 3 j不需要动， 因为j以前的已经被上次遍历证明小于target了。  那么就可以用滑动窗口了。

```cpp
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int res = INT_MAX;
        int sum = 0, j = 0;
        for (int i = 0; i < nums.size(); i++) {
            while(j < nums.size()) {
                if (sum >= s) break;
                else {
                    sum += nums[j++];
                }
            }
            if (sum >= s) res = min(res, j-i);
            sum -= nums[i];
        }
        return res == INT_MAX? 0:res;
    }
};
```

## 384. Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

### Example

For example, the longest substring without repeating letters for`"abcabcbb"`is`"abc"`, which the length is`3`.

For`"bbbbb"`the longest substring is`"b"`, with the length of`1`.

### Challenge

O\(n\) time

还是j不需要再回头，且满足，substring, condition :no repeating, longest

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0;
        vector<int> buff(256, 0);
        auto scan = [&](char c){
            return buff[c] == 0;
        };
        int j = 0, n = s.size();
        for (int i = 0; i < n; i++) {
            while(j < n) {
                if (scan(s[j])) {
                    buff[s[j++]]++;
                } else {
                    break;
                }
            }
            res = max(res, j - i);
            buff[s[i]]--;
        }

        return res;
    }
};
```

## 32. Minimum Window Substring

Given a string source and a string target, find the minimum window in source which will contain all the characters in target.

### Example

For source =`"ADOBECODEBANC"`, target =`"ABC"`, the minimum window is`"BANC"`

[https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/)

### Challenge

Can you do it in time complexity O\(n\) ?

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        vector<int> buff(256, 0);
        vector<int> target(256, 0);
        for (auto c : t) target[c]++;
        auto check = [&]() {
            for (int i = 0; i < 256; i++) {
                if (buff[i] < target[i]) return false;
            }
            return true;
        };
        int j = 0, n = s.size();
        int start = -1;
        int len = INT_MAX;
        for (int i = 0; i < n; i++) {
            while(j < n) {
                if (check()) break;
                else buff[s[j++]]++;
            }
            if (check() && j-i < len) {
                start = i;
                len = j - i;
            }  
            buff[s[i]]--;
        }
        return start == -1 ? "" : s.substr(start, len);
    }
};
```

## 386. Longest Substring with At Most K Distinct Characters

Given a string_s_, find the length of the longest substring T that contains at most k distinct characters.

### Example

For example, Given s =`"eceba"`,`k = 3`,

T is`"eceb"`which its length is`4`.

[https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

### Challenge

O\(n\), n is the size of the string_s_.

这里面有个大坑.用字典来维护size k, 但是注意不能用一旦字典size == k就退出， 因为cccc可以连续，如果c已经在字典里，则可以继续。排除这种情况后dict.size\(\) == k退出， 否则继续更新。

```cpp
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        if (k == 0 || s.empty()) return 0;
        int count = 0;
        vector<int> counter(256, 0);
        int res = 0;
        int j = 0;
        for (int i = 0; i < s.size(); i++) {
            while( j < s.size()) {
                if (count <= k) {
                    res = max(res, j - i);
                    count += counter[s[j]] == 0;
                    counter[s[j++]]++;
                } else {
                    break;
                }
            }
            if (count <= k) res = max(res, j - i);
            counter[s[i]]--;
            count -= counter[s[i]] == 0;
        }
        return res;
    }
};
```

## ![](/assets/heapfortopk.png)

## 401. Kth Smallest Number in Sorted Matrix

Find the\_k\_th smallest number in at row and column sorted matrix.

### Example

Given k =`4`and a matrix:

```
[
  [1 ,5 ,7],
  [3 ,7 ,8],
  [4 ,8 ,9],
]
```

return`5`

### Challenge

Solve it in O\(k log n\) time where n is the bigger one between row size and column size.

这也算一类题目了， 用的思想是top k 就上pq, 碰到不确定的备选答案就丢进去，因为他始终给你维护一个最优的答案

用pq做BFS的辅助找第k大，是多选情况下的应用

```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        const static vector<int> dx{0, 1};
        const static vector<int> dy{1, 0};
        auto cmp=[&](int i, int j) {
            return matrix[i/n][i%n] > matrix[j/n][j%n];
        };
        priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
        pq.push(0);
        visited[0][0] = true;
        int count = 0;
        while(!pq.empty()) {
            auto ind = pq.top();
            pq.pop();
            count++;
            if (count == k) {
                return matrix[ind/n][ind%n];
            }
            for (int i = 0; i < 2; i++) {
                auto nx = ind / n + dx[i];
                auto ny = ind % n + dy[i];
                if (nx < 0 || nx >= n || ny < 0 || ny >= n || visited[nx][ny])
                    continue;
                visited[nx][ny] = true;
                pq.push(nx*n + ny);
            }
        }
        return 0;
    }
};
```

## 373. Find K Pairs with Smallest Sums

You are given two integer arrays**nums1**and**nums2**sorted in ascending order and an integer**k**.

Define a pair**\(u,v\)**which consists of one element from the first array and one element from the second array.

Find the k pairs**\(u1,v1\),\(u2,v2\) ...\(uk,vk\)**with the smallest sums.

**Example 1:**

```
Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

Return: [1,2],[1,4],[1,6]

The first 3 pairs are returned from the sequence:
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**Example 2:**

```
Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

Return: [1,1],[1,1]

The first 2 pairs are returned from the sequence:
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**Example 3:**

```
Given nums1 = [1,2], nums2 = [3],  k = 3 

Return: [1,3],[2,3]

All possible pairs are returned from the sequence:
[1,3],[2,3]
```

return`5`

[https://leetcode.com/problems/find-k-pairs-with-smallest-sums/description/](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/description/)

### Challenge

第K x 的问题， 还是上来想pq,面临多选就网上push,找到为止。

```cpp
class Solution {
public:
    vector<pair<int, int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<pair<int,int>> result;

        auto cmp = [&nums1,&nums2](const pair<int,int>& left, const pair<int,int>& right){
            return nums1[left.first]+nums2[left.second] > nums1[right.first]+nums2[right.second];  
        };


        int m = nums1.size();
        int n = nums2.size();
        if (0 == m || 0 == n)
            return result;
        vector<vector<bool>> visited(m, vector<bool>(n, false));

        priority_queue<pair<int,int>, vector<pair<int,int>>,decltype(cmp)> pq(cmp);

        pq.emplace(0, 0);
        visited[0][0] = true;
        for (int i = 0; i < k && !pq.empty(); i++)
        {
            auto p = pq.top();
            pq.pop();
            result.emplace_back(nums1[p.first], nums2[p.second]);
            int next_num1 = p.first+1;
            int next_num2 = p.second+1;

            if (next_num1 < m && !visited[next_num1][p.second])
            {
                pq.emplace(next_num1, p.second);
                visited[next_num1][p.second] = true;
            }

            if (next_num2 < n && !visited[p.first][next_num2])
            {
                pq.emplace(p.first,next_num2);
                visited[p.first][next_num2] = true;
            }
        }
        return result;
    }
};
```




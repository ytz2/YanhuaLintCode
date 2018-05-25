## 15. Permutations

Given a list of numbers, return all possible permutations.

##### Notice

You can assume that there is no duplicate numbers in the list.

Have you met this question in a real interview?

Yes

**Example**

For nums =`[1,2,3]`, the permutations are:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

[https://www.lintcode.com/en/old/problem/permutations/\#](https://www.lintcode.com/en/old/problem/permutations/#)

### 解题分析:

模板套路类题目

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    vector<vector<int>> permute(vector<int> &nums) {
        // write your code here
        vector<vector<int>> results;
        vector<int> subset;
        vector<bool> visited(nums.size(), false);
        dfs(nums, subset, visited, results);
        return results;
    }

    void  dfs(const vector<int>& nums, vector<int>& subset, vector<bool>& visited, vector<vector<int>>& results)
    {
        if (subset.size() == nums.size())
        {
            results.push_back(subset);
            return;
        }
        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i])
                continue;
            visited[i] = true;
            subset.push_back(nums[i]);
            dfs(nums, subset, visited, results);
            subset.pop_back();
            visited[i] = false;
        }
    }
};
```

### 

## 16. Permutations II

Given a list of numbers with duplicate number in it. Find all**unique**permutations.

Have you met this question in a real interview?

Yes

**Example**

For numbers`[1,2,2]`the unique permutations are:

```
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]
```

[https://www.lintcode.com/en/old/problem/permutations-ii/\#](https://www.lintcode.com/en/old/problem/permutations-ii/#)

### 解题分析:

模板套路类题目, 这里有一个去重的问题

叙述： 当重复出现的时候nums\[i\] == nums\[i-1\]， 且前面已经被visited，才可以继续

### 代码：

```cpp
class Solution {
public:
    /*
     * @param :  A list of integers
     * @return: A list of unique permutations
     */
    vector<vector<int>> permuteUnique(vector<int> &nums) {
        // write your code here
        vector<bool> visited(nums.size(), false);
        vector<int> subset;
        vector<vector<int>> results;
        sort(nums.begin(), nums.end());
        dfs(nums, subset, visited, results);
        return results;
    }

    void dfs(const vector<int>& nums, vector<int>& subset, vector<bool>& visited, vector<vector<int>>& results)
    {
        if (subset.size() == nums.size())
        {
            results.push_back(subset);
            return;
        }

        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i])
                continue;
            if (i != 0 && nums[i] == nums[i-1] && !visited[i-1])
                continue;
            visited[i] = true;
            subset.push_back(nums[i]);
            dfs(nums, subset, visited, results);
            subset.pop_back();
            visited[i] = false;
        }
    }
};
```

## \*\*\*\*\*52. Next Permutation 感觉只能靠背了。。。

Given a list of integers, which denote a permutation.

Find the next permutation in ascending order.

##### Notice

The list may contains duplicate integers.

Have you met this question in a real interview?

Yes

**Example**

For`[1,3,2,3]`, the next permutation is`[1,3,3,2]`

For`[4,3,2,1]`, the next permutation is`[1,2,3,4]`time since it is smaller than the input time numerically.

[https://www.lintcode.com/en/old/problem/next-permutation/](https://www.lintcode.com/en/old/problem/next-permutation/)

### 解题分析:

![](/assets/demo2.png)

![](/assets/demo.png)

这个题目要明白是属于字典顺序

1。 从后面往前找，找到a\[i\] &lt; a\[i+1\]的数字

1. 从后面往前找找到第一个a\[j\] &gt; a\[i\]

2. swap\(a\[i\], a\[j\]

3. reverse\(i+1, end\)

原理：

1。如果是递减，则是最后的字典顺序，无下一个

2。 那么往前找不是递减的， 因为是找字典顺序， 所以下一个当前点必然比现在的要大 （逆字典）

3。大了之后还不够，因为现在是升序

如果上来想不出方法，可以试着找找规律，我们关注的重点应是原数组末尾。

从末尾往左走，如果一直递增，例如`...9,7,5`，那么下一个排列一定会牵扯到左边更多的数，直到一个非递增数为止，例如`...6,9,7,5`。对于原数组的变化就只到`6`这里，和左侧其他数再无关系。`6`这个位置会变成`6`**右侧所有数中比**`6`**大的最小的数**，而`6`会进入最后3个数中，且后3个数必是**升序数组**。

**Q：i为0怎么办？**  
A：i为0说明整个数组是降序的，直接翻转整个数组即可。

**Q：有重复元素怎么办？**  
A：在遍历时只要严格满足`nums[i] > nums[i - 1]`和`nums[j] > nums[i - 1]`就不会有问题。

**Q：元素过少是否要单独考虑？**  
A：当元素个数小于等于1个时，可以直接返回。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A list of integers
     */
    vector<int> nextPermutation(vector<int> &nums) {
        // write your code here
        if (nums.size() <= 1)
            return nums;
        int i = nums.size() - 2;
        //step 1
        while(i >=0 && nums[i] >= nums[i+1])
            i--;
        // step 2
        if (i >= 0)
        {
            //step 3
            int j = nums.size()-1;
            while(j >= 0 && nums[j] <= nums[i])
                j--;
            swap(nums[i], nums[j]);
        }
        //step 4
        reverse(nums.begin()+i+1, nums.end());
        return nums;
    }
};
```

## \*\*\*\*\*190. Next Permutation II

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).

Have you met this question in a real interview?

Yes

**Example**

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

`1,2,3`→`1,3,2`

`3,2,1`→`1,2,3`

`1,1,5`→`1,5,1`

[https://www.lintcode.com/en/old/problem/next-permutation-ii/](https://www.lintcode.com/en/old/problem/next-permutation-ii/)

### 解题分析:

和上面一道题一样的。。。无聊

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: An array of integers
     * @return: nothing
     */
    void nextPermutation(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if (n <= 1)
            return;
        int i = n - 2;
        while(i >=0 && nums[i] >= nums[i+1])
            i--;
        if (i>=0)
        {
            int j = n-1;
            while(j>=0 && nums[j] <= nums[i])
                j--;
            swap(nums[i], nums[j]);
        }
        reverse(nums.begin()+i+1, nums.end());
    }
};
```

## \*\*\*\*197. Permutation Index

Given a permutation which contains no repeated number, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.

Have you met this question in a real interview?

Yes

**Example**

Given \[1,2,4\], return 1.

[https://www.lintcode.com/en/old/problem/permutation-index/](https://www.lintcode.com/en/old/problem/permutation-index/)

### 解题分析:

3， 7， 4， 9， 1

从倒数第二位开始， 比9小的 1，  3 7 4 9 1 -》 3 7 4 9 \* -》 1！

从倒数第三位开始， 比4小的， 1   3 7 1 XX -&gt;  2!

从倒数第四位开始    比7 小的        3 4/1 xxx  2\*3!

从倒数第五位开始 比3 小的 1 xxxx  4!

4!+2\*3! +2! +1 = 39 , 前面有39 个， 加上1 第40个

```
用permutation表示当前阶乘，初始化为1,result表示最终结果，初始化为0。由于最终结果可能巨大，所以用long类型。
i从右往左遍历A，循环中计算A[i]右侧有多少元素比A[i]小，计为smaller，result += smaller * permutation。之后permutation *= A.length - i，为下次循环i左移一位后的排列数。
已算出多少字典序在A之前，返回result+1。
```

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: An array of integers
     * @return: A long integer
     */
    long long permutationIndex(vector<int> &A) {
        // write your code here
        int n = A.size();
        if (n == 0)
            return 0;
        if (n == 1)
            return 1;
        long long result = 1;
        long long factor = 1;
        for (int i = n-2; i >= 0; i--)
        {
            int count = 0;
            for (int j = i+1; j < n; j++)
            {
                if (A[j] < A[i])
                    count++;
            }
            result += count * factor;
            factor *= n - i;
        }
        return result;
    }
};
```

## \*\*\*\*198 Permutation Index II

Given a permutation which may contain repeated numbers, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.

[https://www.lintcode.com/en/old/problem/permutation-index-ii/](https://www.lintcode.com/en/old/problem/permutation-index-ii/)

### 解题分析:

和上面一样， 但是因为有了重复，所以求完之后要除以重复数的阶乘。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: An array of integers
     * @return: A long integer
     */
    long long permutationIndexII(vector<int> &A) {
        // write your code here
        int n = A.size();
        if (n == 0)
            return 0;
        if (n == 1)
            return 1;
        long long res = 0;
        long long factor = 1;
        long long multifact = 1;
        unordered_map<int, int> counter;

        for (int i = n-1; i >= 0; i--)
        {
            counter[A[i]]++;
            multifact *= counter[A[i]];
            int count = 0;
            for (int j = i+1; j<n; j++)
            {
                if (A[j] < A[i])
                    count++;
            }
            res += count*factor/multifact;
            factor *= n - i;
        }
        return res+1;
    }
};
```

## 862 Next Closest Time

Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.

You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.

Have you met this question in a real interview?

Yes

**Example**

Given time =`"19:34"`, return`"19:39"`.

```
Explanation: 
The next closest time choosing from digits 1, 9, 3, 4, is 19:39, which occurs 5 minutes later.  It is not 19:33, because this occurs 23 hours and 59 minutes later.
```

Given time =`"23:59"`, return`"22:22"`.

```
Explanation: 
The next closest time choosing from digits 2, 3, 5, 9, is 22:22. It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
```

[https://www.lintcode.com/en/old/problem/next-closest-time/\#](https://www.lintcode.com/en/old/problem/next-closest-time/#)

### 解题分析:

暴力

DFS也可以，但是写的代码太多了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param time: the given time
     * @return: the next closest time
     */


    string nextClosestTime(string &time) {
        unordered_set<char> store(time.begin(), time.end());
        int hr = stoi(time.substr(0, 2));
        int m = stoi(time.substr(3));
        int tot = hr*60 + m;

        for (int i = 1; i < 24*60; i++)
        {
            tot++; 
            char buffer[5];
            sprintf(buffer, "%02d:%02d", tot/60%24, tot%60);
            string res(buffer);
            bool valid = true;
            for (auto& each : res)
                if (store.find(each) == store.end())
                {
                    valid = false;
                    break;
                }
            if (valid)
                return res;
        }
        return time;
    }



};
```

## 425 Letter Combinations of a Phone Number

Given a digit string excluded`01`, return all possible letter combinations that the number could represent.

A mapping of digit to letters \(just like on the telephone buttons\) is given below.

![](https://lintcode-media.s3.amazonaws.com/problem/200px-Telephone-keypad2.svg.png "Cellphone")

##### Notice

Although the above answer is in lexicographical order, your answer could be in any order you want.

Have you met this question in a real interview?

Yes

**Example**

Given`"23"`

Return`["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]` since it is smaller than the input time numerically.

[https://www.lintcode.com/en/old/problem/letter-combinations-of-a-phone-number/](https://www.lintcode.com/en/old/problem/letter-combinations-of-a-phone-number/)

### 解题分析:

好多年前做过的面试题。。。。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param digits: A digital string
     * @return: all posible letter combinations
     */
    vector<string> letterCombinations(string &digits) {
        // write your code here

        vector<string> result;
        if (digits.empty())
            return result;

        unordered_map<char, string> dict({{'2',"abc"},{'3',"def"},{'4',"ghi"},{'5',"jkl"},{'6',"mno"},{'7',"pqrs"},{'8', "tuv"},{'9',"wxyz"}});

        string subset;
        dfs(digits, dict, result, subset , 0);
        return result;
    }

    void dfs(const string& digits, unordered_map<char, string>& dict, vector<string>& results, string& subset, int index)
    {
        if (index >= digits.size())
        {
            results.push_back(subset);
            return;
        }

        const auto& str = dict[digits[index]];
        for (const auto c: str)
        {
            subset.push_back(c);
            dfs(digits, dict, results, subset, index+1);
            subset.pop_back();
        }
    }
};
```

## 10 String Permutation II

Given a string, find all permutations of it without duplicates.

Have you met this question in a real interview?

Yes

**Example**

Given`"abb"`, return`["abb", "bab", "bba"]`.

Given`"aabb"`, return`["aabb", "abab", "baba", "bbaa", "abba", "baab"]`.

[https://www.lintcode.com/en/old/problem/string-permutation-ii/\#](https://www.lintcode.com/en/old/problem/string-permutation-ii/#)

### 解题分析:

典型的permutation的题目

### 代码：

```cpp
class Solution {
public:
    /**
     * @param str: A string
     * @return: all permutations
     */
    vector<string> stringPermutation2(string &str) {
        // write your code here
        vector<string> results;
        sort(str.begin(), str.end());
        vector<bool> visited(str.size(), false);
        string subset;
        dfs(str, visited, subset, results);
        return results;
    }

    void  dfs(string& str, vector<bool>& visited, string& subset, vector<string>& results)
    {
        if (subset.size() == str.size())
        {
            results.push_back(subset);
            return;
        }

        for (int i = 0; i < str.size(); i++)
        {
            if (i!=0 && str[i] == str[i-1] && !visited[i-1])
                continue;
            if (visited[i])
                continue;
            subset.push_back(str[i]);
            visited[i] = true;
            dfs(str, visited, subset, results);
            visited[i] = false;
            subset.pop_back();
        }
    }
};
```

## 33 N-Queens

The n-queens puzzle is the problem of placing n queens on an`n×n`chessboard such that no two queens attack each other.

Given an integer`n`, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where`'Q'`and`'.'`both indicate a queen and an empty space respectively.

Have you met this question in a real interview?

Yes

**Example**

There exist two distinct solutions to the 4-queens puzzle:

```
[
  // Solution 1
  [".Q..",
   "...Q",
   "Q...",
   "..Q."
  ],
  // Solution 2
  ["..Q.",
   "Q...",
   "...Q",
   ".Q.."
  ]
]
```

### 解题分析:

典型的permutation的题目

### 代码：

```cpp
class Solution {
public:
    /*
     * @param n: The number of queens
     * @return: All distinct solutions
     */
    vector<vector<string>> solveNQueens(int n) {
        // write your code here
        vector<string> board(n, string(n,'.'));
        vector<vector<string>> results;
        dfs(results, board, 0);
        return results;
    }

    void dfs(vector<vector<string>>& results, vector<string>& board, int index)
    {
        if (index >= board.size())
        {
            results.push_back(board);
            return;
        }
        for (int i = 0; i < board.size(); i++)
        {
            if (!isValid(board, index, i))
                continue;
            board[index][i] = 'Q';
            dfs(results, board, index+1);
            board[index][i] = '.';
        }
    }

    bool isValid(vector<string>& board, int i, int j)
    {
        for (int k = 0; k < i; k++)
        {
            if (board[k][j] == 'Q')
                return false;
        }

        int m = i, n = j;
        while(m>=0 && n>=0)
        {
            if (board[m][n] == 'Q')
                return false;
            m--;n--;
        }
        m = i;
        n = j;
        while(m>=0 && n<board.size())
        {
            if (board[m][n] == 'Q')
                return false;
            m--;
            n++;
        }
        return true;
    }

};
```

## 34 N-Queens II

Follow up for N-Queens problem.

Now, instead outputting board configurations, return the total number of distinct solutions.

Have you met this question in a real interview?

Yes

**Example**

For n=4, there are 2 distinct solutions.

### 解题分析:

典型的permutation的题目

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: The number of queens.
     * @return: The total number of distinct solutions.
     */
    int totalNQueens(int n) {
        // write your code here
        int res = 0;
        vector<string> board(n, string(n,'.'));
        dfs(res, board, 0);
        return res;
    }

    void dfs(int& res, vector<string>& board, int index)
    {
        if (index >= board.size())
        {
            res++;
            return;
        }
        for (int i = 0; i < board.size(); i++)
        {
            if (!isValid(board, index, i))
                continue;
            board[index][i] = 'Q';
            dfs(res, board, index+1);
            board[index][i] = '.';
        }
    }

    bool isValid(vector<string>& board, int i, int j)
    {
        for (int k = 0; k < i; k++)
        {
            if (board[k][j] == 'Q')
                return false;
        }

        int m = i, n = j;
        while(m>=0 && n>=0)
        {
            if (board[m][n] == 'Q')
                return false;
            m--;n--;
        }
        m = i;
        n = j;
        while(m>=0 && n<board.size())
        {
            if (board[m][n] == 'Q')
                return false;
            m--;
            n++;
        }
        return true;
    }

};
```




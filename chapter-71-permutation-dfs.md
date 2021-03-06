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

```go
import "sort"
func permuteUnique(nums []int) [][]int {
    res := make([][]int, 0)
    subset := make([]int, 0)
    visited := make([]bool, len(nums))
    sort.Ints(nums)
    helper(nums, subset, visited, &res)
    return res
}

func helper(nums []int, subset []int, visited []bool, result *[][]int) {
    if (len(subset) == len(nums)) {
        tmp := make([]int, len(subset))
        copy(tmp, subset)
        *result = append(*result, tmp)
        return
    }
    for i := 0; i < len(visited); i++ {
        if visited[i] {
            continue
        }

        if i != 0 && nums[i] == nums[i-1] && !visited[i-1] {
            continue
        }
        visited[i] = true
        subset = append(subset, nums[i])
        helper(nums, subset, visited, result)
        visited[i] = false
        subset = subset[:len(subset) - 1]
    }
}
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

[https://leetcode.com/problems/next-permutation/](https://leetcode.com/problems/next-permutation/)

以 1,2,3,4,5,6 为例，其排列依次为：

123456

123465

123546

...

654321

可以看到有这样的关系：123456 &lt; 123465 &lt; 123546 &lt; ... &lt; 654321。

算法推导

如何得到这样的排列顺序？这是本文的重点。我们可以这样来分析：

我们希望下一个数比当前数大，这样才满足“下一个排列”的定义。因此只需要将后面的「大数」与前面的「小数」交换，就能得到一个更大的数。比如 123456，将 5 和 6 交换就能得到一个更大的数 123465。

我们还希望下一个数增加的幅度尽可能的小，这样才满足“下一个排列与当前排列紧邻“的要求。为了满足这个要求，我们需要：

在尽可能靠右的低位进行交换，需要从后向前查找

将一个 尽可能小的「大数」 与前面的「小数」交换。比如 123465，下一个排列应该把 5 和 4 交换而不是把 6 和 4 交换

将「大数」换到前面后，需要将「大数」后面的所有数重置为升序，升序排列就是最小的排列。以 123465 为例：首先按照上一步，交换 5 和 4，得到 123564；然后需要将 5 之后的数重置为升序，得到 123546。显然 123546 比 123564 更小，123546 就是 123465 的下一个排列

以上就是求“下一个排列”的分析过程。

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

```go
/**
 * @param nums: An array of integers
 * @return: nothing
 */
func nextPermutation (nums *[]int)  {
    // write your code here
    arr := *nums
    l := len(arr)
    if l <= 1 {
        return
    }
    i := l - 2
    for i >= 0 && arr[i] >= arr[i+1] {
        i--
    }
    j := l - 1
    if (i >= 0 ) {
        for (j > i && arr[j] <= arr[i]) {
            j--
        }
        arr[i], arr[j] = arr[j], arr[i]
    }
    i = i + 1
    j = l - 1
    for i < j {
        arr[i], arr[j] = arr[j], arr[i]
        i++
        j--
    }
}
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

```go
/**
 * @param A: An array of integers
 * @return: A long integer
 */
func permutationIndex (A []int) int64 {
    // write your code here
    n := len(A)
    if n <= 1 {
        return 1
    }
    var res int64 = 1
    var factorial int64 = 1;
    for i := n - 2; i >=0; i-- {
        factorial = factorial * int64( n - i -1)
        var c int64
        for j := i + 1; j <n; j++ {
            if ( A[i] > A[j]) {
                c++
            }
        }
        res += c * factorial
    }
    return res
}
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

```go
/**
 * @param A: An array of integers
 * @return: A long integer
 */
func permutationIndexII (A []int) int64 {
    // write your code here
    n := len(A)
    if n <= 1 {
        return 1
    }

    var res int64 = 1
    var factorial int64 = 1
    var factor int64 = 1
    counter := map[int]int{A[n-1] : 1}

    for i := n - 2; i >= 0; i-- {
        curInd  := n - i - 1
        factorial = factorial * int64(curInd)
        counter[A[i]] = counter[A[i]] + 1
        factor *= int64(counter[A[i]])
        var count int64 = 0
        for j := i + 1; j < n; j++ {
            if A[i] > A[j] {
                count++
            }
        }
        res += count * factorial / factor
    }
    return res
}
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
    string nextClosestTime(string time) {
        unordered_set<char> valids(time.begin(), time.end());
        char buffer[6];

        auto delim = time.find(":");

        int hr = stoi(time.substr(0, delim));
        int mt = stoi(time.substr(delim + 1));
        for (int i = 1; i < 3600 * 24; i++) {
            mt++;
            hr += mt/60;
            mt = mt%60;
            if (hr == 24) hr = 0;
            sprintf(buffer, "%02d:%02d", hr, mt);
            bool good = true;
            for (int j = 0; j <5; j++) {
                if (buffer[j] == ':') continue;
                if (!valids.count(buffer[j])) {
                    good = false;
                    break;
                }
            }
            if (good) return string(buffer);
        }
        return "";
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

[https://leetcode.com/problems/letter-combinations-of-a-phone-number/](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

### 解题分析:

好多年前做过的面试题。。。。

### 代码：

```cpp
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> result;
        if (digits.empty()) return result;
        string subset;
        static vector<string> dict { "","","abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        helper(result, subset, digits, dict, 0);
        return result;
    }

    void helper(vector<string>& result, string& subset, const string& digits, vector<string>& dict, int cur) {
        if (cur == digits.size()) {
            result.push_back(subset);
            return;
        }
        const auto& str = dict[digits[cur] - '0'];
        for (auto c : str) {
            subset.push_back(c);
            helper(result, subset, digits, dict, cur + 1);
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

[https://leetcode.com/problems/n-queens/](https://leetcode.com/problems/n-queens/)

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
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        if (n == 0) return res;
        vector<int> board(n, -1);
        helper(board, res, 0);
        return res;
    }

    void helper(vector<int>& board,  vector<vector<string>>& res, int cur) {
        if (cur == board.size()) {
            vector<string> subset;
            for (int i = 0; i < board.size(); i++) {
                string tmp(board.size(),'.');
                tmp[board[i]] = 'Q';
                subset.push_back(move(tmp));
            }
            res.push_back(move(subset));
            return;
        }
        for (int i = 0; i < board.size(); i++) {
            if (!isValid(board, cur, i)) continue;
            board[cur] = i;
            helper(board, res, cur + 1);
            board[cur] = -1;
        }
    }

    bool isValid(const vector<int>& board, int row, int col) {
        // same col
        for ( int i = row - 1; i >= 0; i--) {
            if (board[i] == col ) return false;
            int delta = row - i;
            if (col - delta >= 0 && board[i] == col-delta) return false;
            if (col + delta < board.size() && board[i] == col + delta) return false;
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
    int totalNQueens(int n) {
        if (n <= 1) return n;
        vector<int> board(n, -1);
        return helper(board, 0);
    }
    int helper(vector<int>& board, int cur) {
        if (cur == board.size()) {
            return 1;
        }
        int total = 0;
        for (int i = 0; i < board.size(); i++) {
            if (!isValid(board, cur, i)) continue;
            board[cur] = i;
            total += helper(board, cur + 1);
            board[cur] = -1;
        }
        return total;
    }

    bool isValid(const vector<int>& board, int row, int col) {
        // same col
        for ( int i = row - 1; i >= 0; i--) {
            if (board[i] == col ) return false;
            int delta = row - i;
            if (col - delta >= 0 && board[i] == col-delta) return false;
            if (col + delta < board.size() && board[i] == col + delta) return false;
        }
        return true;
    }
};
```




## 828. Word Pattern

Given a`pattern`and a string`str`, find if`str`follows the same pattern.

Here**follow**means a full match, such that there is a bijection between a letter in`pattern`and a**non-empty**word in`str`.

### Example

Given pattern =`"abba"`, str =`"dog cat cat dog"`, return`true`.  
Given pattern =`"abba"`, str =`"dog cat cat fish"`, return`false`.  
Given pattern =`"aaaa"`, str =`"dog cat cat dog"`, return`false`.  
Given pattern =`"abba"`, str =`"dog dog dog dog"`, return`false`.

[https://www.lintcode.com/problem/word-pattern/description](https://www.lintcode.com/problem/word-pattern/description)

### 解题分析:

用两个hash table来回搜索， 另外有corner case 就是一个比另外一个长的问题

### 代码：

```cpp
class Solution {
public:
    /**
     * @param pattern: a string, denote pattern string
     * @param str: a string, denote matching string
     * @return: an boolean, denote whether the pattern string and the matching string match or not
     */
    bool wordPattern(string &pattern, string &str) {
        // write your code here
        unordered_map<char, string> charStr;
        unordered_map<string, char> strChar;
        for (int i = 0, j=0; i < pattern.size(); i++)
        {
            if (str.empty())
                return false;
            char c = pattern[i];
            auto pos = str.find(" ");
            auto token = str.substr(0, pos);
            if (pos != string::npos)
                str = str.substr(pos+1);
            else
                str = "";

            auto cit = charStr.find(c);
            auto strit = strChar.find(token);
            if (cit == charStr.end() && strit == strChar.end())
            {
                charStr[c] = token;
                strChar[token] = c;
            }
            else if (cit !=charStr.end() && strit != strChar.end())
            {
                if (cit->first != strit->second)
                    return false;
            }
            else
            {
                return false;
            }
        }
        return str.empty();
    }
};
```

## 211. String Permutation

Given two strings, write a method to decide if one is a permutation of the other.

### Example

`abcd`is a permutation of`bcad`, but`abbe`is not a permutation of`abe`

[https://www.lintcode.com/problem/word-pattern/description](https://www.lintcode.com/problem/word-pattern/description)

### 解题分析:

用个hash table

### 代码：

```cpp
class Solution {
public:
    bool Permutation(string &A, string &B) {
        if (A.size() != B.size() )
            return false;
        unordered_map<char, int> dict;
        for (int i = 0; i < A.size(); i++){
            dict[A[i]]++;dict[B[i]]--;
        }
        for (const auto& each : dict)
            if (each.second != 0) return false;
        return true;
    }
};
```

### 

## 123. Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

### Example

Given board =

```
[
  "ABCE",
  "SFCS",
  "ADEE"
]
```

word = `"ABCCED"`, -&gt; returns `true`,  
word = `"SEE"`, -&gt; returns `true`,  
word = `"ABCB"`, -&gt; returns `false`.

[https://www.lintcode.com/problem/word-search/description](https://www.lintcode.com/problem/word-search/description)

### 解题分析:

BFS/DFS all OK, use DFS here

### 代码：

```cpp
class Solution {
public:
    /**
     * @param board: A list of lists of character
     * @param word: A string
     * @return: A boolean
     */
    bool exist(vector<vector<char>> &board, string &word) {
        // write your code here
        if (word.empty() || board.empty())
            return false;
        int m = board.size();
        int n = board[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        auto leading = word[0];
        for (int i =0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (dfs(board, visited, word, 0, i, j))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>>& board, vector<vector<bool>>& visited, string& word, int current, int i, int j)
    {
        if (current>= word.size() || word[current]== board[i][j] && current == word.size()-1 )
            return true;
        if (word[current] != board[i][j])
            return false;
        static vector<int> dx={-1, 1, 0, 0};
        static vector<int> dy={ 0, 0, 1, -1};
        visited[i][j] = true;
        for (int k =0; k < 4; k++)
        {
            int next_i = i + dx[k];
            int next_j = j + dy[k];
            if (next_i < 0 || next_i >=board.size()|| next_j < 0 || next_j >= board[0].size() || visited[next_i][next_j])
                continue;
            if (dfs(board, visited, word, current+1, next_i, next_j))
                return true;
        }
        visited[i][j] = false;
        return false;
    }
};
```



## 51. Previous Permutation

Given a list of integers, which denote a permutation.

Find the previous permutation in ascending order.

### Example

For`[1,3,2,3]`, the previous permutation is`[1,2,3,3]`

For`[1,2,3,4]`, the previous permutation is`[4,3,2,1]`

https://www.lintcode.com/problem/previous-permutation/description

### 解题分析:

逆过程

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers
     * @return: A list of integers that's previous permuation
     */
    vector<int> previousPermuation(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if (n <= 1)
            return nums;
        int i = n-2;
        while(i >= 0 && nums[i] <= nums[i+1])
            i--;
        if (i >= 0)
        {
            int  j = n-1;
            while( j>=0 && nums[j] >= nums[i])
                j--;
            swap(nums[j], nums[i]);
        }
        reverse(nums.begin()+i+1, nums.end());
        return nums;
    }
};
```






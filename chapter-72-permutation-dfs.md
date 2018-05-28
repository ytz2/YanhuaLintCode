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

[https://www.lintcode.com/problem/previous-permutation/description](https://www.lintcode.com/problem/previous-permutation/description)

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

## 442. Implement Trie \(Prefix Tree\)

Implement a trie with insert, search, and startsWith methods.

### Example

```
insert("lintcode")
search("code")

>
>
>
 false
startsWith("lint")

>
>
>
 true
startsWith("linterror")

>
>
>
 false
insert("linterror")
search("lintcode)

>
>
>
 true
startsWith("linterror")

>
>
>
 true
```

[https://www.lintcode.com/problem/implement-trie-prefix-tree/description](https://www.lintcode.com/problem/implement-trie-prefix-tree/description)

### 解题分析:

Trie的基本操作

### 代码：

```cpp
class Trie {
public:

    class TrieNode{
    public:
        TrieNode()
            : isWord(false),
              children(vector<TrieNode*>(26, nullptr))
        {}

        ~TrieNode(){
            for (auto p : children)
            {
                if (p)
                    delete p;
            }
            children.clear();
        }

        bool isWord;
        vector<TrieNode*> children;
    };


    Trie() {
        // do intialization if necessary
        root = new TrieNode();
    }

    ~Trie(){
        if (root)
            delete root;
    }
    /*
     * @param word: a word
     * @return: nothing
     */
    void insert(string &word) {
        // write your code here
        auto p = root;
        for (auto c:word)
        {
            int ind = c-'a';
            if (!p->children[ind])
                p->children[ind] = new TrieNode();
            p = p->children[ind];
        }
        p->isWord = true;
    }

    /*
     * @param word: A string
     * @return: if the word is in the trie.
     */
    bool search(string &word) {
        // write your code here
        auto p = find(word);
        return p && p->isWord;
    }

    /*
     * @param prefix: A string
     * @return: if there is any word in the trie that starts with the given prefix.
     */
    bool startsWith(string &prefix) {
        // write your code here
        return find(prefix)!=nullptr;
    }
private:
    TrieNode* find(const string& s)
    {
        auto p = root;
        for (const auto c : s)
        {
            int ind = c-'a';
            if (!p->children[ind])
                return nullptr;
            p = p->children[ind];
        }
        return p;
    }

private:
    TrieNode* root;
};
```



## 634. Word Squares

Given a set of words**without duplicates**, find all[`word squares`](https://en.wikipedia.org/wiki/Word_square)you can build from them.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k &lt; max\(numRows, numColumns\).

For example, the word sequence`["ball","area","lead","lady"]`forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y

```

### Example

```
Given a set ["area","lead","wall","lady","ball"]
return [["wall","area","lead","lady"],["ball","area","lead","lady"]]
Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).

Given a set ["abat","baba","atan","atal"]
return [["baba","abat","baba","atan"],["baba","abat","baba","atal"]]
Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

https://www.lintcode.com/problem/word-squares/description

### 解题分析:

第一步上来就是用回溯法一个一个尝试，但是在build的过程中要验证是不是startwith 的问题， 比如 ball area, 第三个的时候要判断是不是lexx 如果words set太大的话就要一个一个过。所以要引入trie, 把每一个节点存入startswith的list，这样一下就可得了，算是剪枝。



### 代码：

```cpp
class TrieNode{
public:
    vector<string> words;
    vector<TrieNode*> children;
    TrieNode()
        : children(vector<TrieNode*>(26, nullptr))
    {
    }
    ~TrieNode()
    {
        for (auto child : children)
            if (child) delete child;
    }
};

class Trie{
public:
    Trie(const vector<string>& words)
    {
        root = new TrieNode();
        for ( const auto& word : words)
        {
            auto p = root;
            for (int i = 0; i < word.size(); i++)
            {
                int ind = word[i] - 'a';
                if (!p->children[ind])
                {
                    p->children[ind] = new TrieNode();
                }
                p->children[ind]->words.push_back(word);
                p = p->children[ind];
            }
        }
    }
    
    const vector<string>& startWith(const string& str)
    {
        static vector<string> dummy;
        auto p = root;
        for (int i = 0; i < str.size(); i++)
        {
            int ind = str[i]-'a';
            if (!p->children[ind])
                return dummy;
            p = p->children[ind];
        }
        return p->words;
    }
    
    ~Trie()
    {
        delete root;
    }
private:
  TrieNode* root;  
};


class Solution {
public:
    /*
     * @param words: a set of words without duplicates
     * @return: all word squares
     */
    vector<vector<string>> wordSquares(vector<string> &words) {
        Trie trie(words);
        vector<vector<string>> results;
        if (words.empty())
            return results;
        vector<string> subset;
        for (auto word: words)
        {
            subset.push_back(word);
            dfs(subset, results, trie);
            subset.pop_back();
        }
        dfs( subset, results,trie);
        return results;
    }
    
    void dfs(vector<string>& subset, vector<vector<string>>& results, Trie& trie)
    {
        if (subset.size() == subset[0].size())
        {
            results.push_back(subset);
            return;
        }
        int n = subset.size();
        string str;
        for (int i = 0; i < n; i++)
        {
            str.push_back(subset[i][n]);
        }
        const auto& words = trie.startWith(str);
        for (const auto& word : words)
        {
            subset.push_back(word);
            dfs(subset, results, trie);
            subset.pop_back();
        }
    }
};
```






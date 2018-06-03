# Word 四兄弟问题

## 大哥 WordBreakII

## 582. Word Break II

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

### Example

Gieve s =`lintcode`,  
dict =`["de", "ding", "co", "code", "lint"]`.

A solution is`["lint code", "lint co de"]`.

[https://www.lintcode.com/problem/word-break-ii/description](https://www.lintcode.com/problem/word-break-ii/description)

### 解题分析:

dfs

### 代码：

```cpp
class Solution {
public:
    /*
     * @param s: A string
     * @param wordDict: A set of words.
     * @return: All possible sentences.
     */
    vector<string> wordBreak(string &s, unordered_set<string> &wordDict) {
        // write your code here
        vector<string> results;
        if (s.empty() || wordDict.empty())
            return results;
        unordered_map<int,bool> canBreak;
        vector<string> subset;
        dfs(s, wordDict, subset, results, canBreak, 0);
        return results;
    }

    void dfs(string& s, unordered_set<string>& wordDict, vector<string>& subset, vector<string>& results, unordered_map<int, bool>& canBreak, int startIndex)
    {
        if (startIndex >= s.size())
        {
            string res;
            for (auto& each:subset)
                res+=each+" ";
            res.pop_back();
            results.push_back(res);
        }

        if (canBreak.find(startIndex) != canBreak.end() && !canBreak[startIndex])
            return;

        for (int i = startIndex; i <s.size(); i++)
        {
            auto str = s.substr(startIndex, i-startIndex+1);
            if (wordDict.find(str) == wordDict.end())
                continue;
            subset.push_back(str);
            int n = results.size();
            dfs(s, wordDict, subset, results, canBreak, i+1);
            canBreak[i+1] = n != results.size();
            subset.pop_back();
        }
    }
};
```

## 829. Word Pattern II

Given a`pattern`and a string`str`, find if`str`follows the same pattern.

Here**follow**means a full match, such that there is a[bijection](https://baike.baidu.com/item/双射/942799?fr=aladdin)between a letter in`pattern`and a**non-empty**substring in`str`.\(i.e if`a`corresponds to`s`, then`b`cannot correspond to`s`. For example, given pattern =`"ab"`, str =`"ss"`, return`false`.\)

### Example

Given pattern =`"abab"`, str =`"redblueredblue"`, return`true`.  
Given pattern =`"aaaa"`, str =`"asdasdasdasd"`, return`true`.  
Given pattern =`"aabb"`, str =`"xyzabcxzyabc"`, return`false`.

[https://www.lintcode.com/problem/word-pattern-ii/description](https://www.lintcode.com/problem/word-pattern-ii/description)

### 解题分析:

用两个hash table来回搜索， 另外有corner case 就是一个比另外一个长的问题

### 代码：

```cpp
class Solution {
public:
    /**
     * @param pattern: a string,denote pattern string
     * @param str: a string, denote matching string
     * @return: a boolean
     */
    bool wordPatternMatch(string &pattern, string &str) {
        // write your code here
        return dfs(pattern, str, 0, 0);
    }

    bool dfs(const string& pattern, const string& str, int i, int j)
    {
        if (i == pattern.size() && j == str.size())
            return true;
        if (i == pattern.size() || j == str.size())
            return false;
        char p = pattern[i];
        if (cache.find(p) != cache.end())
        {
            auto strpos = str.find(cache[p], j);
            if (str.size()-j < cache[p].size() || str.substr(j, cache[p].size()) != cache[p] )
                return false;
            auto next_j = j + cache[p].size();
            return dfs(pattern, str, i+1, next_j);
        }

        for (int jj = j; jj < str.size(); jj++)
        {
            auto substr = str.substr(j, jj-j + 1);
            if (cacheImage.find(substr) != cacheImage.end())
                continue;
            cache[p] = substr;
            cacheImage[substr] = p;
            if (dfs(pattern, str, i+1, jj+1))
                return true;
            cache.erase(p);
            cacheImage.erase(substr);
        }

        return false;
    }

    unordered_map<char, string> cache;
    unordered_map<string, char> cacheImage;
};
```

## 132. Word Search II

Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position.

### Example

Given matrix:

```
doaf


agai


dcan
```

and dictionary:

```
{"dog", "dad", "dgdg", "can", "again"}
```

return {"dog", "dad", "can", "again"}

dog:

```
do
af


a
g
ai


dcan
```

dad:

```
d
oaf


a
gai


d
can
```

can:

```
doaf


agai


d
can
```

again:

```
doaf


agai


dca
n
```

### Challenge

Using trie to implement your algorithm.

### 

### 代码：

```cpp
struct TrieNode{
    TrieNode()
        :children(vector<TrieNode*>(26, nullptr)),
         isWord(false)
        {}
    ~TrieNode()
    {
        for (auto ptr : children)
            if (ptr) delete ptr;
    }
    vector<TrieNode*> children;
    bool isWord;
};


class Trie{
 public:
    Trie(const vector<string>& words)
    {
        root = new TrieNode();
        for (const auto& word: words)
        {
            auto p = root;
            for (char c : word)
            {
                int ind = c-'a';
                if (!p->children[ind])
                    p->children[ind] = new TrieNode();
                p = p->children[ind];
            }
            p->isWord = true;
        }
    }
    ~Trie()
    {
        delete root;
    }

    bool startWith(const string& str, bool& isWord)
    {
        auto p = root;
        isWord = false;
        for ( const auto c : str)
        {
            int ind = c - 'a';
            if (!p->children[ind])
                return false;
            p = p->children[ind];
        }
        isWord = p->isWord;

        return true;
    }

 private:
    TrieNode* root;
};

class Solution {
public:
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    vector<string> wordSearchII(vector<vector<char>> &board, vector<string> &words) {
        // write your code here
        vector<string> results;
        if (board.empty() || board[0].empty())
            return results;
        Trie trie(words);
        int m = board.size(), n = board[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));

        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                string sub(1, board[i][j]);
                visited[i][j] = true;
                dfs(board, visited, trie, i, j, sub, results);
                visited[i][j] = false;
            }
        }
        return results;
    }

    void dfs(vector<vector<char>> &board, vector<vector<bool>>& visited,  Trie& trie, int i, int j, string& substr, vector<string> & results)
    {
        bool isWord=false;
        if (!trie.startWith(substr, isWord))
            return;
        if (isWord && find(results.begin(), results.end(), substr) == results.end())
            results.push_back(substr);
        int m = board.size();
        int n = board[0].size();
        static vector<int> dx{ -1, 1, 0, 0};
        static vector<int> dy{ 0,  0, -1, 1};
        for (int k = 0; k < 4; k++)
        {
            int next_i = i + dx[k];
            int next_j = j + dy[k];

            if (next_i < 0 || next_i >= m || next_j < 0 || next_j >= n || visited[next_i][next_j])
                continue;
            visited[next_i][next_j] = true;
            substr.push_back(board[next_i][next_j]);
            dfs(board, visited, trie, next_i, next_j, substr, results);
            substr.pop_back();
            visited[next_i][next_j] = false;
        }

    }

};
```



## 121. Word Ladder II

Given two words \(startandend\), and a dictionary, find all shortest transformation sequence\(s\) fromstarttoend, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

### Example

Given:  
start=`"hit"`  
end=`"cog"`  
dict=`["hot","dot","dog","lot","log"]`  


Return  


```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```

### Challenge

这道题遇见过好多次了。。。。这算第一次动手做， 相比于word ladder I 应为是求所有解，所以会是一个建立隐式图，然后遍历隐式图生成解的过程。 



中间很多不一样的处理方式可以从下面这个例子来引申开来

![](/assets/wl2.png)

Word Ladder I 里面我们如从rex-&gt;text  则标记tex为访问过了， 就不会再考虑ted-&gt;tex， 但是tex-&gt;tex是一个合法的路径。 所以说BFS只能保证我们层级遍历，但是保证不了node之间的关系。 

为了建立图的关系， 每一个节点都要标记他从当前所引申出来的下一步

unordered\_map&lt;string, vector&lt;string&gt; &gt; graph

因为要做DFS最短路的遍历，所以还要标记出每一步的计数， 而不是用unordered\_set. 

unordered\_map&lt;string, int&gt; counter;

为了不抹去ted-&gt;text

```
if ( counter.find(str) == counter.end())
{
    q.push(str);
    counter[str] = counter[parent]+1;
}
//还是要把他放到图的children节点之中，我们已经遍历过这个节点了，但是我们需要这个节点在生成答案中用到
graph[original].push_back(str);
```



最后生成答案的时候就按图索骥， 根据步数， 图 来生成所有的路径。

注意DFS的时候 要把下一步的步数不是 cur != last+1的滤掉。

### 代码：

```cpp
class Solution {
public:
    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: a list of lists of string
     */
    vector<vector<string>> findLadders(string &start, string &end, unordered_set<string> &dict) {
        // write your code here
        dict.insert(end);
        unordered_map<string, int> counter;
        unordered_map<string, vector<string>> next;
        queue<string> q;
        q.push(start);
        counter[start] = 0;
        vector<vector<string>> results;
        vector<string> subset;
        while(!q.empty())
        {
            string word = q.front();
            string original = word;
            q.pop();
            if (word == end)
                break;
            vector<string> snext;
            for (int i = 0; i < word.size(); i++)
            {
                char c = word[i];
                for (char ch = 'a'; ch <='z'; ch++)
                {
                    if (ch == c)
                        continue;
                    word[i] = ch;
                    if (dict.find(word) == dict.end())
                        continue;
                    if (counter.find(word) == counter.end())
                    {
                        q.push(word);
                        counter[word] = counter[original] + 1;
                    }
                    snext.push_back(word);
                }
                word[i] = c;
            }
            next[original] = snext;
        }
        subset.push_back(start);
        dfs(start, end, counter, next, results, subset);
        return results;
    }
    
    void dfs(const string& now, const string& end, unordered_map<string, int>& counter, unordered_map<string, vector<string>>& next, vector<vector<string>>& results, vector<string>& subset )
    {
        if (now == end)
        {
            results.push_back(subset);
            return;
        }
        
        const auto& vec = next[now];
        for (auto each : vec)
        {
            if (counter[now]+1 != counter[each])
                continue;
            subset.push_back(each);
            dfs(each, end, counter, next, results, subset);
            subset.pop_back();
        }
    }
};
```




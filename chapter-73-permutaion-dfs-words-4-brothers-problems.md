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
    bool wordPatternMatch(string pattern, string str) {
        if (pattern.empty() || str.empty()) {
            return pattern.empty() && str.empty();
        }
        vector<string> dict(26, "");
        unordered_set<string> used;
        return helper(pattern, str, dict, used, 0, 0);
    }

    bool helper(const string& pattern, const string& str, vector<string>& dict, unordered_set<string>& used, int l, int r) {
        if ( l == pattern.size() || r == str.size()) {
            return l == pattern.size() && r == str.size();
        }
        char c = pattern[l];
        int ind = c - 'a';
        if (!dict[ind].empty()) {
            const auto& ref = dict[ind];
            if (r + ref.size() > str.size()) return false;
            return str.substr(r, ref.size()) == ref && helper(pattern, str, dict, used, l + 1, r + ref.size());
        }
        for (int len = 1; len <= str.size() - r; len++) {
            auto sub =str.substr(r, len);
            if (used.count(sub)) continue;
            used.insert(sub);
            dict[ind] = sub;
            if (helper(pattern, str, dict, used, l + 1, r + sub.size()))
                return true;
            used.erase(sub);
            dict[ind] = "";           
        }
        return false;
    }
};
```

## 132. Word Search II

Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position.

[https://leetcode.com/problems/word-search-ii/](https://leetcode.com/problems/word-search-ii/)

**Note:**

1. All inputs are consist of lowercase letters
   `a-z`
   .
2. The values of 
   `words`
   are distinct.

### 

### 代码：

```cpp
class Solution {
public:
    struct Node {
        vector<Node*> children = vector<Node*>(26, nullptr);
        bool isWord = false;
        Node() = default;
        ~Node() {
            for (auto c : children) 
                if (c) delete c;
        }
    };

    struct Trie {
        Trie(const vector<string>& words) {
            for (const auto& word : words) {
                auto cur = &root;
                for (auto c : word) {
                    auto ind = c - 'a';
                    if (!cur->children[ind]) cur->children[ind] = new Node;
                    cur = cur->children[ind];
                }
                cur->isWord = true;
            }
        }

        Node* find(const string& prefix) {
            auto cur = &root;
            for (auto c : prefix) {
                auto ind = c - 'a';
                if (ind < 0 || ind >= 26) return nullptr;
                if (!cur->children[ind]) return nullptr;
                cur = cur->children[ind];
            }
            return cur;
        }

        bool search(const string& word) {
            auto node = find(word);
            return node && node->isWord;
        }

        bool startWith(const string& word) {
            return nullptr != find(word);
        }

        Node root;
    };

    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        unordered_set<string> result;
        if (board.empty() || board[0].empty() || words.empty()) return {};

        Trie trie(words);
        string str;

        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                char c = board[i][j];
                board[i][j] = ' ';
                str.push_back(c);
                if (trie.startWith(str)) 
                    helper(board, result, str, trie, i, j);
                str.pop_back();
                board[i][j] = c;
            }
        }
        return vector<string>(result.begin(), result.end());
    }

    void helper(vector<vector<char>>& board, unordered_set<string>& result, string& str, Trie& trie, int i, int j) {
        if (trie.search(str)) {
            result.insert(str);
        }
        const static vector<int> dx{0, 0, -1, 1};
        const static vector<int> dy{-1, 1, 0, 0};
        for (int k = 0; k < 4; k++){
            int x = i + dx[k];
            int y = j + dy[k];
            if (x < 0 || x >= board.size() || y < 0 || y >= board[0].size()) continue;
            char c = board[x][y];
            board[x][y] = ' ';
            str.push_back(c);
            if (trie.startWith(str)) helper(board, result, str, trie, x, y);
            str.pop_back();
            board[x][y] = c;
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
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_map<string, unordered_set<string>> graph;
        unordered_map<string,int> counter;
        vector<vector<string>> result;
        queue<string> q;
        q.push(beginWord);
        unordered_set<string> dict(wordList.begin(), wordList.end());
        unordered_set<string> visited{beginWord};
        bool found = false;
        while(!q.empty()) {
            int n = q.size();
            for (int i = 0; i < n; i++) {
                auto str = q.front(); q.pop();
                auto orig = str;
                if (str == endWord) found = true;
                for (int j = 0; j < str.size(); j++) {
                    auto c = str[j];
                    for (auto ch = 'a'; ch <= 'z'; ch++) {
                        if (ch == c) continue;
                        str[j] = ch;
                        if (!dict.count(str)) continue;
                        if (!counter.count(str)) counter[str] = counter[orig] + 1;
                        // cache the option
                        graph[orig].insert(str);
                        if (visited.count(str)) continue;
                        visited.insert(str);
                        q.push(str);
                    }
                    str[j] = c;
                }
            }
            if (found) break;
        }
        if (!found) return result;
        vector<string> subset{beginWord};
        helper(beginWord, endWord, subset, result, graph, counter);
        return result;
    }

    void helper(const string& begin, const string& end, vector<string>& subset, vector<vector<string>>& res, unordered_map<string, unordered_set<string>>& graph, unordered_map<string, int>& counter) {
        if (begin == end) {
            res.push_back(subset);
            return;
        }

        const auto& nexts = graph[begin];
        for (const auto& word : nexts) {
            if (counter[word] != counter[begin] + 1) continue;
            subset.push_back(word);
            helper(word, end, subset, res, graph, counter);
            subset.pop_back();
        }
    }
};
```




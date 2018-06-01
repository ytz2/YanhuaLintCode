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

Here**follow**means a full match, such that there is a[bijection](https://baike.baidu.com/item/%E5%8F%8C%E5%B0%84/942799?fr=aladdin)between a letter in`pattern`and a**non-empty**substring in`str`.\(i.e if`a`corresponds to`s`, then`b`cannot correspond to`s`. For example, given pattern =`"ab"`, str =`"ss"`, return`false`.\)

### Example

Given pattern =`"abab"`, str =`"redblueredblue"`, return`true`.  
Given pattern =`"aaaa"`, str =`"asdasdasdasd"`, return`true`.  
Given pattern =`"aabb"`, str =`"xyzabcxzyabc"`, return`false`.

https://www.lintcode.com/problem/word-pattern-ii/description

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




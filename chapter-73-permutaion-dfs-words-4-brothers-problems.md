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

用两个hash table来回搜索， 另外有corner case 就是一个比另外一个长的问题

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




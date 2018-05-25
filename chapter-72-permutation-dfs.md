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




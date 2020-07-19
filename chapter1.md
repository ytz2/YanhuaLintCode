# Warm up

## 627 Longest Palindrome

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example`"Aa"`is not considered a palindrome here.

**Example**

Given s =`"abccccdd"`return`7`

One longest palindrome that can be built is`"dccaccd"`, whose length is`7`.

[http://www.lintcode.com/en/problem/longest-palindrome/](http://www.lintcode.com/en/problem/longest-palindrome/)

### 解题分析:

首先要明确一个概念，什么是回文串。比如：上海自来水来自海上。 那么具体判断是不是回文， 用code来说话：

```cpp
 bool isPalindrome(const std::string& s, int beg, int end)
 {
   while(beg <= end)
   {
     if (s[beg]!=s[end])
       break;
     beg++; end--;
   }
   return beg>end;
 }
```

上面这段代码是判断回文的套路， 那么发现， 回文的成分字母可以全部为偶数个， 也可以为一个奇数字母在中间，和其他偶数个字母组成。

那么问题就变成了数字母的游戏：

1. Count the occurrence of each char
2. iterate over the counting table:  
   1. If it is even ： len+=2  
   2. If it is odd and count &gt;3, len +=count-1 and mark once

   1. If ever marked return len+1, else return len

3. ### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: a string which consists of lowercase or uppercase letters
     * @return: the length of the longest palindromes that can be built
     */
    int longestPalindrome(string &s) {
        // write your code here
        unordered_map<char, int> countingMap;
        for(const auto& c : s)
            countingMap[c]++;
        int res = 0;
        bool hasOdd = false;
        for(const auto& each : countingMap)
        {
            int c = each.second;
            if (c%2)
            {
                res += c-1;
                hasOdd = true;
            }
            else
            {
                res += c;
            }
        }
        return hasOdd? res+1 : res;
    }
};
```

### 复杂度分析:

O（n\) , n = s.size\(\)

###### 2020/07/19

[https://leetcode.com/problems/longest-palindrome/](https://leetcode.com/problems/longest-palindrome/)

回顾一下， 这个题目有两个可以改善的地方

1. 不要想着一口吃个胖子，解决问题的时候永远先计数再解决问题，模块化比速度重要很多
2. 解决字节计数的问题的时候使用array效率要高很多 
3. ```cpp
   class Solution {
   public:
    int longestPalindrome(string s) {
        std::vector<int> count(128, 0);
        for (const auto c : s)
            count[c]++;
        int len = 0;
        int hasOdd = 0;
        for (const auto& each : count) {
            if (each % 2 == 1)
                hasOdd = 1;
            len += each - each % 2;
        }
        return len + hasOdd;
    }
   };
   ```

```py
# python3
class Solution:
    def longestPalindrome(self, s: str) -> int:
        arr = [0] * 128
        for c in s:
            arr[ord(c)] = arr[ord(c)] + 1
        res = 0
        hasOdd = 0
        for n in arr:
            if (n % 2 == 1):
                hasOdd = 1;
            res = res + n - n%2
        return res + hasOdd
```

```go
// golang
func longestPalindrome(s string) int {
    var arr [128]int
    for _, c := range s {
        arr[int(c)]++   
    }
    res := 0
    hasOdd := 0
    for _, v := range arr {
        if v % 2 == 1 {
            hasOdd = 1
        }
        res += v - v%2
    }
    return res + hasOdd
}
```

## 415 Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

##### Notice

Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.

**Example**

`"A man, a plan, a canal: Panama"`is a palindrome.

`"race a car"`is not a palindrome.

[http://www.lintcode.com/en/problem/valid-palindrome/](http://www.lintcode.com/en/problem/valid-palindrome/)

### 解题分析:

在627中的解题分析中已经写了如何判断是否为回文的代码， 但是不满足此题目的要求， 此题要求字母必须为a-zA-Z0-9, 其余可以忽略。 那么就有一些corner case需要考虑， 列举如下：

1. 空字符串
2. 忽略大小写
3. 忽略非字符

实现非常简单。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: A string
     * @return: Whether the string is a valid palindrome
     */
    bool isPalindrome(string &s) {
        // write your code here
        if (s.empty())
            return true;
        int beg = 0, end = s.size()-1;

        while( beg <= end)
        {
            while( beg <= end && !isValid(s[beg]))
                beg++;
            while( beg <= end && !isValid(s[end]))
                end--;
            if (beg <= end && std::tolower(s[beg]) != std::tolower(s[end]))
                return false;
            beg++;
            end--;
        }
        return true;
    }

private:
    bool isValid(const char& c)
    {
        return c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z' || c >= '0' && c <= '9';
    }
};
```

### 复杂度分析:

O（n\) , n = s.size\(\)

### 笔记

```
if (beg <= end && std::tolower(s[beg]) != std::tolower(s[end]))
```

这一行有两个位置容易出错， 在忽略完非法字符后，应该再判断是否越界，并且将待比较字符归一。

2020/07/19

isValid可以用C API 来替代， 另外不需要考虑空的情况 while\(beg &lt; end）可以cover

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int beg = 0;
        int end = s.size() - 1;
        while(beg < end) {
            while(beg < end && !isalnum(s[beg])) beg++;
            while(beg < end && !isalnum(s[end])) end--;
            if (beg < end && std::tolower(s[beg++]) != std::tolower(s[end--]))
                return false;
        }
        return true;
    }    
};
```

```py
#py3
class Solution:
    def isPalindrome(self, s: str) -> bool:
        i = 0 
        j = len(s) - 1
        while( i < j):
            while( i < j and not s[i].isalnum()):
                i+=1
            while( i < j and not s[j].isalnum()):
                j-=1
            if (s[i].lower() != s[j].lower()):
                return False
            i+=1
            j-=1
        return True
```

```
// golang
// 好久不写golang手生， golang 两个var的forloop pattern, string index 要变成rune才能开搞
import  "unicode"
func isPalindrome(s string) bool {
    f := func(r rune) bool {
        return r >= 'A' && r <= 'Z'|| r >= 'a' && r <= 'z' || r >='0' && r <= '9'
    }
    beg := 0
    end := len(s) - 1
    for beg, end = 0, len(s) - 1; beg < end; beg, end = beg + 1, end - 1 {
        for ;beg < end && !f(rune(s[beg])); beg++ {

        }
        for ;beg < end && !f(rune(s[end]));end--{

        } 
        if unicode.ToLower(rune(s[beg])) != unicode.ToLower(rune(s[end])) {
            return false
        }
    }
    return true
}
```

## 

## 13 strStr

For a given source string and a target string, you should output the**first**index\(from 0\) of target string in source string.

If target does not exist in source, just return`-1`.

Have you met this question in a real interview?

Yes

**Clarification**

Do I need to implement KMP Algorithm in a real interview?

* Not necessary. When you meet this problem in a real interview, the interviewer may just want to test your basic implementation ability. But make sure you confirm with the interviewer first.

**Example**

If source =`"source"`and target =`"target"`, return`-1`.

If source =`"abcdabcdefg"`and target =`"bcd"`, return`1`.

[http://www.lintcode.com/en/problem/valid-palindrome/](https://leetcode.com/problems/implement-strstr/)

### 解题分析:

读书的时候implement过boyer-morre 的算法， 可以达到O\(N\)的复杂度，空间复杂度记得是O\(256\)， 256为ascii码长度，但是这道题目是不可能用这种算法面试的时候写出的。

所以可以简单看成一个str find的implementation. 这道题目在leetcode上有用string版本的， 这个依然是cstring style,所以考虑好以下corner case:

1. source == null
2. target == null
3. source == "\0"
4. target == "\0"

双层遍历即可实现。

### 代码：

```cpp
class Solution {
public:
    /*
     * @param source: source string to be scanned.
     * @param target: target string containing the sequence of characters to match
     * @return: a index to the first occurrence of target in source, or -1  if target is not part of source.
     */
    int strStr(const char *source, const char *target) {
        // write your code here
        if (!target)
            return -1;
        if ( !*target)
            return 0;
        if (!source)
            return -1;

        int res = 0;

        while(*source && *target)
        {
            const char *sourcePtr = source;
            const char *targetPtr = target;
            while(*sourcePtr && *targetPtr && *sourcePtr == *targetPtr)
            {
                sourcePtr++;
                targetPtr++;
            }

            if (!*targetPtr)
                return res;
            source++;
            res++;
        }

        return -1;
    }

};
```

### 复杂度分析:

O（mn\) m = strlen\(m\) n=strlen\(target\)



2020/07/19

```cpp
// c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = needle.size();
        int m = haystack.size();
        if (n == 0)
            return 0;
        int i = 0;
        while ( i <= m - n) {
            bool found = true;
            for (int k =i, j = 0;  j < n; k++, j++) {
                if (haystack[k] != needle[j]) {
                    found = false;
                    break;
                }
            }
            if (found)
                return i;
            i++;
        }
        return -1;
    }
};
```

```py
# py3
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if len(needle) == 0:
            return 0
        i = 0
        m = len(haystack)
        n = len(needle)
        while(i <= m - n):
            j , k = i, 0
            match = True
            while (k < n):
                if haystack[j] != needle[k]:
                    match = False
                    break
                k+=1
                j+=1
            if match:
                return i
            i+=1
        return -1
        
```

```go
// golang
func strStr(haystack string, needle string) int {
    m, n := len(haystack), len(needle)
    if len(needle) == 0 {
        return 0
    }
    for i:=0; i <= m - n; i++ {
        found := true
        for j,k := i,0; k < n; j,k = j+1, k+1 {
            if haystack[j] != needle[k] {
                found = false
                break
            }
        }
        if found {
            return i
        }
    }
    return -1
}
```

## 200 Longest Palindromic Substring

Given a string`S`, find the longest palindromic substring in`S`. You may assume that the maximum length of`S`is 1000, and there exists one unique longest palindromic substring.

Have you met this question in a real interview?

Yes

**Example**

Given the string =`"abcdzdcab"`, return`"cdzdc"`.

[http://www.lintcode.com/en/problem/longest-palindromic-substring/](http://www.lintcode.com/en/problem/longest-palindromic-substring/)

### 解题分析:

回文的题目在做搜索问题的时候是有套路的：

1 中心搜索法

以 每一个A字符串A\[i\]  为中心， 向外辐射找最长回文， 同时记录最长字符位置和长度

2 动态规划法

动态规划对于判定从i到j是否为回文有一个固定的套路， 简单列举如下

状态表示： A\[i\]\[j\] 表示从i-&gt;j 是否为回文串

状态初始： A\[i\]\[i\] = true, A\[i\]\[i+1\] =a\[i\] == a\[i+1\] , 之所以要考虑i+1是因为回文可以是奇数回文也可以是偶数回文

状态方程 ： A\[i\]\[j\] = A\[i+1\]\[j-1\] && a\[i\] == a\[j\]

最后结果：  max of j-i+1 where A\[i\]\[j\] == true

这两种方法都可以，只是动态规划要有额外的空间开销。

### 代码：

1. DP解法， 超时了，但是没有必要做细节优化，已经是o\(n^2\)的复杂度了， 如果非要深究细节，就是遍历了两次，再就是STL开辟大的空间耗时等等。

```cpp
class Solution {
public:
    /**
     * @param s: input string
     * @return: the longest palindromic substring
     */
    string longestPalindrome(string &s) {
        // write your code here
        if (s.empty())
            return s;
        vector<vector<bool>> isPalindromeTbl(s.size(), vector<bool>(s.size(), false));
        buildTable(s, isPalindromeTbl);

        int start =0, len = 1;
        for (int i =0; i < s.size(); i++)
        {
            for (int j = 0; j < s.size(); j++)
            {
                if (isPalindrome(i,j,isPalindromeTbl) && j-i+1 > len)
                {
                    len = j-i+1;
                    start = i;
                }
            }
        }

        return s.substr(start, len);
    }

private:

    void buildTable(const string& s, vector<vector<bool>>& isPalindrome_)
    {
        int n = s.size();
        for(int i=0; i < n; i++)
            isPalindrome_[i][i] = true;
        for(int i=0; i < n-1; i++)
            isPalindrome_[i][i+1] = s[i] == s[i+1];

        for (int len = 2; len < n; len++)
        {
            for (int i = 0; i + len < n; i++)
            {
                int j = i + len;
                isPalindrome_[i][j] = isPalindrome_[i+1][j-1] && s[i] == s[j]; 
            }
        }
    }

    bool isPalindrome(int i, int j, const vector<vector<bool>>& isPalindrome_)
    {
        assert(i < isPalindrome_.size() && j < isPalindrome_.size());
        return isPalindrome_[i][j];
    }

};
```

1. 中心射线法

```cpp
class Solution {
public:
    /**
     * @param s: input string
     * @return: the longest palindromic substring
     */
    string longestPalindrome(string &s) {
        // write your code here

        int start = 0, len = 1;
        for (int i = 0; i < s.size(); i++)
        {
            auto lengthOdd = radius(s, i, i);
            if (lengthOdd > len )
            {
                len = lengthOdd;
                // eg (3,3) -> (1,5) -> return 5  , start = 3-len/2 
                start = i - len/2;
            }
            auto lengthEven = radius(s, i, i+1);
            if (lengthEven > len)
            {
                len = lengthEven;
                // eg. (3,4) -> (2,5)  -> return 4, then 2= 3-4/2+1
                start = i - len/2 +1;
            }
        }
        return s.substr(start, len);
    }

    int radius(const string& s, int i, int j)
    {
        while(i >= 0 && j < s.size() && s[i] == s[j])
        {
            i--;
            j++;
        }
        return j-i-1;
    }
};
```

### 复杂度分析:

都是o\(n^2\)， DP有额外o\(n^2\)的空间复杂度

### 

## 667 Longest Palindromic Subsequence

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is`1000`.

Have you met this question in a real interview?

Yes

**Example**

Given s =`"bbbab"`return`4`  
One possible longest palindromic subsequence is`"bbbb"`.

[http://www.lintcode.com/en/problem/valid-palindrome/](http://www.lintcode.com/en/problem/longest-palindromic-subsequence/#)

### 解题分析:

同是回文的题目，此题与200有类似，不同之处为子回文串需要顺序但是不需要连续， 这就否定了中心射线法。当然，如果需要做中心射线法也可以，但就变成了一个递归问题，复杂度会更高一些。

那么需要选择DP解法，DP有一类问题的最大最小路径问题与这个其实类似， 200题中buildTable的过程中只是把是否为回文串\(boolean\)放在其中， 如果换个思路， tbl\[i\]\[j\] 存的是最大回文串长度呢？

状态描述： a\[i\]\[j\] = max of palindrome length from i to j

状态初始： a\[i\]\[i\] = 1, a\[i\]\[i+1\] = a\[i\]==a\[i+1\]?2:1

状态方程： a\[i\]\[j\] = maxmax \(a\[i+1\]\[j\[, a\[i\]\[j-1\],  s\[i\] == s\[j\] ? a\[i+1\]\[j-1\]+2 : INT\_MIN\)

最后结果： a\[0\]\[n-1\]

### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: the maximum length of s is 1000
     * @return: the longest palindromic subsequence's length
     */
    int longestPalindromeSubseq(string &s) {
        // write your code here
        if (s.empty())
            return 0;
        vector<vector<int>> tbl(s.size(), vector<int>(s.size(), 0));
        for(int i = 0; i < s.size(); i++)
            tbl[i][i] = 1;
        for(int i = 0; i < s.size()-1; i++)
            tbl[i][i+1] = (s[i] == s[i+1]? 2 : 1);
        for (int len = 2; len < s.size(); len++)
        {
            for (int i = 0; i+len < s.size(); i++)
            {
                int j = i+len;
                tbl[i][j] = max(max(tbl[i+1][j], tbl[i][j-1]), s[i]==s[j]? tbl[i+1][j-1]+2 : INT_MIN);
            }
        }
        return tbl[0][s.size()-1];
    }
};
```

### 复杂度分析:

o\(n^2\)

## 841 String Replace

Given two identical-sized string array`A`,`B`and a string`S`. All substrings`A`appearing in`S`are replaced by`B`.\(Notice: From left to right, it**must**be replaced if it can be replaced. If there are multiple alternatives, replace longer priorities. After the replacement of the characters can't be replaced again.\)

##### Notice

* The size of each string array does not exceed
  `100`
  , the total string length does not exceed
  `50000`
  .
* The lengths of A \[i\] and B \[i\] are equal.
* The length of S does not exceed
  `50000`
  .
* All characters are lowercase letters.
* We guarantee that the A array does not have the same string

Have you met this question in a real interview?

Yes

**Example**

Given A =`["ab","aba"]`, B =`["cc","ccc"]`, S =`"ababa"`, return`"cccba"`.

```
In accordance with the rules, the substring that can be replaced is "ab" or "aba". Since "aba" is longer, we replace "aba" with "ccc".
```

Given A =`["ab","aba"]`, B =`["cc","ccc"]`,S =`"aaaaa"`,return`"aaaaa"`.

```
S does not contain strings in A, so no replacement is done.
```

Given A =`["cd","dab","ab"]`, B =`["cc","aaa","dd"]`, S =`"cdab"`, return`"ccdd"`.

```
From left to right, you can find the "cd" can be replaced at first, so after the replacement becomes "ccab", then you can find "ab" can be replaced, so the string after the replacement is "ccdd".
```

[http://www.lintcode.com/en/problem/string-replace/](http://www.lintcode.com/en/problem/string-replace/)

### 解题分析:

这道题目挺接地气的， 至少看上去是日常写代码的时候需要解决的一个具体的问题。

题目要求用a,b 组成的key value pair 作为字典， 把s中最长的串替换掉。 写起来很好玩，至少在production中我也会这么写替换不难， 难道是找出最长的替换

那么就可以用一个两层的字典来存， 第一层存长度，字典， 第二层字典中存 string, 对应的在a中的位置。 在搜索s的时候， 从最长的长高度开始查找我们的字典， 如果有则替换， 否则向下继续搜索。

以题目的例子

A =`["ab","aba"]`, B =`["cc","ccc"]`, S =`"ababa"`

dict = {

2: {'ab': 0}

3: {'aba' :1}

}

从s.substr\(0, 3\) 开始搜索， 发现可以把a\[1\]替换成b\[1\], 继续搜索，不能搜索长度3， 直接搜索长度2， 未果， 结束。最后为：cccba

网上有直接自己写hash的方法， 我个人认为没有必要，而且也不会变成一个production usable 的代码。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param a: The A array
     * @param b: The B array
     * @param s: The S string
     * @return: The answer
     */
    string stringReplace(vector<string> &a, vector<string> &b, string &s) {
        // Write your code here
        if (s.empty() || a.empty() || b.empty())
            return s;
        unordered_map<int /*length*/, unordered_map<string /*input of a*/, int /*index*/>> table;
        for (int i = 0; i < a.size(); i++)
            table[a[i].size()][a[i]] = i;
        vector<int> keys;
        for(const auto& eachLen : table)
            keys.push_back(eachLen.first);
        sort(keys.begin(), keys.end(), std::greater<int>()); // hight to low;
        string res;
        int i = 0;
        while( i < s.size())
        {
            bool found = false;
            for (const auto& len : keys)
            {
                string token = s.substr(i, len);
                if (table[len].find(token) == table[len].end())
                    continue;
                else
                {
                    found = true;
                    res += b[table[len][token]];
                    i+=len;
                    break;
                }
            }
            if (!found)
            {
                res.push_back(s[i++]);
            }
        }
        return res;
    }
};
```

### 复杂度分析:

哈希的时候是有复杂度的， 所以 复杂度为 O\( average string length in a \* size of a \* size of s\) = o \(kmn\)

## 594 strStr II

Implement`strStr`function in O\(n + m\) time.

`strStr`return the first index of the target string in a source string. The length of the target string is_m\_and the length of the source string is\_n_.  
If target does not exist in source, just return -1.

Have you met this question in a real interview?

Yes

**Example**

Given source =`abcdef`, target =`bcd`, return`1`.

[http://www.lintcode.com/en/problem/strstr-ii/\#](http://www.lintcode.com/en/problem/strstr-ii/#)

### 解题分析:

这

道题目其实是考察背诵的，无力吐槽，做一遍robin-karp

robin-karp背后的思想很容易理解  
：

比如，abcde -&gt; cde  
， strstr里用了双重循环。 第一层为abcde 遍历, 第二重为cde遍历， 从a-&gt;b-&gt;c都需要从头到尾比一遍。 rk算法就是解决不需要从头到尾比的问题， 首先要hash \(cde\) = hashTarget, 那么分别去和 hash\(abc\), hash\(bcd\), hash \(cde\)来比较， 假设hash是一个o\(1\)的算法， 那么实际上可以达到O\(m+n） 的复杂度。 但是实际问题是hash是一个o\(targetLen\)的复杂度的算法。 如何使他变成O\(1\)

hash\(abc\) = \(a\*BASE^2 + b\* BASE^1 +c\) % MOD ;

当移到hash\(bcd\)的时候 hash\(bcd\) = \(hash\(abcd\) - a\*BASE^3 + MOD\) %MOD, 实际上一步就可以求得

当我们发现hash\(cde\) = hash\(target）的时候， 因为hash相等只是必要条件， 序言验证是否充分，则需要从头比较。

要注意的细节如下：

BASE 取 31 or 33, 为什么？ 经验值

MOD 取 1E5， 为什么？ 如果是32位机器，不会overflow \(lintcode跑在32位机上， 试出来的\)

掌握 hash： hash = \(hash\*BASE+c\) % MOD hash start with 0

迭代更新的时候如果小于0， 加上MOD即可

### 代码：

```cpp
class Solution {
public:
    /*
     * @param source: A source string
     * @param target: A target string
     * @return: An integer as index
     */
    int strStr2(const char* source, const char* target) {
        // write your code here

        if (!source || !target)
            return -1;
        if (!*target)
            return 0;

        // abcde ,  bcd 
        int sourceLen = strlen(source);
        int targetLen = strlen(target);

        // step 1. define hash
        int BASE = 31;
        int MOD = 1e6;

        // step 2. hash target
        int targetHash = 0;
        for (int i = 0; i < targetLen; i++ )
            targetHash = (targetHash * BASE + target[i]) % MOD;

        // step 2.5, prepare pow, why? we need it in step 3
        int power = 1;
        for (int i = 0; i < targetLen; i++ )
            power = power * BASE % MOD;
        // step 3 match
        int hashSource = 0;
        for (int i = 0; i < sourceLen; i++)
        {
            hashSource = (hashSource * BASE + source[i]) % MOD;
            if (i < targetLen-1)
                continue;
            // abcd -a
            if (i >= targetLen )
            {
                hashSource = (hashSource - source[i - targetLen] * power ) % MOD;
                if (hashSource < 0)
                    hashSource += MOD;
            }
            if (hashSource == targetHash)
            {
                bool found = true;
                for (int j = 0; j < targetLen; j++)
                {
                    if (source[i-targetLen+j+1] != target[j])
                    {
                        found = false;
                        break;
                    }
                }
                if (found)
                    return i-targetLen+1;
            }
        }

        return -1;
    }
};
```

### 复杂度分析:

o\(m+n\), m = source.size\(\), n=target.size\(\)


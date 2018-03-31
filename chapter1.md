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

[http://www.lintcode.com/en/problem/valid-palindrome/](http://www.lintcode.com/en/problem/strstr/#)

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



## 200 Longest Palindromic Substring

Given a string`S`, find the longest palindromic substring in`S`. You may assume that the maximum length of`S`is 1000, and there exists one unique longest palindromic substring.

Have you met this question in a real interview?

Yes

**Example**

Given the string =`"abcdzdcab"`, return`"cdzdc"`.

http://www.lintcode.com/en/problem/longest-palindromic-substring/



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

2. 中心射线法

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




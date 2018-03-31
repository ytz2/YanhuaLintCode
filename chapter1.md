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

```
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

```
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

http://www.lintcode.com/en/problem/valid-palindrome/

### 解题分析:

在627中的解题分析中已经写了如何判断是否为回文的代码， 但是不满足此题目的要求， 此题要求字母必须为a-zA-Z0-9, 其余可以忽略。 那么就有一些corner case需要考虑， 列举如下：

1. 空字符串
2. 忽略大小写
3. 忽略非字符

实现非常简单。

### 代码：

```
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








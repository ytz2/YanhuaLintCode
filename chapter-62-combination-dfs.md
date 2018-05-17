## \*\*\*\*652. Factorization

A non-negative numbers can be regarded as product of its factors.  
Write a function that takes an integer n and return all possible combinations of its factors.

##### Notice

* Elements in a combination \(a1, a2, … , ak\) must be in non-descending order. \(ie, a1 ≤ a2 ≤ … ≤ ak\).
* The solution set must not contain duplicate combination.

Have you met this question in a real interview?

Yes

**Example**

Given n =`8`  
return`[[2,2,2],[2,4]]`  
// 8 = 2 x 2 x 2 = 2 x 4.

Given n =`1`  
return`[]`

Given n =`12`  
return`[[2,6],[2,2,3],[3,4]]`

[https://www.lintcode.com/en/old/problem/factorization/](https://www.lintcode.com/en/old/problem/factorization/)

### 解题分析:

这道题自己把自己误导了，本来想的是先求得所有的质数因子，然后再想办法组合，但是为了保证升序且unique，组合的时候无比痛苦。

看了别人的答案写了第一版：

把分解因子的过程放到DFS中去

下面这个过程保证了质因子从小到大排列

但是如果要合并的话，则可以从i=2开始一直到n， 回溯i， 然后继续分解n/i， 这样就可以使得按照升序排列了

比如12

第一次遍历

2    。。。。

3  4

~~4  3， 去掉重复 非升序~~

~~6 余下 2， 去掉，因为重复， 非升序~~

~~12 去掉，只有一个~~

保留以2开头的和以3开头的，其余不要，非升序

以2开头 可以继续我 **2,2，3 **或者 **2， 6**

通过观察发现: 如果要保证 升序， 则因子要在 2 - sqrt\(12\)中间找， 比如， 4， 就不可以 因为 12/4 &lt; 4,所以就会被去掉

因为要保证下一个升序，则 当前以x开头， 则下一次分解因子应该以x开始

递归的出口n &lt;=1

递归的定义：

for（ start : sqrt\(n\)\)

```
    sub.push\(i\)

    dfs

    sub.pop\(\)
```

但是有一个问题就是

我们限定了只遍历到sqrt\(n\)  那么就有一个问题， 我们限定在start到sqrt\(n\)中间找 ， 比如12， 在\[2, 3\]找 然后余下6， 在\[2, 2\]找，我们就会漏掉6本身这个解，所以当范围限定在【x,x】的时候我们要看一下n&gt;= start的情况，把n也加到现有的subset中

```
for (int i=2; i<=n;i++)
 while(n2%i==0)
    n2/=i
    ....
```

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: a list of combination
     */
    vector<vector<int>> getFactors(int n) {
        // write your code here
        vector<vector<int>> results;
        vector<int> subset;
        dfs( n, subset, 2, results);
        return results;
    }

    void dfs(int n, vector<int>& subset, int start, vector<vector<int>>& results)
    {
        if (n <= 1)
        {
            if (subset.size() > 1)
                results.push_back(subset);
            return;
        }

        for (int i = start; i <= sqrt(n); i++)
        {
            if (n % i != 0)
                continue;
            subset.push_back(i);
            dfs(n/i, subset, i, results);
            subset.pop_back();
        }

        if (n >= start )
        {
            subset.push_back(n);
            dfs(1, subset, n, results);
            subset.pop_back();
        }
    }
};
```

### 复杂度分析:

?

## 426. Restore IP Addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

Have you met this question in a real interview?

Yes

**Example**

Given`"25525511135"`, return

```
[
  "255.255.11.135",
  "255.255.111.35"
]
```

Order does not matter.

[https://www.lintcode.com/en/old/problem/restore-ip-addresses/](https://www.lintcode.com/en/old/problem/restore-ip-addresses/)

### 解题分析:

一个有效的IP地址由四个数字组成 xxx.xxx.xxx.xxx每一个xxx在0-255之间那么就是看是不是最后有四个0-255的数字组成。那么这道题还是一个选还是不选的问题

### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: the IP string
     * @return: All possible valid IP addresses
     */
    vector<string> restoreIpAddresses(string &s) {
        // write your code here
        vector<string> results;
        if (s.empty())
            return results;
        vector<int> subset;
        dfs(s, 0, results, subset);
        return results;
    }

    void dfs(const string&s, int startIndex, vector<string>& results, vector<int>& subset)
    {
        if (startIndex >= s.size() )
        {
            if (subset.size() == 4)
            {
                string res;
                for(int i = 0; i < 4; i++)
                    res += to_string(subset[i])+".";
                res.pop_back();
                results.push_back(res);
            }
            return;
        }

        if (subset.size() >= 4)
            return;

        for (int i = 1; i <= 3; i++)
        {
            if (startIndex+i > s.size())
                break;
            auto str = s.substr(startIndex, i);
            int n = stoi(str);
            if (n == 0 && i == 1)
            {
                subset.push_back(n);
                dfs(s, startIndex+1, results, subset);
                subset.pop_back();
                break;
            }
            if (n>255)
                break;
            subset.push_back(n);
            dfs(s, startIndex+i, results, subset);
            subset.pop_back();
        }

    }
};
```

### 复杂度分析:

2^n

## 427. Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Have you met this question in a real interview?

Yes

**Example**

Given`n = 3`, a solution set is:

`"((()))", "(()())", "(())()", "()(())", "()()()"`

[https://www.lintcode.com/en/old/problem/generate-parentheses/](https://www.lintcode.com/en/old/problem/generate-parentheses/)

### 解题分析:

这道题考的不是写code,考的是是不是知道结论， i &lt;n-1 l&gt;= r i = n-1 l =r 才是valid

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: n pairs
     * @return: All combinations of well-formed parentheses
     */
    vector<string> generateParenthesis(int n) {
        // write your code here
        vector<string> results;
        string subset;
        dfs(0, 2*n, 0, 0, subset, results );
        return results;
    }

    void dfs(int ind, int n, int l, int r, string& subset, vector<string>& results)
    {
        if ( ind == n || l < r  )
        {
            if (l == r)
                results.push_back(subset);
            return;
        }

        subset.push_back('(');
        dfs(ind+1, n, l+1, r, subset, results);
        subset.pop_back();

        subset.push_back(')');
        dfs(ind+1, n, l, r+1, subset, results);
        subset.pop_back();
    }

};
```

### 复杂度分析:

2^n

## 196. Missing Number

Given an array contains_N\_numbers of 0 ..\_N_, find which number doesn't exist in the array.

Have you met this question in a real interview?

Yes

**Example**

Given_N_=`3`and the array`[0, 1, 3]`, return`2`.

[https://www.lintcode.com/en/old/problem/missing-number/](https://www.lintcode.com/en/old/problem/missing-number/)

### 解题分析:

o\(n\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: An array of integers
     * @return: An integer
     */
    int findMissing(vector<int> &nums) {
        // write your code here
        if (nums.empty())
            return 0;
        int n = nums.size();
        int sum = n*(n+1)/2;
        for (const auto& each : nums)
            sum -= each;
        return sum;
    }
};
```

### 复杂度分析:

o\(n\)

## 107. Word Break

Given a string s and a dictionary of words dict, determine if s can be break into a space-separated sequence of one or more dictionary words.

Have you met this question in a real interview?

Yes

**Example**

Given s =`"lintcode"`, dict =`["lint", "code"]`.

Return true because**"lintcode"**can be break as`"lint code"`.

[https://www.lintcode.com/en/old/problem/word-break/\#](https://www.lintcode.com/en/old/problem/word-break/#)

### 解题分析:

是否可以，一般都是宽搜，深搜是求具体的所有方案的问题， 就这道题而言，宽搜好过深搜，没必要写深搜,

### 代码：

```cpp
class Solution {
public:
    /*
     * @param s: A string
     * @param dict: A dictionary of words dict
     * @return: A boolean
     */
    bool wordBreak(string &s, unordered_set<string> &dict) {
        // write your code here
        if (s.empty() && dict.empty())
            return true;
        if (s.empty() || dict.empty())
            return false;
        set<int> lenSize;
        for (const auto& each: dict)
            lenSize.insert(each.size());
        return dfs(s, 0, lenSize, dict);
    }

    bool dfs(string& s, int startIndex, set<int>& lenSize, unordered_set<string>& dict)
    {
        if (startIndex >= s.size())
            return true;
        for (const auto& len : lenSize)
        {
            auto str = s.substr(startIndex, len );
            if (dict.find(str) == dict.end())
                continue;
            if (dfs(s, startIndex+len, lenSize, dict))
                return true;
        }
        return false;
    }

};
```

### 复杂度分析:

2^n

## 683. Word Break III

Give a dictionary of words and a sentence with all whitespace removed, return the number of sentences you can form by inserting whitespaces to the sentence so that each word can be found in the dictionary.

##### Notice

Ignore case

Have you met this question in a real interview?

Yes

**Example**

Given a String`CatMat`  
Given a dictionary`["Cat", "Mat", "Ca", "tM", "at", "C", "Dog", "og", "Do"]`  
return`3`

we can form 3 sentences, as follows:  
`CatMat = Cat Mat`  
`CatMat = Ca tM at`  
`CatMat = C at Mat`

[https://www.lintcode.com/en/old/problem/word-break-iii/](https://www.lintcode.com/en/old/problem/word-break-iii/)

### 解题分析:

和上题不尽相同, 但是中间提交的时候evolve了两次

1。 第一次是没有看到忽略大小写

2。 第二次是超时了， 看了一眼可以用Memorized search来解决

### 代码：

```cpp
class Solution {
public:
    /*
     * @param : A string
     * @param : A set of word
     * @return: the number of possible sentences.
     */
    int wordBreak3(string& s, unordered_set<string>& dict) {
        // Write your code here
        if (s.empty() && dict.empty())
            return 1;
        if (s.empty() || dict.empty())
            return 0;
        toLower(s);
        set<int> lenSize;
        unordered_set<string> dup;
        for (auto each : dict)
            if (!each.empty())
            {
                lenSize.insert(each.size());
                toLower(each);
                dup.insert(each);
            }
        return dfs(s, 0, lenSize, dup);
    }

private:
    void toLower(string& s)
    {
        for (size_t i=0; i<s.size(); i++)
            s[i] = tolower(s[i]);
    }

    int dfs(string& s, int startIndex, set<int>& lenSize, unordered_set<string>& dict)
    {
        if (startIndex >= s.size())
            return 1;
        if (counts_.find(startIndex) != counts_.end())
            return counts_[startIndex];
        int res = 0;
        for (const auto& len : lenSize)
        {
            if (startIndex + len > s.size() )
                continue;
            auto str = s.substr(startIndex, len);
            if (dict.find(str) == dict.end())
                continue;
            res += dfs(s, startIndex + len, lenSize, dict);
        }
        counts_[startIndex] = res;
        return res;
    }

    unordered_map<int, int> counts_; 
};
```

### 复杂度分析:

n^2 懒得分析mem search, 如果是DP肯定是n^2 :\)

## 108. Palindrome Partitioning II

Given a string_s_, cut\_s\_into some substrings such that every substring is a palindrome.

Return the**minimum**cuts needed for a palindrome partitioning of_s_.

**Example**

Given s =`"aab"`,

Return`1`since the palindrome partitioning \["aa", "b"\] could be produced using\_1\_cut.

[https://www.lintcode.com/en/old/problem/palindrome-partitioning-ii/\#](https://www.lintcode.com/en/old/problem/palindrome-partitioning-ii/#)

### 解题分析:

这种题目一看就是DP问题， 放在DFS这里刷，就用DFS来解决吧

### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: A string
     * @return: An integer
     */
    int minCut(string &s) {
        // write your code here
        if (s.empty())
            return 0;
        return dfs(s, 0)-1;
    }

    int dfs(string& s, int startIndex)
    {
        if (startIndex >= s.size())
            return 0;
        int res = INT_MAX;
        for (int i = startIndex; i < s.size(); i++)
        {
            if (isValid(s, startIndex, i))
            {
                if (count_.find(i+1) == count_.end())
                    res = min(res, 1+ dfs(s, i+1));
                else
                    res = min(res, 1+count_[i+1]);
            }
        }

        count_[startIndex] = res;
        return res;
    }

    bool isValid(const string& s, int beg, int end)
    {
        while(beg < end)
        {
            if (s[beg] != s[end])
                return false;
            beg++; end--;
        }
        return true;
    }
    unordered_map<int, int> count_;
};
```

### 




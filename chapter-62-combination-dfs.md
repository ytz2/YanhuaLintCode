## 652. Factorization

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


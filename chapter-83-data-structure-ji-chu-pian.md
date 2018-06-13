## 128. Hash Function

In data structure Hash, hash function is used to convert a string\(or any other type\) into an integer smaller than hash size and bigger or equal to zero. The objective of designing a hash function is to "hash" the key as unreasonable as possible. A good hash function can avoid collision as less as possible. A widely used hash function algorithm is using a magic number 33, consider any string as a 33 based big integer like follow:

hashcode\("abcd"\) = \(ascii\(a\) \* 333+ ascii\(b\) \* 332+ ascii\(c\) \*33 + ascii\(d\)\) % HASH\_SIZE

```
                          = \(97\* 333+ 98 \* 332 + 99 \* 33 +100\) % HASH\_SIZE

                          = 3595978 % HASH\_SIZE
```

here HASH\_SIZE is the capacity of the hash table \(you can assume a hash table is like an array with index 0 ~ HASH\_SIZE-1\).

Given a string as a key and the size of hash table, return the hash value of this key.f

### Example

For key="abcd" and size=100, return 78

[https://www.lintcode.com/problem/hash-function/description](https://www.lintcode.com/problem/hash-function/description)

### 解题分析:

老生常谈

### 代码：

```cpp
class Solution {
public:
    /**
     * @param key: A string you should hash
     * @param HASH_SIZE: An integer
     * @return: An integer
     */
    int hashCode(string &key, int HASH_SIZE) {
        // write your code here
        long res = 0;
        for (auto& c : key)
            res = (res*33 + c)%HASH_SIZE;
        return res;
    }
};
```

## 685. First Unique Number In Stream

Given a continuous stream of numbers, write a function that returns the first unique number whenever terminating number is reached\(include terminating number\). If there no unique number before terminating number or you can't find this terminating number, return`-1`.

### Example

Given a stream`[1, 2, 2, 1, 3, 4, 4, 5, 6]`and a number`5`  
return`3`

Given a stream`[1, 2, 2, 1, 3, 4, 4, 5, 6]`and a number`7`  
return`-1`

https://www.lintcode.com/problem/first-unique-number-in-stream/description

### 解题分析:

这种类型的题目做多了就知道肯定是hash table + double linked list组合， 这个不新鲜，加了一个unordered\_set而已

### 代码：

```cpp
#include <list>
class Solution {
public:
    /**
     * @param nums: a continuous stream of numbers
     * @param number: a number
     * @return: returns the first unique number
     */
    int firstUniqueNumber(vector<int> &nums, int number) {
        // Write your code here
        list<int> l;
        unordered_map<int, list<int>::iterator> map_;
        unordered_set<int> dup_;
        
        bool found = false;
        for (int i = 0; i <nums.size(); i++)
        {
            int v = nums[i];
            if (dup_.count(v))
                continue;
            if (map_.count(v))
            {
                l.erase(map_[v]);
                dup_.insert(v);
                continue;
            }
            l.push_back(v);
            map_[v] = prev(l.end());
            if (v == number)
            {
                found = true;
                break;
            }
        }
        if (l.empty() || !found)
            return -1;
        return l.front();
    }
};
```




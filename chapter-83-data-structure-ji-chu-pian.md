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




## 607 Two Sum - Data structure design

Design and implement a TwoSum class. It should support the following operations:`add`and`find`.

`add`- Add the number to an internal data structure.  
`find`- Find if there exists any pair of numbers which sum is equal to the value.

Have you met this question in a real interview?

Yes

**Example**

```
add(1); add(3); add(5);
find(4) // return true
find(7) // return false
```

http://www.lintcode.com/en/problem/two-sum-data-structure-design/

### 解题分析:

挺好玩的一个题目， two sum算是经典题了，但这道题是让选数据结构。 什么样的数据结构插入简单搜索也简单？  

如果从two sum那道题过来，要求storage是排好的， 且允许重复出现。（multiset\) . 而且搜索的最优定为 o\(n\) 两根指针 

### 代码：

```cpp
class TwoSum {
public:
    /*
     * @param number: An integer
     * @return: nothing
     */
    void add(int number) {
        // write your code here
        store.insert(number);
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    bool find(int value) {
        // write your code here
        auto beg = store.begin();
        auto end = --store.end();
        while(beg != end)
        {
            int sum = *beg+*end;
            if (sum == value)
                return true;
            else if (sum > value)
                --end;
            else
                ++beg;
        }
        return false;
    }
    
    multiset<int> store;
};
```

### 复杂度分析:

add: o\(log\(n\)\) find o\(n\)

或者不指望排序，但是用unordered\_map&lt;int,int&gt; key是数字，value是count，这样就可以存多个数字了，insert是O（1）, find O\(n\)

```
class TwoSum {
public:
    /*
     * @param number: An integer
     * @return: nothing
     */
    void add(int number) {
        // write your code here
        store[number]++;
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    bool find(int value) {
        // write your code here
        for (auto each : store)
        {
            auto expected = value - each.first;
            if (expected == each.first)
            {
                if (each.second > 1)
                    return true;
                continue;
            }
                
            if (store.find(expected) !=store.end())
                return true;
        }
        return false;
    }
    
    unordered_map<int, int> store;
};
```




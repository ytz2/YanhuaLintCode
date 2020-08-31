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

## 1429. First Unique Number In Stream

You have a queue of integers, you need to retrieve the first unique integer in the queue.

Implement the`FirstUnique` class:

* `FirstUnique(int[] nums)`
  Initializes the object with the numbers in the queue.
* `int showFirstUnique()`
   returns the value of
  **the first unique**
  integer of the queue, and returns
  **-1**
  if there is no such integer.
* `void add(int value)`
   insert value to the queue.

[https://leetcode.com/problems/first-unique-number/](https://leetcode.com/problems/first-unique-number/)

### 解题分析:

这种类型的题目做多了就知道肯定是hash table + double linked list组合， 这个不新鲜，加了一个unordered\_set而已

### 代码：

```cpp
class FirstUnique {
public:
    FirstUnique(vector<int>& nums) {
        for (const auto num : nums) {
            add(num);
        }
    }

    int showFirstUnique() {
        return q.empty()? -1 : q.front();
    }

    void add(int value) {
        if (iters.count(value)) {
            q.erase(iters[value]);
            iters.erase(value);
        } else if (!collection.count(value)) {
            q.push_back(value);
            iters[value] = prev(q.end());
        }
        collection.insert(value);
    }

private:
    list<int> q;
    unordered_map<int, list<int>::iterator> iters; 
    unordered_set<int> collection;
};

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique* obj = new FirstUnique(nums);
 * int param_1 = obj->showFirstUnique();
 * obj->add(value);
 */
```

## 613. High Five

There are two properties in the node student`id`and`scores`, to ensure that each student will have at least 5 points, find the average of 5 highest scores for each person.

### Example

```
Given results = [[1,91],[1,92],[2,93],[2,99],[2,98],[2,97],[1,60],[1,58],[2,100],[1,61]]

Return
```

[https://www.lintcode.com/problem/high-five/description](https://www.lintcode.com/problem/high-five/description)

### 解题分析:

top k 的一个复杂版。。。。

### 代码：

```cpp
/**
 * Definition for a Record
 * class Record {
 * public:
 *   int id, score;
 *   Record(int id, int score) {
 *     this->id = id;
 *     this->score = score;
 *   }
 * };
 */
class Solution {
public:
    /**
     * @param results a list of <student_id, score>
     * @return find the average of 5 highest scores for each person
     * map<int, double> (student_id, average_score)
     */
    map<int, double> highFive(vector<Record>& results) {
        // Write your code here
        map<int, double> result;
        typedef priority_queue<int, vector<int>, greater<int>> pqType;
        unordered_map<int, pqType> tbl;
        for (auto& each : results)
        {
            auto& pq = tbl[each.id];
            if (pq.size() < 5)
            {
                pq.push(each.score);
                continue;
            }

            if (pq.top() >= each.score)
                continue;
            pq.pop();
            pq.push(each.score);
        }
        for (auto& pair : tbl)
        {
            auto& id = pair.first;
            auto& pq = pair.second;
            double sum = 0;
            while(!pq.empty())
            {
                sum += pq.top();
                pq.pop();
            }
            result[id] = sum / 5;
        }
        return result; 
    }
};
```

## 606. Kth Largest Element II

Find K-th largest element in an array. and N is much larger than k.

### Example

In array`[9,3,2,4,8]`, the`3rd`largest element is`4`.

In array`[1,2,3,4,5]`, the`1st`largest element is`5`,`2nd`largest element is`4`,`3rd`largest element is`3`and etc.

[https://www.lintcode.com/problem/kth-largest-element-ii/description](https://www.lintcode.com/problem/kth-largest-element-ii/description)

### 解题分析：

暗示用pq也可以过。。。写个quick select的版本算了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an integer unsorted array
     * @param k: an integer from 1 to n
     * @return: the kth largest element
     */
    int kthLargestElement2(vector<int> &nums, int k) {
        // write your code here
        return helper(nums, 0, nums.size()-1, k);
    }

    int helper(vector<int>& nums, int beg, int end, int k)
    {
        if (beg >= end)
            return nums[end];
        int mid = nums[(beg + end)/2];
        int s = beg, e = end;
        while ( s <= e)
        {
            while( s <= e && nums[s] > mid)
                s++;
            while( s <= e && nums[e] < mid)
                e--;
            if (s <= e )
                swap(nums[s++], nums[e--]);
        }
        if (beg + k - 1 <= e )
            return helper(nums, beg, e, k);
        else if ( beg + k - 1 >= s)
            return helper(nums, s, end, k - (s-beg));
        return nums[e+1];
    }
};
```

## 601. Flatten 2D Vector

Implement an iterator to flatten a 2d vector.

### Example

Given 2d vector =

```
[
  [1,2],
  [3],
  [4,5,6]
]
```

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:`[1,2,3,4,5,6]`.

[https://www.lintcode.com/problem/flatten-2d-vector/description](https://www.lintcode.com/problem/flatten-2d-vector/description)

### 解题分析：

维护i, j指针即可

### 代码：

```cpp
class Vector2D {
public:
    Vector2D(vector<vector<int>>& vec2d)
        : vect_(vec2d), i(0),j(0)
    {
        // Initialize your data structure here
    }

    int next() {
        // Write your code here
        int val = vect_[i][j];
        if (j == vect_[i].size()-1)
        {
            i++;j=0;
        }
        else
        {
            j++;
        }
        return val;
    }

    bool hasNext() {

        return (i<vect_.size()-1) || (i == vect_.size()-1 && j < vect_[i].size());
    }

private:
    vector<vector<int>>& vect_;
    int i , j;
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i(vec2d);
 * while (i.hasNext()) cout << i.next();
 */
```

## 545. Top k Largest Numbers II

mplement a data structure, provide two interfaces:

1. `add(number)`
   . Add a new number in the data structure.
2. `topk()`
   . Return the top
   _k_
   largest numbers in this data structure.
   _k_
   is given when we create the data structure.

### Example

```
s = new Solution(3);

>
>
 create a new data structure.
s.add(3)
s.add(10)
s.topk()

>
>
 return [10, 3]
s.add(1000)
s.add(-99)
s.topk()

>
>
 return [1000, 10, 3]
s.add(4)
s.topk()

>
>
 return [1000, 10, 4]
s.add(100)
s.topk()

>
>
 return [1000, 100, 10]
```

[https://www.lintcode.com/problem/top-k-largest-numbers-ii/description](https://www.lintcode.com/problem/top-k-largest-numbers-ii/description)

### 解题分析：

没新意

### 代码：

```cpp
class Solution {
public:
    /*
    * @param k: An integer
    */Solution(int k) {
        // do intialization if necessary
        k_= k;
    }

    /*
     * @param num: Number to be added
     * @return: nothing
     */
    void add(int num) {
        // write your code here
        if (store.size() < k_)
            store.insert(num);
        else if (num > *store.begin())
        {
            store.erase(store.begin());
            store.insert(num);
        }

    }

    /*
     * @return: Top k element
     */
    vector<int> topk() {
        // write your code here
       vector<int> result;
       int i = 0; 
       for (auto it = store.rbegin(); it!=store.rend() && i < k_; i++, it++)
       {
           result.push_back(*it);
       }
       return result;
    }
    int k_;
    multiset<int> store;
};
```

## 526. Load Balancer

Implement a load balancer for web servers. It provide the following functionality:

1. Add a new server to the cluster =
   &gt;
   `add(server_id)`
   .
2. Remove a bad server from the cluster =
   &gt;
   `remove(server_id)`
   .
3. Pick a server in the cluster randomly with equal probability =
   &gt;
   `pick()`
   .

### Example

At beginning, the cluster is empty =&gt;`{}`.

```
add(1)
add(2)
add(3)
pick()

>
>
 1         // the return value is random, it can be either 1, 2, or 3.
pick()

>
>
 2
pick()

>
>
 1
pick()

>
>
 3
remove(1)
pick()

>
>
 2
pick()

>
>
 3
pick()

>
>
 3
```

[                            
](https://www.lintcode.com/problem/load-balancer/description)[https://www.lintcode.com/problem/load-balancer/description](https://www.lintcode.com/problem/load-balancer/description)

### 解题分析：

和那个random的是一道题

### 代码：

```cpp
class LoadBalancer {
public:
    LoadBalancer() {
        // do intialization if necessary
        srand(time(NULL));
        n = 0;
    }

    /*
     * @param server_id: add a new server to the cluster
     * @return: nothing
     */f
    void add(int server_id) {
        // write your code here
        if (indices_.count(server_id))
            return;
        indices_[server_id] = n;
        if (n < store_.size())
            store_[n] = server_id;
        else
            store_.push_back(server_id);
        n++;
    }

    /*
     * @param server_id: server_id remove a bad server from the cluster
     * @return: nothing
     */
    void remove(int server_id) {
        // write your code here
        if (!indices_.count(server_id))
            return;
        store_[indices_[server_id]] = store_[n-1];
        indices_[store_[n-1]] = indices_[server_id];
        indices_.erase(server_id);
        n--;
    }

    /*
     * @return: pick a server in the cluster randomly with equal probability
     */
    int pick() {
        // write your code here
        return store_[rand()%n];
    }

    unordered_map<int, int> indices_;
    vector<int> store_;
    int n;
};
```

## 486. Merge K Sorted Arrays

Given\_k\_sorted integer arrays, merge them into one sorted array.

### Example

Given 3 sorted arrays:

```
[
  [1, 3, 5, 7],
  [2, 4, 6],
  [0, 8, 9, 10, 11]
]
```

return`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`.

### Challenge

Do it in O\(N log k\).

* _N_
  is the total number of integers.
* _k_
  is the number of arrays.

[                            
](https://www.lintcode.com/problem/load-balancer/description)[https://www.lintcode.com/problem/merge-k-sorted-arrays/description](https://www.lintcode.com/problem/merge-k-sorted-arrays/description)

### 解题分析：

变种题

### 代码：

```cpp
class Solution {
public:
    /**
     * @param arrays: k sorted integer arrays
     * @return: a sorted array
     */
    struct CmpOp{
      bool operator()(const std::pair<int,int>& left, const std::pair<int,int>& right)
      {
          return left.first > right.first;
      }
    };
    vector<int> mergekSortedArrays(vector<vector<int>> &arrays) {
        // write your code here
        vector<int> res;
        vector<int> cur(arrays.size(), 0);
        priority_queue<pair<int,int>, vector<pair<int,int>>, CmpOp> pq;
        for ( int i = 0; i < arrays.size(); i++)
        {
            if (cur[i] == arrays[i].size())
                continue;
            pq.push(make_pair(arrays[i][cur[i]], i));
            cur[i]++;
        }

        while(!pq.empty())
        {
            auto p = pq.top();
            pq.pop();
            res.push_back(p.first);
            if (cur[p.second] == arrays[p.second].size())
                continue;
            pq.push(make_pair(arrays[p.second][cur[p.second]], p.second));
            cur[p.second]++;
        }

        return res;
    }
};
```

## 129. Rehashing

The size of the hash table is not determinate at the very beginning. If the total size of keys is too large \(e.g. size &gt;= capacity / 10\), we should double the size of the hash table and rehash every keys. Say you have a hash table looks like below:

`size=3`,`capacity=4`

```
[null, 21, 14, null]
       ↓    ↓
       9   null
       ↓
      null
```

The hash function is:

```
int hashcode(int key, int capacity) {
    return key % capacity;
}
```

here we have three numbers, 9, 14 and 21, where 21 and 9 share the same position as they all have the same hashcode 1 \(21 % 4 = 9 % 4 = 1\). We store them in the hash table by linked list.

rehashing this hash table, double the capacity, you will get:

[   https://www.lintcode.com/problem/rehashing/description                        
](https://www.lintcode.com/problem/load-balancer/description)

### 解题分析：

1. hash ： \(k+size\)%size 负数 2. 放一个buffer存当前key的位置

### 代码：

```cpp
/**
 * Definition of ListNode
 * class ListNode {
 * public:
 *     int val;
 *     ListNode *next;
 *     ListNode(int val) {
 *         this->val = val;
 *         this->next = NULL;
 *     }
 * }
 */
class Solution {
public:
    /**
     * @param hashTable: A list of The first node of linked list
     * @return: A list of The first node of linked list which have twice size
     */    
    vector<ListNode*> rehashing(vector<ListNode*> hashTable) {
        // write your code here
        int size = hashTable.size()*2;
        vector<ListNode*> res(size, nullptr);
        vector<ListNode*> cur(size, nullptr);
        for (auto ptr : hashTable)
        {
            if (!ptr)
                continue;
            while(ptr)
            {
                int k = (ptr->val+size) % size;
                if (!cur[k])
                {
                    res[k] = ptr;
                    cur[k] = ptr;
                }
                else
                {
                    cur[k]->next = ptr;
                    cur[k] = ptr;
                }
                ptr = ptr->next;
            }
        }
        for (auto ptr : cur)
        {
            if (ptr) ptr->next = nullptr;
        }
        return res;
    }
};
```

## 124. Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

### Example

Given`[100, 4, 200, 1, 3, 2]`,  
The longest consecutive elements sequence is`[1, 2, 3, 4]`. Return its length:`4`.

### 解题分析

这个题目要求O\(n\)的解，非人力所能及。。。。背诵类题目了。。。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param num: A list of integers
     * @return: An integer
     */
    int longestConsecutive(vector<int> &num) {
        // write your code here
        unordered_set<int> store(num.begin(), num.end());
        int longest = 0;
        for (int i = 0; i < num.size(); i++)
        {
            int down = num[i]-1, up = num[i]+1;
            while(store.count(down))
            {
                store.erase(down);
                down--;
            }
            while(store.count(up))
            {
                store.erase(up);
                up++;
            }
            longest = max(longest, up - down - 1);
        }
        return longest;
    }
};
```

## 551. Nested List Weight Sum

Given a nested list of integers, return the sum of all integers in the list weighted by their depth. Each element is either an integer, or a list -- whose elements may also be integers or other lists.

### Example

Given the list`[[1,1],2,[1,1]]`, return`10`. \(four 1's at depth 2, one 2 at depth 1, 4 \* 1 \* 2 + 1 \* 2 \* 1 = 10\)  
Given the list`[1,[4,[6]]]`, return`27`. \(one 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4 \* 2 + 6 \* 3 = 27\)

[https://www.lintcode.com/problem/nested-list-weight-sum/description](https://www.lintcode.com/problem/nested-list-weight-sum/description)

### 解题分析

Easy 的题目， 简单递归就好了

### 代码：

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer,
 *     // rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds,
 *     // if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds,
 *     // if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    int depthSum(const vector<NestedInteger>& nestedList) {
        // Write your code here
        return depthSum(1, nestedList);
    }

    int depthSum(int depth, const vector<NestedInteger>& nestedList)
    {
        int sum = 0;
        for (const auto& each : nestedList){
            if (each.isInteger())
            {
                sum += depth * each.getInteger();
            }
            else
            {
                sum += depthSum(depth+1, each.getList());
            }
        }
        return sum;
    }
};
```

## 575. Decode String

Given an expression`s`includes numbers, letters and brackets. Number represents the number of repetitions inside the brackets\(can be a string or another expression\)．Please expand expression to be a string.

### Example

s =`abc3[a]`return`abcaaa`  
s =`3[abc]`return`abcabcabc`  
s =`4[ac]dy`, return`acacacacdy`  
s =`3[2[ad]3[pf]]xyz`, return`adadpfpfpfadadpfpfpfadadpfpfpfxyz`

### Challenge

Can you do it without recursion?

[https://www.lintcode.com/problem/decode-string/description](https://www.lintcode.com/problem/decode-string/description)

### 解题分析

这种带括号的就是无脑上stack， 碰到左括号push状态，右括号pop更新全局。 具体看代码。

这道题用了两个stack,一个记录几个数，一个当前的partial string.

### 代码：

```cpp
class Solution {
public:
    /**
     * @param s: an expression includes numbers, letters and brackets
     * @return: a string
     */
    string expressionExpand(string &s) {
        // write your code here
        stack<int> countStk;
        stack<string> partStk;
        string result;
        int number = 0;

        for (int i = 0; i < s.size(); i++)
        {
            if (isdigit(s[i]))
            {
                number = number*10 + s[i] - '0';
            }
            else if (s[i] == '[')
            {
                countStk.push(number);
                number = 0;
                partStk.push("");
            }
            else if (s[i] == ']')
            {
                int n = countStk.top();
                countStk.pop();
                string tmp;
                for (int i = 0; i < n; i++)
                    tmp.append(partStk.top());
                partStk.pop();
                if (partStk.empty())
                    result.append(tmp);
                else
                    partStk.top().append(tmp);
            }
            else
            {
                if (partStk.empty())
                    result.push_back(s[i]);
                else
                    partStk.top().push_back(s[i]);
            }
        }
        return result;
    }
};
```

## 540. Zigzag Iterator

Given two**1d**vectors, implement an iterator to return their elements alternately.

### Example

Given two 1d vectors:

```
v1 = [1, 2]
v2 = [3, 4, 5, 6]
```

By calling next repeatedly until hasNext returns`false`, the order of elements returned by next should be:`[1, 3, 2, 4, 5, 6]`.

[https://www.lintcode.com/problem/zigzag-iterator/description](https://www.lintcode.com/problem/zigzag-iterator/description)

### 解题分析

.就是 ++cur\_%2来回捯饬

### 代码：

```cpp
class ZigzagIterator {
public:
    /*
    * @param v1: A 1d vector
    * @param v2: A 1d vector
    */
    ZigzagIterator(vector<int>& v1, vector<int>& v2) 
        : cur_(0), indices_(vector<int>{0, 0}), array_(vector<vector<int>*>{&v1, &v2})
    {
        // do intialization if necessary
    }

    /*
     * @return: An integer
     */
    int next() {
        // write your code here
        if (!hasNext())
            return -1;
        int v;
        if (indices_[cur_] != array_[cur_]->size())
        {
            v = array_[cur_]->at(indices_[cur_]);
            indices_[cur_]++;
            cur_ = ++cur_%2;
        }
        else
        {
            cur_ = ++cur_%2;
            return next();
        }
        return v;
    }

    /*
     * @return: True if has next
     */
    bool hasNext() {
        // write your code here
        int next = (cur_+1)%2;
        return !(array_[cur_]->size() == indices_[cur_] && array_[next]->size() == indices_[next]);
    }

    int cur_;
    vector<int> indices_;
    vector<vector<int>*> array_;
};

/**
 * Your ZigzagIterator object will be instantiated and called as such:
 * ZigzagIterator solution(v1, v2);
 * while (solution.hasNext()) result.push_back(solution.next());
 * Ouptut result
 */
```

## 541. Zigzag Iterator II

ollow up[Zigzag Iterator](http://www.lintcode.com/en/problem/zigzag-iterator/): What if you are given`k`1d vectors? How well can your code be extended to such cases? The "Zigzag" order is not clearly defined and is ambiguous for`k > 2`cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic".

### Example

Given`k = 3`1d vectors:

```
[1,2,3]
[4,5,6,7]
[8,9]
```

Return`[1,4,8,2,5,9,3,6,7]`.

[https://www.lintcode.com/problem/zigzag-iterator-ii/description](https://www.lintcode.com/problem/zigzag-iterator-ii/description)

### 解题分析

改用一个list存 i, ind pair了， 如果满了就erase掉， 这样都是O（1）的操作

### 代码：

```cpp
#include <list>
class ZigzagIterator2 {
public:
    /*
    * @param vecs: a list of 1d vectors
    */ZigzagIterator2(vector<vector<int>>& vecs) 
        :vectors(vecs)
    {
        // do intialization if necessary
        for (int i = 0; i < vectors.size(); i++)
        {
            if (vectors[i].empty())
                continue;
            indices_.push_back(make_pair(i,0));
        }
        if (indices_.empty())
            cur = indices_.end();
        else
            cur = indices_.begin();
    }

    /*
     * @return: An integer
     */
    int next() {
        // write your code here
        int i = cur->first;
        int ind = cur->second++;
        int v = vectors[i][ind];
        if (cur->second == vectors[i].size())
        {
            auto tmp = cur;
            cur++;
            indices_.erase(tmp);
        }
        else
        {
            cur++;
        }
        if (!indices_.empty() && cur == indices_.end())
            cur = indices_.begin();
        return v;
    }

    /*
     * @return: True if has next
     */
    bool hasNext() {
        // write your code here
        return cur != indices_.end();
    }

private:
    vector<vector<int>>& vectors;
    list<pair<int, int>> indices_;
    list<pair<int, int>>::iterator cur;
};

/**
 * Your ZigzagIterator2 object will be instantiated and called as such:
 * ZigzagIterator2 solution(vecs);
 * while (solution.hasNext()) result.push_back(solution.next());
 * Ouptut result
 */
```

## 528. Flatten Nested List Iterator

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

### Example

* Given the list`[[1,1],2,[1,1]]`, By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:`[1,1,2,1,1]`.

* Given the list`[1,[4,[6]]]`, By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:`[1,4,6]`.

[https://www.lintcode.com/problem/flatten-nested-list-iterator/description](https://www.lintcode.com/problem/flatten-nested-list-iterator/description)

### 解题分析

这个挺好玩的，自己包含自己，维护一个cur index和cur index对应的iter,剩下的全是递归了。。。

### 代码：

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer,
 *     // rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds,
 *     // if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds,
 *     // if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class NestedIterator {
public:
    NestedIterator(const vector<NestedInteger> &nestedList) 
        : nested(nestedList)
    {
        // Initialize your data structure here.
        cur = 0;
        iter = nullptr;
    }

    // @return {int} the next element in the iteration
    int next() {
        // Write your code here
        auto& node = nested[cur];
        int v;
        if (node.isInteger())
        {
            cur++;
            v = node.getInteger();
        }
        else
        {
            v = iter->next();
            if (!iter->hasNext())
            {
                delete iter;
                iter = nullptr;
                cur++;
            }
        }
        return v;
    }

    // @return {boolean} true if the iteration has more element or false
    bool hasNext() {
        // Write your code here
        if (cur == nested.size())
            return false;
        while(cur < nested.size())
        {
            auto& node = nested[cur];
            if (node.isInteger())
                return true;
            if (!iter)
                iter = new NestedIterator(node.getList());
            if (iter->hasNext())
                return true;
            delete iter;
            iter = nullptr;
            cur++;
        }
        return cur !=nested.size();
    }

    const vector<NestedInteger>& nested;
    int cur;
    NestedIterator* iter;
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) v.push_back(i.next());
 */
```

## \*\*\*\*471. Top K Frequent Words

Given a list of words and an integer k, return the top k frequent words in the list.

### Example

Given

```
[
    "yes", "lint", "code",
    "yes", "code", "baby",
    "you", "baby", "chrome",
    "safari", "lint", "code",
    "body", "lint", "code"
]
```

for k =`3`, return`["code", "lint", "baby"]`.

for k =`4`, return`["code", "lint", "baby", "yes"]`,

### Challenge

Do it in O\(nlogk\) time and O\(n\) extra space.

[https://www.lintcode.com/problem/top-k-frequent-words/description](https://www.lintcode.com/problem/top-k-frequent-words/description)

### 解题分析

不难，但是有一个和C++相关的lambda表示cmp的方法，最好学会， 还有记住pq default left  &lt; right给出来的是 max heap。

要用min heap, 要反过来一下逻辑。这里面string order坑死人

### 代码：

```cpp
#include <list>

class Solution {
public:
    /**
     * @param words: an array of string
     * @param k: An integer
     * @return: an array of string
     */
    vector<string> topKFrequentWords(vector<string> &words, int k) {
        if (k == 0)
            return vector<string>();

        unordered_map<string, int> tbl;

        auto cmp = [&](const string& left, const string& right){
          if (tbl[left] == tbl[right])
            return left < right;
          return tbl[left] > tbl[right];
        };
        for (auto& word : words)
            tbl[word]++;
        priority_queue<string, vector<string>, decltype(cmp)> pq(cmp);
        for (auto& elem : tbl)
        {
            if (pq.size() != k)
                pq.push(elem.first);
            else
            {
                auto& t = pq.top();
                if (tbl[t] < tbl[elem.first] || tbl[t] == tbl[elem.first] && elem.first < t )
                {
                    pq.pop();
                    pq.push(elem.first);
                }
            }
        }
        vector<string> res;
        while(!pq.empty())
        {
            res.push_back(pq.top());
            pq.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }


};
```




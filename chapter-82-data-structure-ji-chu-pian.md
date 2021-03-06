## 642. Moving Average from Data Stream

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

### Example

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1 // return 1.00000
m.next(10) = (1 + 10) / 2 // return 5.50000
m.next(3) = (1 + 10 + 3) / 3 // return 4.66667
m.next(5) = (10 + 3 + 5) / 3 // return 6.00000
```

[https://leetcode.com/problems/moving-average-from-data-stream/](https://leetcode.com/problems/moving-average-from-data-stream/)

### 解题分析:

一个q就好了

### 代码：

```cpp
class MovingAverage {
public:
    /** Initialize your data structure here. */
    MovingAverage(int size) {
        this->size = size;
    }

    double next(int val) {
        total += val;
        q.push(val);
        if (q.size() > size) {
            total -= q.front();
            q.pop();
        }
        return total / q.size();
    }

    int  size = 0;
    queue<int> q;
    double total = 0;
};
```

## 209. First Unique Character in a String

Find the first unique character in a given string. You can assume that there is at least one unique character in the string.

### Example

For`"abaccdeff"`, return`'b'`.

[https://www.lintcode.com/problem/first-unique-character-in-a-string/description](https://www.lintcode.com/problem/first-unique-character-in-a-string/description)

### 解题分析:

hashtable

### 代码：

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        vector<int> counter(26, 0);
        for (auto c : s) counter[c-'a']++;
        for (int i = 0; i < s.size(); i++) {
            if (counter[s[i] - 'a'] == 1) return i;
        }
        return -1;
    }
};
```

## 657. Insert Delete GetRandom O\(1\)

Design a data structure that supports all following operations in average`O(1)`time.

* `insert(val)`
  : Inserts an item val to the set if not already present.
* `remove(val)`
  : Removes an item val from the set if present.
* `getRandom`
  : Returns a random element from current set of elements. Each element must have the same probability of being returned.

### Example

```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

Input test data

\(

one parameter per line.

\)

[https://leetcode.com/problems/insert-delete-getrandom-o1/](https://leetcode.com/problems/insert-delete-getrandom-o1/)

### 解题分析:

维护一个hash table, key 是值，val是 vector的 index ， 维护一个vector, 存具体的值， 维护一个n存当前个数。

n和 vector的关系是小于或者等于vec.size\(\), push 很简单， 要么push back,要么在n位置插入并更新n

tricky的在delete:

vec\[n-1\] : 最后一个值

tbl\[val\]: val对应的索引

vec\[tbl\[val\]\], val对应在vec的值

vec\[tbl\[val\]\] = vec\[n-1\] 把最后一位占用要被删掉的位子

tbl\[vec\[n-1\]\]  = tbl\[val\] 同时更新他的新的索引

tbl.erase\(val\); n-- ; 做删除操作并把最后一位忽略

所以基本思想就是利用最后一位鸠占鹊巢，然后断尾求生。。。

### 代码：

```cpp
class RandomizedSet {
public:
    /** Initialize your data structure here. */
    RandomizedSet() {

    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if (hash.count(val)) return false;
        int size = hash.size();
        if (vals.size() < size + 1) {
            vals.push_back(val);
        } else {
            vals[size] = val;
        }
        hash[val] = size;
        return true;
    }

    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        if (!hash.count(val)) return false;
        int last = hash.size() - 1;
        int lastVal = vals[last];
        int cur = hash[val];
        vals[cur] = lastVal;
        hash[lastVal] = cur;
        hash.erase(val);
        return true;
    }

    /** Get a random element from the set. */
    int getRandom() {
        return vals[rand() % hash.size()];
    }

private:
    unordered_map<int, int> hash;
    vector<int> vals;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
     */
    bool remove(int val) {
        // write your code here
        if (!tbl.count(val))
            return false;

        store[tbl[val]] = store[n-1];
        tbl[store[n-1]] = tbl[val];
        tbl.erase(val);
        n--;
        return true;
    }

    /*
     * @return: Get a random element from the set
     */
    int getRandom() {
        if (n < 1)
            return 0;
        // write your code here
        return store[rand()%n];
    }
private:
    unordered_map<int, int> tbl;
    vector<int> store;
    int n;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * bool param = obj.insert(val);
 * bool param = obj.remove(val);
 * int param = obj.getRandom();
 */
```

## 612.K Closest Points

Given some`points`and a point`origin`in two dimensional space, find`k`points out of the some points which are nearest to`origin`.  
Return these points sorted by distance, if they are same with distance, sorted by x-axis, otherwise sorted by y-axis.

### Example

Given points =`[[4,6],[4,7],[4,4],[2,5],[1,1]]`, origin =`[0, 0]`, k =`3`  
return`[[1,1],[2,5],[4,4]]`

[https://www.lintcode.com/problem/k-closest-points/description](https://www.lintcode.com/problem/k-closest-points/description)

[https://leetcode.com/problems/k-closest-points-to-origin/](https://leetcode.com/problems/k-closest-points-to-origin/)

### 解题分析:

PRIORITY QUEUE

第一种办法是维护一个size =k的最大堆， 打擂台的方式过一遍 nlogk

第二种办法， heafy 一个最小堆 o\(n\) 然后不停的pop， klog\(n\) ,所以应该是o\(n+klog\(n\)\)

### 代码：

```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        auto dist = [&](int i) {
            return  points[i][0]*points[i][0] + points[i][1]*points[i][1];
        };
        auto cmp = [&](int i, int j) {
            return dist(i) < dist(j);
        };
        priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
        for (int i = 0; i < points.size(); i++) {
            pq.push(i);
            if (pq.size() > K) pq.pop();
        }
        vector<vector<int>> result;
        while(!pq.empty()) {
            result.push_back(points[pq.top()]);
            pq.pop();
        }
        return result;
    }
}

class Solution {
public:
    /**
     * @param points: a list of points
     * @param origin: a point
     * @param k: An integer
     * @return: the k closest points
     */
    vector<Point> kClosest(vector<Point> &points, Point &origin, int k) {
        // write your code here
        CmpOp cmp(origin);
        make_heap(points.begin(), points.end(), cmp);
        vector<Point> results;
        while(results.size() < k && !points.empty())
        {
            results.push_back(points.front());
            pop_heap(points.begin(), points.end(), cmp);
            points.pop_back();
        }
        return results;
    }
};
```

## 544. Top k Largest Numbers

Given an integer array, find the top\_k\_largest numbers in it.

### Example

Given`[3,10,1000,-99,4,100]`and_k_=`3`.  
Return`[1000, 100, 10]`.

[https://www.lintcode.com/problem/top-k-largest-numbers/description](https://www.lintcode.com/problem/top-k-largest-numbers/description)

### 解题分析:

和上面一样的技巧

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an integer array
     * @param k: An integer
     * @return: the top k largest numbers in array
     */
    vector<int> topk(vector<int> &nums, int k) {
        // write your code here
       make_heap(nums.begin(), nums.end());
       vector<int> results;
       while(!nums.empty() && results.size() < k )
       {
           results.push_back(nums.front());
           pop_heap(nums.begin(), nums.end());
           nums.pop_back();
       }
       return results;
    }
};

class Solution {
public:
    vector<int> topk(vector<int> &nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (auto& num : nums) {
            pq.push(num);
            if (pq.size() > k) pq.pop();
        }
        vector<int> res;
        while(!pq.empty()) {
            res.push_back(pq.top());
            pq.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

## 104. Merge K Sorted Lists

Merge\_k\_sorted linked lists and return it as one sorted list.

Analyze and describe its complexity.

### Example

Given lists:

```
[
  2-
>
4-
>
null,
  null,
  -1-
>
null
],
```

return`-1->2->4->null`.

[https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/)

### 解题分析:

两种思路，比较流行的思路是priority\_queue， 田忌赛马的故事， 放k个进去，优胜的出去，递补同组下一个，继续，复杂度为o\(nlogk\) k为单组平均长度

第二种思路是用merge sort的思路，merge明白两个，剩下的就是用递归的思路来解决两两merge

### 代码：

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto cmp = [](ListNode* l, ListNode* r) {
            return l->val > r->val;
        };
        priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> pq(cmp);
        for (auto each : lists) {
            if (each) pq.push(each);
        }
        ListNode dummy;
        ListNode* cur = &dummy;
        while(!pq.empty()) {
            auto node = pq.top();
            pq.pop();
            cur->next = node;
            cur = cur->next;
            if (node->next) pq.push(node->next);
        }
        return dummy.next;
    }
};


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
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        // write your code here
        return helper(lists, 0, lists.size()-1);
    }

    ListNode* helper(vector<ListNode*>& lists, int beg, int end)
    {
        if (beg > end)
            return nullptr;
        if (beg == end)
            return lists[beg];
        if (beg+1 == end)
            return mergeTwoList(lists[beg], lists[end]);
        int mid = (beg + end)/2;
        auto left = helper(lists, beg, mid);
        auto right = helper(lists, mid+1, end);
        return mergeTwoList(left, right);
    }

    ListNode* mergeTwoList(ListNode* left, ListNode* right)
    {
        ListNode dummy;
        ListNode* head = &dummy;
        while(left && right)
        {
            if (left->val < right->val)
            {
                head->next = left;
                left = left->next;
            }
            else
            {
                head->next = right;
                right = right->next;
            }
            head = head->next;
        }
        while(left)
        {
            head->next = left;
            head =head->next;
            left = left->next;
        }
        while(right)
        {
            head->next = right;
            head = head->next;
            right = right->next;
        }
        head->next = nullptr;
        return dummy.next;
    }
};
```

## 263. Ugly Number

Write a program to check whether a given number is an ugly number.

Ugly numbers are**positive numbers**whose prime factors only include`2, 3, 5`.

**Example 1:**

```
Input:
 6

Output:
 true

Explanation: 
6 = 2 × 3
```

**Example 2:**

```
Input:
 8

Output:
 true

Explanation: 
8 = 2 × 2 × 2
```

**Example 3:**

```
Input:
 14

Output:
 false 

Explanation: 
14
 is not ugly since it includes another prime factor 
7
.
```

```cpp
class Solution {
public:
    bool isUgly(int num) {
        if (num <= 0) return false;
        const static vector<int> primes{2, 3, 5};        
        for (auto prime : primes) {
            while( num % prime == 0 ) num /= prime;
        }
        return num == 1;
    }
};
```

## \*\*\*\*4. Ugly Number II

Ugly number is a number that only have factors`2`,`3`and`5`.

Design an algorithm to find the\_n\_th ugly number. The first 10 ugly numbers are`1, 2, 3, 4, 5, 6, 8, 9, 10, 12...`

### Example

If`n=9`, return`10`.

### Challenge

O\(_n\_log\_n_\) or O\(_n_\) time.

[https://leetcode.com/problems/ugly-number-ii/](https://leetcode.com/problems/ugly-number-ii/)

### 解题分析:

狗屎一样的题目

### 代码：

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        int i2 = 0, i3 = 0, i5 = 0;
        vector<int> tbl{1};
        while(tbl.size() < n) {
            auto vi2 = tbl[i2] * 2;
            auto vi3 = tbl[i3] * 3;
            auto vi5 = tbl[i5] * 5;
            auto v = min(vi2, min(vi3, vi5));
            tbl.push_back(v);
            if (vi2 == v) i2++;
            if (vi3 == v) i3++;
            if (vi5 == v) i5++;
        }
        return tbl[n-1];
    }
};
```

## 134. LRU Cache

Design and implement a data structure for Least Recently Used \(LRU\) cache. It should support the following operations: `get` and `set`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
`set(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

[https://www.lintcode.com/problem/lru-cache/description](https://www.lintcode.com/problem/lru-cache/description)

### 解题分析:

做烂的题目没啥好说的了。。。。

### 代码：

```cpp
#include <list>
class LRUCache {
public:

    struct Node{
      Node(int k, int v)
        :key(k),val(v)
        {}
      int key;
      int val;
    };

    /*
    * @param capacity: An integer
    */LRUCache(int capacity) {
        // do intialization if necessary
        capacity_ = capacity;
    }

    /*
     * @param key: An integer
     * @return: An integer
     */
    int get(int key) {
        // write your code here
        if (!map_.count(key))
            return -1;
        auto val = (*map_[key])->val;
        move2head(key);
        return val;
    }

    /*
     * @param key: An integer
     * @param value: An integer
     * @return: nothing
     */
    void set(int key, int value) {
        // write your code here
        if (map_.count(key))
        {
            (*map_[key])->val = value;
            move2head(key);
            return;
        }
        else
        {
            list_.push_front(new Node(key, value));
            map_[key] = list_.begin();
            if (list_.size() > capacity_)
            {
                auto node = list_.back();
                list_.pop_back();
                map_.erase(node->key);
                delete node;
            }
        }
    }
private:
    void move2head(int k)
    {
        if (!map_.count(k))
            return; 
        auto it = map_[k];
        auto node = *it;
        list_.erase(it);
        list_.push_front(node);
        map_[k] = list_.begin();
    }
private:
    int capacity_;
    unordered_map<int, list<Node*>::iterator> map_;
    list<Node*> list_;
};
```

## 24. LFU Cache

LFU \(Least Frequently Used\) is a famous cache eviction algorithm.

For a cache with capacity_k_, if the cache is full and need to evict a key in it, the key with the lease frequently used will be kicked out.

Implement`set`and`get`method for LFU cache.

### Example

Given`capacity=3`

[https://www.lintcode.com/problem/lfu-cache/description](https://www.lintcode.com/problem/lfu-cache/description)

[https://leetcode.com/problems/lfu-cache/](https://leetcode.com/problems/lfu-cache/)

### 解题分析:

1. get, set 是log\(n\)的解法，维护一个set来查找freq,一个unordered map来维护存在不存在以及Node的快速access. 
2. O\(1\)的办法, 为了实现O\(1\), node存 k, v, freq, iter, 用一个hash table来快速索引， 用一个freq vs list table 来维护最小频率。难点在于touch里的维护minFreq的方法，写着写着就糊涂了。

### 代码：

```cpp
class LFUCache {
public:
    LFUCache(int capacity) {
        size = capacity;
    }

    int get(int key) {
        if (!hash.count(key)) return -1;
        touch(key);
        return (*hash[key])->val;
    }

    void put(int key, int value) {
        if (size == 0) return;
        if (hash.count(key)) {
            touch(key);
            (*hash[key])->val = value;
            return;
        }
        if (hash.size() == size) {
            auto node = buckets[minFreq].back();
            buckets[minFreq].pop_back();
            if (buckets[minFreq].empty()) buckets.erase(minFreq);
            hash.erase(node->key);
            delete node;
        }
        minFreq = 0;
        buckets[minFreq].push_front(new Node(key, value));
        hash[key] = buckets[minFreq].begin();
    }

private:
    struct Node {
        int key;
        int val;
        int freq = 0;
        Node(int key, int val) : key(key), val(val) {}
    };

    typedef list<Node*> List;
    unordered_map<int, List> buckets;
    unordered_map<int, List::iterator> hash;
    int size = 0;
    int minFreq = 0;

private:
    void touch(int key) {
        if (!hash.count(key)) return;
        auto it = hash[key];
        auto node = *it;
        buckets[node->freq].erase(it);
        if (buckets[node->freq].empty()) {
            buckets.erase(node->freq);
            if (minFreq == node->freq) minFreq++;
        }
        node->freq++;
        buckets[node->freq].push_front(node);
        hash[key] = buckets[node->freq].begin();
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```




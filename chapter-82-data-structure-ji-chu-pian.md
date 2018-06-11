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

[https://www.lintcode.com/problem/moving-average-from-data-stream/description](https://www.lintcode.com/problem/moving-average-from-data-stream/description)

### 解题分析:

一个q就好了

### 代码：

```cpp
class MovingAverage {
public:
    /*
    * @param size: An integer
    */MovingAverage(int size)
        : size(size), total(0)
    {
        // do intialization if necessary
    }

    /*
     * @param val: An integer
     * @return:  
     */
    double next(int val) {
        // write your code here
        total += val;
        q.push(val);
        if (q.size() > size )
        {
            total -= q.front();
            q.pop();
        }
        return double(total)/q.size();
    }

private:
    int size; 
    long total;
    queue<int> q;
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param = obj.next(val);
 */
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
    /**
     * @param str: str: the given string
     * @return: char: the first unique character in a given string
     */
    char firstUniqChar(string &str) {
        // Write your code here
        unordered_map<char, int> tbl;
        for (auto c : str)
            tbl[c]++;
        for (auto c : str)
            if (tbl[c] == 1)
                return c;
        return 0;
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

[                
](https://www.lintcode.com/problem/insert-delete-getrandom-o1/description)[https://www.lintcode.com/problem/insert-delete-getrandom-o1/description](https://www.lintcode.com/problem/insert-delete-getrandom-o1/description)

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
    RandomizedSet() {
        // do intialization if necessary
        srand(time(NULL));
        n = 0;
    }

    /*
     * @param val: a value to the set
     * @return: true if the set did not already contain the specified element or false
     */
    bool insert(int val) {
        // write your code here
        if (tbl.count(val))
            return false;
        tbl[val] = n;
        if (n < store.size())
            store[n] = val;
        else
            store.push_back(val);
        n++;
        return true;
    }

    /*
     * @param val: a value from the set
     * @return: true if the set contained the specified element or false
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

### 解题分析:

PRIORITY QUEUE

第一种办法是维护一个size =k的最大堆， 打擂台的方式过一遍 nlogk

第二种办法， heafy 一个最小堆 o\(n\) 然后不停的pop， klog\(n\) ,所以应该是o\(n+klog\(n\)\)

### 代码：

```cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * }; * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */

class CmpOp{

public:
    CmpOp(const Point& origin)
    {
        origin_ = Point(origin.x, origin.y);
    }


    bool operator()(const Point& left, const Point& right)
    {
        return mincmp(left,right);
    }
private:
    bool mincmp(const Point& left, const Point& right)
    {
        int distl = dist(left);
        int dist2 = dist(right);
        if (distl < dist2)
            return true;
        else if (distl > dist2)
            return false;
        else if (left.x < right.x)
            return true;
        else if (left.x > right.x)
            return false;
        return left.y<right.y;
    }
    int dist(const Point& p)
    {
        return (p.x-origin_.x)*(p.x-origin_.x) + (p.y-origin_.y)*(p.y-origin_.y);
    }
private:
    Point origin_;    
};


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
        priority_queue<Point, vector<Point>,CmpOp> pq(cmp);
        for (auto& p : points)
        {
            if (pq.size() < k)
                pq.push(p);
            else
            {
                auto& top = pq.top();
                if (cmp(p, top))
                {
                    pq.pop();
                    pq.push(p);
                }
            }
        }
        vector<Point> results;
        while(!pq.empty())
        {
            results.push_back(pq.top());
            pq.pop();
        }
        reverse(results.begin(), results.end());
        return results;
    }
};


/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * }; * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */

class CmpOp{

public:
    CmpOp(const Point& origin)
    {
        origin_ = Point(origin.x, origin.y);
    }


    bool operator()(const Point& left, const Point& right)
    {
        return !mincmp(left,right);
    }
private:
    bool mincmp(const Point& left, const Point& right)
    {
        int distl = dist(left);
        int dist2 = dist(right);
        if (distl < dist2)
            return true;
        else if (distl > dist2)
            return false;
        else if (left.x < right.x)
            return true;
        else if (left.x > right.x)
            return false;
        return left.y<right.y;
    }
    int dist(const Point& p)
    {
        return (p.x-origin_.x)*(p.x-origin_.x) + (p.y-origin_.y)*(p.y-origin_.y);
    }
private:
    Point origin_;    
};


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

### 解题分析:

两种思路，比较流行的思路是priority\_queue， 田忌赛马的故事， 放k个进去，优胜的出去，递补同组下一个，继续，复杂度为o\(nlogk\) k为单组平均长度

第二种思路是用merge sort的思路，merge明白两个，剩下的就是用递归的思路来解决两两merge

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
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    struct Compare{
        bool operator()(ListNode* left, ListNode* right){
            return left->val > right->val;
        }
    };
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        // write your code here
        priority_queue<ListNode*, vector<ListNode*>,Compare> pq;
        for (auto node : lists)
        {
            if (node)   
                pq.push(node);
        }
        ListNode dummy; 
        ListNode* head = &dummy;
        while(!pq.empty())
        {
            auto node = pq.top();
            pq.pop();
            head->next = node;
            head = head->next;
            if (node->next)
                pq.push(node->next);
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

## \*\*\*\*4. Ugly Number II

Ugly number is a number that only have factors`2`,`3`and`5`.

Design an algorithm to find the\_n\_th ugly number. The first 10 ugly numbers are`1, 2, 3, 4, 5, 6, 8, 9, 10, 12...`

### Example

If`n=9`, return`10`.

### Challenge

O\(_n\_log\_n_\) or O\(_n_\) time.

[https://www.lintcode.com/problem/ugly-number-ii/description](https://www.lintcode.com/problem/ugly-number-ii/description)

### 解题分析:

1 方法1， 暴力法，把所有的ugly number都生成出来 2^i \*3^j\*5^k &lt;INT\_MAX, 这个组合常数个，sort一下取第n个

2。 用merge sort的方法，  2^i, 3^j, 5^k 三个list， 不停的merge，然后取到第n个。  其实写的时候可以简洁一点，  维护i2, i3,i5为

```
  的index， 每次比较num\[i\]\*2, num\[j\]\*3, num\[k\]\*5大小, 取最小的放上去并且Index ++ . 可以理解成 i, j, k记录上一个最小的位置， 直到他们ready为止（比如5， 一直指到0， 然后1，过了好久才用第二个元素再乘以5, 因为之前的都被2 3 cover掉了）。
```

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: the nth prime number as description.
     */
    int nthUglyNumber(int n) {
        // write your code here
        static vector<int> table;
        for (long i = 1; i <= INT_MAX; i*=2)
            for(long j = i; j <= INT_MAX; j*=3)
                for(long k = j; k <= INT_MAX; k*=5)
                {
                    table.push_back(k);
                }
        sort(table.begin(), table.end());
        return table[n-1];
    }
};
```

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: the nth prime number as description.
     */
    int nthUglyNumber(int n) {
        // write your code here
        vector<int> table{1};
        static int i2 = 0;
        static int i3 = 0;
        static int i5 = 0;
        while(table.size() < n)
        {
            int nexti2 = table[i2] * 2;
            int nexti3 = table[i3] * 3;
            int nexti5 = table[i5] * 5;
            int next = min(nexti2,min(nexti3, nexti5));
            if ( next == nexti2) i2++;
            if ( next == nexti3) i3++;
            if ( next == nexti5) i5++;
            table.push_back(next);
        }
        return table[n-1];
    }
};
```




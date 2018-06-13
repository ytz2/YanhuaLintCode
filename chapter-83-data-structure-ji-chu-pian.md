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

[https://www.lintcode.com/problem/first-unique-number-in-stream/description](https://www.lintcode.com/problem/first-unique-number-in-stream/description)

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

https://www.lintcode.com/problem/flatten-2d-vector/description

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




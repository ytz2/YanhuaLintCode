## 

## 两数之和（相向）

## 56 Two Sum

Given an array of integers, find two numbers such that they add up to a specific target number.

The function`twoSum`should return\_indices\_of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers \(both index1 and index2\) are zero-based.

##### Notice

You may assume that each input would have exactly one solution

Have you met this question in a real interview?

Yes

**Example**

numbers=`[2, 7, 11, 15]`, target=`9`

return`[0, 1]`

[http://www.lintcode.com/en/problem/remove-duplicate-numbers-in-array/](http://www.lintcode.com/en/problem/remove-duplicate-numbers-in-array/)

[https://leetcode.com/problems/two-sum/submissions/](https://leetcode.com/problems/two-sum/submissions/)

### 解题分析:

当年被问烂了的题， 直接写吧

### 代码：

```cpp
class Solution {
public:
    /**
     * @param numbers: An array of Integer
     * @param target: target = numbers[index1] + numbers[index2]
     * @return: [index1 + 1, index2 + 1] (index1 < index2)
     */
    vector<int> twoSum(vector<int> &numbers, int target) {
        // write your code here
        unordered_map<int,int> tbl;
        for(int i = 0; i < numbers.size(); i++)
        {
            if (tbl.find(target-numbers[i]) != tbl.end())
                return vector<int>{tbl[target-numbers[i]],i};
            else
                tbl[numbers[i]] = i;
        }
        return vector<int>(2,-1);
    }
};
```

### 复杂度分析:

o\(n\)

2020/07/26 纯粹为了写golang

```go
import "sort"
func twoSum(nums []int, target int) []int {

    type pair struct {
        v int
        i int  
    } 
    arr := make([]*pair, len(nums))

    for i, v := range nums {
        arr[i] = &pair{
            v:v,
            i:i,
        }
    }
    sort.SliceStable(arr, func(i, j int) bool {
        return arr[i].v < arr[j].v
    })

    res := make([]int, 2)
    b, e := 0, len(nums) -1
    for b < e {
        s := arr[b].v + arr[e].v
        if s == target {
            res[0] = arr[b].i
            res[1] = arr[e].i
            break
        } else if s < target {
            b++
        } else {
            e--
        }
    }
    return res
}

func twoSum(nums []int, target int) []int {
    h := make(map[int]int, len(nums))
    res := make([]int, 2)
    for i, v := range nums {
        if prev, ok := h[target - v]; ok {
            res[0] = prev
            res[1] = i
            break
        }
        h[v] = i
    }
    return res
}
```

## 数组去重问题

## 521 Remove Duplicate Numbers in Array

Given an array of integers, remove the duplicate numbers in it.

You should:  
1. Do it in place in the array.  
2. Move the unique numbers to the front of the array.  
3. Return the total number of the unique numbers.

**Example**

Given_nums_=`[1,3,1,4,4,2]`, you should:

1. Move duplicate integers to the tail of
   _nums_=`[1,3,4,2,?,?]`
2. Return the number of unique integers in
   _nums_&gt;`4`

Actually we don't care about what you place in`?`, we only care about the part which has no duplicate integers.

[http://www.lintcode.com/en/problem/remove-duplicate-numbers-in-array/](http://www.lintcode.com/en/problem/remove-duplicate-numbers-in-array/)

[https://leetcode.com/problems/remove-duplicates-from-sorted-array/](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

### 解题分析:

空间换时间的话就是弄一个hash set往里面丢， o\(n\)算是最优了， 要么就sort一下快慢指针来做

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: an array of integers
     * @return: the number of unique integers
     */
    int deduplication(vector<int> &nums) {
        // write your code here
        if (nums.empty())
            return 0;
        sort(nums.begin(), nums.end());
        int beg = 0,end = 0;
        while( end < nums.size())
        {
            if (nums[end] == nums[beg])
                end++;
            else
                nums[++beg] = nums[end++];
        }

        return beg+1;
    }
};
```

### 复杂度分析:

nlog\(n\)

2020/07/28

```go
func removeDuplicates(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    s, f := 0, 0
    for f < len(nums) {
        if nums[s] == nums[f] {
            f++;
        } else {
            nums[s+1] = nums[f];
            s++
            f++
        }
    }
    return s+1
}
```



## 滑动窗口问题

## 604 Window Sum

Given an array of n integer, and a moving window\(size k\), move the window at each iteration from the start of the array, find the

`sum`

of the element inside the window at each moving.

**Example**

For array`[1,2,7,8,5]`, moving window size k =`3`.  
1 + 2 + 7 = 10  
2 + 7 + 8 = 17  
7 + 8 + 5 = 20  
return`[10,17,20]`

[http://www.lintcode.com/en/problem/window-sum/](http://www.lintcode.com/en/problem/window-sum/)

### 解题分析:

直接写好了, sliding window average....数字信号处理的一个基本算法

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: a list of integers.
     * @param k: length of window.
     * @return: the sum of the element inside the window at each moving.
     */
    vector<int> winSum(vector<int> &nums, int k) {
        // write your code here
        vector<int> result;
        if (nums.empty())
            return result;
        int sum = 0;
        int beg = 0;
        for(;beg < k && beg <nums.size(); beg++)
            sum+=nums[beg];
        result.push_back(sum);
        while(beg < nums.size())
        {
            sum = sum + nums[beg]  - nums[beg-k];
            result.push_back(sum);
            beg++;
        }
        return result;
    }
};
```

### 复杂度分析:

o\(n\)

## 360 Sliding Window Median

Given an array of n integer, and a moving window\(size k\), move the window at each iteration from the start of the array, find the median of the element inside the window at each moving. \(If there are even numbers in the array, return the N/2-th number after sorting the element in the window. \)

Have you met this question in a real interview?

Yes

**Example**

For array`[1,2,7,8,5]`, moving window size k = 3. return`[2,7,7]`

At first the window is at the start of the array like this

`[ | 1,2,7 | ,8,5]`, return the median`2`;

then the window move one step forward.

`[1, | 2,7,8 | ,5]`, return the median`7`;

then the window move one step forward again.

`[1,2, | 7,8,5 | ]`, return the median`7`;

[http://www.lintcode.com/en/problem/sliding-window-median/](http://www.lintcode.com/en/problem/sliding-window-median/)

### 解题分析 1:

同上题，但是在处理window内的median的时候有问题。 一般处理数据流且要排序的情况下，需要用插入排序， 以下借用别人的一张图片来表示如下：

先找到应该被去掉的数字，去掉，然后再插入新数字，插入排序，单个操作的话是o\(k\) 所以总的复杂度为klog\(k\) + \(n-k\)\*k

![](/assets/import.png)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @param k: An integer
     * @return: The median of the element inside the window at each moving
     */
    vector<int> medianSlidingWindow(vector<int> &nums, int k) {
        // write your code here
        vector<int> result;
        if (nums.empty() || k<=0)
            return result;

        vector<int> window(nums.begin(), nums.begin()+k-1);
        sort(window.begin(), window.end());
        for (int i = k-1; i < nums.size(); i++)
        {
            window.push_back(nums[i]);
            auto it = prev(window.end(),1);
            auto insertionPoint = upper_bound(window.begin(), it, *it);
            rotate(insertionPoint, it, it+1);
            result.push_back(window[k%2==0?k/2-1:k/2]);
            auto deletePoint = find(window.begin(), window.end(), nums[i-k+1]);
            window.erase(deletePoint);
        }
        return result;
    }
};
```

### 复杂度分析:

klog\(k\) + \(n-k\)\*k

### 解题分析 2:

sliding window在siding排序的时候，用multiset大法好， multiset是binary search tree,且允许重复元素出现，这样既维持了顺序又维持了长度，且每次插入，删除的时间复杂度是log\(k\)。 比用两个q强。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @param k: An integer
     * @return: The median of the element inside the window at each moving
     */
    vector<int> medianSlidingWindow(vector<int> &nums, int k) {
        // write your code here
        vector<int> result;
        if (nums.empty() || k<=0)
            return result;
        multiset<int> window(nums.begin(), nums.begin()+k);
        auto mid = next(window.begin(), (k-1)/2);
        result.push_back(*mid);
        for (int i = k; i < nums.size(); i++)
        {
            window.insert(nums[i]);
            // 枚举四种情况  最后可得，插入小的时候左移 而删除小于等于的时候右移
            if (nums[i] <*mid)
                --mid;
            if (nums[i-k] <= *mid)
                ++mid;
            window.erase(window.lower_bound(nums[i-k]));
            result.push_back(*mid);
        }
        return result;
    }
};
```

### 复杂度分析:

klog\(k\) + \(n-k\)\*log\(k\)

#### 笔记：

upper_bound, lower\_bound, rotate, next, prev   这些std lib要熟悉_

## 362 Sliding Window Maximum

Given an array of n integer with duplicate number, and a moving window\(size k\), move the window at each iteration from the start of the array, find the maximum number inside the window at each moving.

Have you met this question in a real interview?

Yes

**Example**

For array`[1, 2, 7, 7, 8]`, moving window size`k = 3`. return`[7, 7, 8]`

At first the window is at the start of the array like this

`[|1, 2, 7| ,7, 8]`, return the maximum`7`;

then the window move one step forward.

`[1, |2, 7 ,7|, 8]`, return the maximum`7`;

then the window move one step forward again.

`[1, 2, |7, 7, 8|]`, return the maximum`8`;

[http://www.lintcode.com/en/problem/sliding-window-maximum/](http://www.lintcode.com/en/problem/sliding-window-maximum/)

### 解题分析:

滑动窗口问题，还是multiset大法好

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers
     * @param k: An integer
     * @return: The maximum number inside the window at each moving
     */
    vector<int> maxSlidingWindow(vector<int> nums, int k) {
        // write your code here
        vector<int> res;
        if (nums.empty() || k <= 0)
            return res;
        multiset<int> window(nums.begin(), nums.begin()+k);
        auto it_max = window.rbegin();
        res.push_back(*it_max);
        for (int i = k; i < nums.size(); i++)
        {
            window.insert(nums[i]);
            window.erase(window.lower_bound(nums[i-k]));
            res.push_back(*window.rbegin());
        }
        return res;
    }
};
```

### 复杂度分析:

klog\(k\) + \(n-k\)\*log\(k\)

#### 笔记：

multiset最大数可直接从window.rbegin\(\)得到， 不需要移动iterator

## 610 Two Sum - Difference equals to target

Given an array of integers, find two numbers that their`difference`equals to a target value.  
where index1 must be less than index2. Please note that your returned answers \(both index1 and index2\) are NOT zero-based.

##### Notice

It's guaranteed there is only one available solution

Have you met this question in a real interview?

Yes

**Example**

Given nums =`[2, 7, 15, 24]`, target =`5`  
return`[1, 2]`\(7 - 2 = 5\)

[http://www.lintcode.com/en/problem/two-sum-difference-equals-to-target/](http://www.lintcode.com/en/problem/two-sum-difference-equals-to-target/)

### 解题分析:

用hashtable滑动的去做还是o\(n\), 这个题用双指针做一下：）

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an array of Integer
     * @param target: an integer
     * @return: [index1 + 1, index2 + 1] (index1 < index2)
     */
    vector<int> twoSum7(vector<int> &nums, int target) {
        // write your code here.
        if (nums.size() < 2)
            return vector<int>(2,-1);
        vector<pair<int, int>> sorted;
        for(int i = 0; i < nums.size(); ++i)
            sorted.push_back(make_pair(nums[i],i+1));
        sort(sorted.begin(), sorted.end());
        int beg = 0, end = 1;
        while(end < nums.size())
        {
            int sum = sorted[beg].first + abs(target);
            if (sum == sorted[end].first)
            { 
                int ind1 = sorted[beg].second, ind2 = sorted[end].second;
                if (ind1 < ind2)
                    return vector<int>{ind1, ind2};
                return vector<int>{ind2, ind1};
            }
            else if (sum > sorted[end].first)
                end++;
            else
            {
                beg++;
                if (beg == end)
                    end++;
            }
        }
    }
};
```

### 复杂度分析:

o\(nlog\(n\)\)

#### 笔记

这里target&lt;0的时候稍微纠结了一下， 因为sorted， 所以我们只关心abs\(target\)， 因为可以a-b也可以b-a 。 另外一个纠结了以下的地方是++beg == end的时候，其实end应该向前移动，举个反例就是如果输入时0，we are fucked.

## 链表中点问题

## 228 . Middle of Linked List

Find the middle node of a linked list.

Have you met this question in a real interview?

Yes

**Example**

Given`1->2->3`, return the node with value 2.

Given`1->2`, return the node with value 1.

[http://www.lintcode.com/en/problem/middle-of-linked-list/](http://www.lintcode.com/en/problem/middle-of-linked-list/)

### 解题分析:

基本功题目， 可惜我基本功太差，记得快指针初始位置要在慢指针后面，画画图就知道了。

条件为while\(fast && fast-&gt;next\)

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
    /*
     * @param head: the head of linked list.
     * @return: a middle node of the linked list
     */
    ListNode * middleNode(ListNode * head) {
        // write your code here
        if (!head)
            return head;
        ListNode *first = head, *second = head->next;
        while(second && second->next)
        {
            first=first->next;
            second = second->next->next;
        }
        return first;
    }
};复杂度分析:
```

#### 复杂度

o\(n\).

## 81. Data Stream Median

Numbers keep coming, return the median of numbers at every time a new number added.

Have you met this question in a real interview?

Yes

**Clarification**

What's the definition of Median?

* Median is the number that in the middle of a sorted array. If there are n numbers in a sorted array A, the median is`A[(n - 1) / 2]`. For example, if`A=[1,2,3]`, median is`2`. If`A=[1,19]`, median is`1`.

**Example**

For numbers coming list:`[1, 2, 3, 4, 5]`, return`[1, 1, 2, 2, 3]`.

For numbers coming list:`[4, 5, 1, 3, 2, 6, 0]`, return`[4, 4, 4, 3, 3, 3, 3]`.

For numbers coming list:`[2, 20, 100]`, return`[2, 2, 20]`.

### 解题分析:

第一招用multiset大法，维护一个左边界的median , 所以考虑这个问题的时候：我们已经在median的最左面了，现在我们是偶数的时候， 如果比他大，则向右移动这样可以指在中间，如果我们是奇数，那么比他小的时候左移，比他大不需要移动，因为我们要指在左边界。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: the median of numbers
     */
    vector<int> medianII(vector<int> &nums) {
        // write your code here
        vector<int> res;
        if (nums.empty())
            return res;
        multiset<int> container;
        container.insert(nums[0]);
        auto it = container.find(nums[0]);
        res.push_back(*it);
        for (int i = 1; i< nums.size(); i++)
        {
            auto n = container.size();
            container.insert(nums[i]);
            // even number
            if (n%2 ==0)
            {
                // we always sit on the left median
                if (nums[i] >= *it)
                    it++;
            }
            else // odd
            {
                // we are in the middle
                if (nums[i] < *it)
                    it--;
            }
            res.push_back(*it);
        }
        return res;
    }
};
```

#### 复杂度

插入是log\(n\)

### 解题分析 2:

第二个办法是priority\_queue, 左边一个最大pq,右面一个最小pq 只要保证leftq.size\(\) - rightq.size\(\) &lt;=1就可以始终从leftq.top\(\)找到median,其实这是用两个q来模拟multiset的中间节点

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: the median of numbers
     */
    vector<int> medianII(vector<int> &nums) {
        // write your code here
        priority_queue<int, vector<int>> leftq;
        priority_queue<int, vector<int>, greater<int>> rightq;
        vector<int> res;
        if (nums.size() <= 1)
            return nums;
        leftq.push(nums[0]);
        res.push_back(nums[0]);
        for(int i=1; i<nums.size(); ++i)
        {
            int num = nums[i];
            if (num < leftq.top())
                leftq.push(num);
            else
                rightq.push(num);
            int left_n = leftq.size();
            int right_n = rightq.size();
            if (left_n + 1 == right_n)
            {
                auto tmp = rightq.top();
                rightq.pop();
                leftq.push(tmp);
            }
            if (left_n -2 == right_n)
            {
                auto tmp = leftq.top();
                leftq.pop();
                rightq.push(tmp);
            }
            res.push_back(leftq.top());
        }
        return res;
    }
};
```

#### 复杂度

插入是log\(n\)

## 545. Top k Largest Numbers II

Implement a data structure, provide two interfaces:

1. `add(number)`
   . Add a new number in the data structure.
2. `topk()`
   . Return the top
   _k_
   largest numbers in this data structure.
   _k_
   is given when we create the data structure.

Have you met this question in a real interview?

Yes

**Example**

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

### 解题分析:

写复杂了，刚开始觉得开一个最小q， 然后以为另外放一个list， 和LRU cache一样。。。其实太天真了， get的时候还是要sort, 第一版代码

### 代码：

```cpp
#include <list>
class Solution {
public:
    /*
    * @param k: An integer
    */Solution(int k) {
        // do intialization if necessary
        n_ = k;
    }

    /*
     * @param num: Number to be added
     * @return: nothing
     */
    void add(int num) {
        // write your code here
        container_.push_back(num);
        auto it = prev(container_.end(), 1);
        q_.push(make_pair(num, it));
        if (q_.size() > n_)
        {
            auto t = q_.top();
            q_.pop();
            auto it = t.second;
            container_.erase(it);
        }
    }

    /*
     * @return: Top k element
     */
    vector<int> topk() {
        // write your code here
        vector<int> res(container_.begin(), container_.end());
        std::sort(res.begin(), res.end(), std::greater<int>());
        return res;
    }
private:
    int n_;
    list<int> container_;
    typedef pair<int, std::list<int>::iterator> PairT;
    struct myCompare
    {
        bool operator() (const PairT& left, const PairT& right)
        {
            return left.first > right.first;
        }
    };
    priority_queue< PairT, vector<PairT>, myCompare > q_;
};
```

#### 

```
#include <list>
class Solution {
public:
    /*
    * @param k: An integer
    */Solution(int k) {
        // do intialization if necessary
        n_ = k;
    }

    /*
     * @param num: Number to be added
     * @return: nothing
     */
    void add(int num) {
        // write your code here
        if (q_.size() < n_)
            q_.push(num);
        else
        {
            if (q_.top() < num )
            {
                q_.pop();
                q_.push(num);
            }
        }
    }

    /*
     * @return: Top k element
     */
    vector<int> topk() {
        // write your code here
        vector<int> res;
        while(!q_.empty())
        {
            res.push_back(q_.top());
            q_.pop();
        }

        for(const auto& each:res)
            q_.push(each);
        sort(res.begin(), res.end(), greater<int>());
        return res;
    }
private:
    int n_;
    priority_queue<int, vector<int>, greater<int>> q_;
};
```

#### 

#### 复杂度

插入是nlog\(k\) , klog\(k\)

## 550. Top K Frequent Words II

Find top\_k\_frequent words in realtime data stream.

Implement three methods for\_Topk\_Class:

1. `TopK(k)`
   . The constructor.
2. `add(word)`
   . Add a new word.
3. `topk()`
   . Get the current top
   _k_
   frequent words.

##### Notice

If two words have the same frequency, rank them by alphabet.

Have you met this question in a real interview?

Yes

**Example**

```
TopK(2)
add("lint")
add("code")
add("code")
topk()

>
>
 ["code", "lint"]
```

### 解题分析:

这道题是看了答案写的，考察的是算法+数据结构+语言feature

其中注意set的alloc定义，是先比的key对应的value,如果value相等再做string的比较。 之所以要处理相等的情况是因为search的时候如果相等就会被返回，如果只定义key-value search,返回的不是我们想要的。

维护一个小于等于k的set, 和一个unordered\_map, 用map来计数， 用set来控制最频繁的k个数字，因为set是基于frequency来比较的，所以每次有新来的时候先不要更新map，而是移掉set中的记录，更新map重新插入以不影响alloc的算法。 如果大小超过k就把最不频繁的那个去掉

### 代码：

```cpp
unordered_map<string, int> counter_;
class TopK {
public:
    /*
    * @param k: An integer
    */TopK(int k) {
        // do intialization if necessary
        k_ = k;
    }

    /*
     * @param word: A string
     * @return: nothing
     */
    void add(string &word) {
        // write your code here
        if (counter_.find(word) == counter_.end())
            counter_[word] = 1;
        else
        {
            auto it = recorder_.find(word);
            if (it != recorder_.end())
            {
                recorder_.erase(it);
            }
            counter_[word]++;
        }

        recorder_.insert(word);

        if (recorder_.size() > k_)
            recorder_.erase(prev(recorder_.end(),1));
    }

    /*
     * @return: the current top k frequent words.
     */
    vector<string> topk() {
        // write your code here
        vector<string> res(recorder_.begin(), recorder_.end());
        return res;
    }


    int k_;

    struct MyCompare
    {
        bool operator() (const string& left, const string& right)
        {
            int leftn = counter_[left];
            int rightn = counter_[right];
            if (leftn != rightn)
                return leftn > rightn;
            return left < right;
        }
    };
    set<string, MyCompare> recorder_;
};
```




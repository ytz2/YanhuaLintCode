#### 894. Pancake Sorting

Given an an`unsorted array`, sort the given array. You are allowed to do only following operation on array.

```
flip(arr, i): Reverse array from 0 to i
```

Unlike a traditional sorting algorithm, which attempts to sort with the fewest comparisons possible, the goal is to sort the sequence in as few reversals as possible.

##### Notice

You only call`flip`function.  
Don't allow to use any sort function or other sort methods.

Java：you can call`FlipTool.flip(arr, i)`  
C++： you can call`FlipTool::flip(arr, i)`  
Python2&3 you can call`FlipTool.flip(arr, i)`

Have you met this question in a real interview?

Yes

**Example**

Given array =`[6, 7, 10, 11, 12, 20, 23]`  
Use`flip(arr, i)`function to sort the array.

[http://www.lintcode.com/en/problem/pancake-sorting/](http://www.lintcode.com/en/problem/pancake-sorting/)

[https://leetcode.com/problems/pancake-sorting/submissions/](https://leetcode.com/problems/pancake-sorting/submissions/)

### 解题分析:

Youtube上看一个视频， 先找最大的，翻到顶上去，然后再全翻，这样最大的就在下面了，然后再找从0到倒数第二的最大，重复上面的操作

代码：

```
/**
 * class FlipTool {
 * public:
 *   static void flip(vector<int>& arr, int i);
 * };
 */
class Solution {
public:
    /**
     * @param array: an integer array
     * @return: nothing
     */
    void pancakeSort(vector<int> &array) {
        // Write your code here
        if (array.size() <= 1)
            return;
        int last = array.size();
        while(last != 0)
        {
            int m = findMax(array, last);
            FlipTool::flip(array, m);
            FlipTool::flip(array, --last);
        }
    }

    int findMax(vector<int>& array, int end)
    {
        int m = 0;
        for(int i = 0; i < end; i++)
        {
            if (array[i] > array[m])
                m = i;
        }
        return m;
    }
};
```

### 复杂度分析:

o\(n^2\)

2020/08/05

```go
func pancakeSort(A []int) []int {
    flip := func(A []int, k int) {
        for b, e := 0, k -1; b < e; b, e = b +1, e-1 {
            A[b], A[e] = A[e], A[b]
        }
    }

    fmax := func(A []int, k int) int {
        index := 0
        for i:= 0; i < k; i++ {
            if A[i] > A[index] {
                index = i
            }
        } 
        return index + 1
    }

    res := make([]int, 0)
    k := len(A)

    for k > 1 {
        m := fmax(A, k)
        flip(A, m)
        res = append(res, m)
        flip(A, k)
        res = append(res, k)
        k--
    }
    return res
}
```

写Golang还挺好玩的

#### 625. Partition Array II

Partition an unsorted integer array into three parts:

1. The front part 
   &lt;
   _low_
2. The middle part 
   &gt;
   =
   _low_
   &
   &lt;
   =
   _high_
3. The tail part 
   &gt;
   _high_

Return any of the possible solutions.

##### Notice

low &lt;= high in all testcases.

Have you met this question in a real interview?

Yes

**Example**

Given`[4,3,4,1,2,3,1,2]`, and low =`2`and high =`3`.

Change to`[1,1,2,3,2,3,4,4]`.

\(\[1,1,2,2,3,3,4,4\] is also a correct answer, but \[1,2,1,2,3,3,4,4\] is not\)

### 解题分析:

代码：

```
class Solution {
public:
    /*
     * @param nums: an integer array
     * @param low: An integer
     * @param high: An integer
     * @return: 
     */
    void partition2(vector<int> &nums, int low, int high) {
        // write your code here
        if (nums.size() <=1)
            return;
        int l = 0, r = nums.size()-1, i =0;

        while(i < r)
        {
            if (nums[i] < low)
                swap(nums[i++], nums[l++]);
            else if (nums[i] > high)
                swap(nums[i], nums[r--]);
            else
                i++;
        }
    }
};
```

### 复杂度分析:

o\(n\)

2020/08/05

三色排序的一个变形, 就不写一遍了

#### 102. Linked List Cycle

Given a linked list, determine if it has a cycle in it.

**Example**

Given -21-&gt;10-&gt;4-&gt;5, tail connects to node index 1, return true

[https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)

### 解题分析:

快慢指针

代码：

```
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
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
    bool hasCycle(ListNode * head) {
        // write your code here
        if (!head)
            return false;
        ListNode* fast = head->next;
        while(fast && fast->next)
        {
            if (fast == head)
                return true;
            head = head->next;
            fast = fast->next->next;
        }
        return false;
```

### 复杂度分析:

o\(n\)



2020/08/07

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func hasCycle(head *ListNode) bool {
    if head == nil {
        return false
    }
    s, f := head, head.Next
    for f != nil && f.Next != nil {
        s , f = s.Next, f.Next.Next
        if s == f {
            return true
        }
    }
    return false
}
```



#### 58. 4Sum

Given an array_S\_of\_n\_integers, are there elements\_a_,_b_,_c_, and_d\_in\_S\_such that\_a + b + c + d = target_?

Find all unique quadruplets in the array which gives the sum of target.

##### Notice

Elements in a quadruplet \(_a,b,c,d_\) must be in non-descending order. \(ie,_a ≤ b ≤ c ≤ d_\)  
The solution set must not contain duplicate quadruplets.

Have you met this question in a real interview?

Yes



[https://leetcode.com/problems/4sum/](https://leetcode.com/problems/4sum/)

**Example**

Given array S =`{1 0 -1 0 -2 2}`, and target =`0`. A solution set is:

```
(-1, 0, 0, 1)
(-2, -1, 1, 2)
(-2, 0, 0, 2)
```

### 解题分析:

无聊，堆代码

快慢指针

### 代码：

```
class Solution {
public:
    /**
     * @param numbers: Give an array
     * @param target: An integer
     * @return: Find all unique quadruplets in the array which gives the sum of zero
     */
    vector<vector<int>> fourSum(vector<int> &numbers, int target) {
        // write your code here
        vector<vector<int>> res;
        int n = numbers.size();
        if (n < 4)
            return res;
        sort(numbers.begin(), numbers.end());
        for (int i = 0; i < n - 3; i++)
        {
            // skip duplicates
            if (i!=0 && numbers[i-1] == numbers[i])
                continue;
            for (int j = i + 1; j < n -2; j++)
            {
                // skip duplicates
                if (j != i + 1 && numbers[j - 1] == numbers[j])
                    continue;
                int beg = j + 1, end = n - 1;
                int targetVal = target - numbers[i] - numbers[j];
                while(beg < end)
                {
                    if (beg != j + 1 && numbers[beg - 1] == numbers[beg])
                    {
                        beg++;
                        continue;
                    }
                    if (end != n - 1 && numbers[end+1] == numbers[end])
                    {
                        end--;
                        continue;
                    }
                    int val = numbers[beg] + numbers[end];
                    if (val > targetVal)
                        end--;
                    else if (val < targetVal)
                        beg++;
                    else
                    {
                        res.push_back(vector<int>{numbers[i], numbers[j], numbers[beg], numbers[end]});
                        beg++;
                        end--;
                    }
                }
            }
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n^3\)

2020/08/07

```go
import (
    "sort"
)
func fourSum(nums []int, target int) [][]int {
    res := make([][]int, 0)
    n := len(nums)
    
    // illegal input
    if n < 4 {
        return res
    }
    sort.Ints(nums)
    for b := 0; b < n -3; b++ {
        if b > 0 && nums[b] == nums[b-1] {
            continue
        }
        for s := b+1; s < n - 2; s++ {
            if s > b + 1 && nums[s] == nums[s-1] {
                continue
            }
            x, y := s + 1, n -1 
            for x < y {
                if x < y && x > s + 1 && nums[x] == nums[x-1] {
                    x++
                    continue
                }
                if x < y && y < n - 1 && nums[y] == nums[y+1] {
                    y--
                    continue
                }
                if x == y {
                    break
                }
                v := nums[b] + nums[s] + nums[x] + nums[y]
                if v > target {
                    y--
                } else if v < target {
                    x++
                } else {
                    res = append(res, []int{nums[b], nums[s], nums[x], nums[y]})
                    x++
                }
            }
        }
    }
    return res
}
```



#### 103. Linked List Cycle II

Given a linked list, return the node where the cycle begins.

If there is no cycle, return`null`.

Have you met this question in a real interview?

Yes

**Example**

Given`-21->10->4->5`, tail connects to node index 1，return`10`

### 解题分析:

快慢指针，先让快慢指针相遇，然后从头和相遇点同时走，遇到的位置就是了。有数学证明，懒得记，画图总结一下也能知道

### 代码：

```
Given a linked list, return the node where the cycle begins.

If there is no cycle, return null.

Have you met this question in a real interview? Yes
Example
Given -21->10->4->5, tail connects to node index 1，return 10
```

### 复杂度分析:

NA

2020/08/07

那个方法太fancy了。。。我着实不会。。。。

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func detectCycle(head *ListNode) *ListNode {
    set := make(map[*ListNode]bool)
    for head != nil {
        if _, ok := set[head]; ok {
            return head
        }
        set[head] = true
        head = head.Next
    }
    return nil
}
```




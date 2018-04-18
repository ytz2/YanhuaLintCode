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

### 解题分析:

Youtube上看一个视频， 先找最大的，翻到顶上去，然后再全翻，这样最大的就在下面了，然后再找从0到倒数第二的最大，重复上面的操作

代码：

```cpp
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

这道题包括那个sort colors都是可以用count sort或者二次partion来做， 但是这个有个challenge,只让用一次遍历。 idea是左右两个指针

AAAAAAAAAABBBBBBBBBCCCCCCCC

                       L                    I R

维护三根指针， i为动态指针，L维护比L小， R比H大， 那么比L小的时候swap , i, L, 两个指针一起走， 比H大， swap i , r, r左移。 i不能动，因为我们只知道他比H小但是不知道他比L大，所以让他停在那里等下次循环判断。 如果在中间，只需要移动I，从图上看，I=R是终止条件

代码：

```cpp
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






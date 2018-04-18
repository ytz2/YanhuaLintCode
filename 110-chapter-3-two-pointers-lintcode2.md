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

http://www.lintcode.com/en/problem/pancake-sorting/

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


## 464 Sort Integers II

Given an integer array, sort it in ascending order. Use quick sort, merge sort, heap sort or any O\(nlogn\) algorithm.

Have you met this question in a real interview?

Yes

**Example**

Given`[3, 2, 1, 4, 5]`, return`[1, 2, 3, 4, 5]`.

[http://www.lintcode.com/en/problem/sort-integers-ii/](http://www.lintcode.com/en/problem/sort-integers-ii/)

### 解题分析:

两个经典的排序方法：

merge sort: 先局部有序， mergeSort\(A, beg, mid\); mergeSort\(A, mid+1, end\); merge\(A,  beg, end\) ， 为了避免空间多次开辟，提前开辟一个等空间的buffer放进去，这样省点时间

quick sort: 先整体有序， 再局部有序， partition\(A\), sort\(A left\), sort \(A right\)

quick sort 模板

while\(left &lt;= right\)

{

while\(left &lt;= right && A\[left\] &lt; p\) left++;

while \(left&lt;= right && A\[right\] &gt; p\) right--;;

if \(left &lt;= right\) swap\(A\[left++\], A\[right--\]\);

}

为什么比Index要加等号？ ： 懒得算右边界，直接让他错过去完事

为什么比value不加等号？ ： 111112 如果加了等号 就会一次只去掉2， 这样不加等号，就可以把1弄到右面去， 尽量均分

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: an integer array
     * @return: nothing
     */
    void sortIntegers2(vector<int> &A) {
        // write your code here
        //mergeSort(A);
        quickSort(A);
    }

    void mergeSort(vector<int>& A)
    {
        vector<int> buffer(A.size(),0);
        mergeSort(A, 0, A.size()-1, buffer);    
    }

    void mergeSort(vector<int>& A, int beg, int end, vector<int>& buffer)
    {
        if (beg >= end)
            return;
        int mid = beg + (end - beg)/2;
        mergeSort(A, beg, mid, buffer);
        mergeSort(A, mid+1, end, buffer);
        merge(A, beg, end, buffer);
    }

    void merge(vector<int>& A, int beg, int end, vector<int>& buffer)
    {
        int mid = beg + (end - beg) /2;
        int left = beg;
        int right = mid+1;
        int cur = beg;
        while(left <= mid && right <= end)
            buffer[cur++] = A[left] < A[right]? A[left++] : A[right++];
        while(left <= mid)
            buffer[cur++] = A[left++];
        while(right <= end)
            buffer[cur++] = A[right++];
        for(int i = beg; i<=end; i++)
            A[i] = buffer[i];
    }

    void quickSort(vector<int>& A)
    {
        quickSort(A, 0, A.size()-1);
    }

    void quickSort(vector<int>& A, int beg, int end)
    {
        if (beg >= end)
            return;
        int pivot = A[beg + (end-beg)/2];
        int left = beg, right = end;
        while(left <= right)
        {
            while (left <= right && A[left]<pivot)
                left++;
            while (left <= right && A[right] > pivot)
                right--;
            if (left <= right)
                swap(A[left++], A[right--]);
        }
        quickSort(A, beg, right);
        quickSort(A, left, end);
    }

};
```

### 复杂度分析:

nlog\(n\)



## 5 Kth Largest Elemen

Find K-th largest element in an array.

##### Notice

You can swap elements in the array

Have you met this question in a real interview?

Yes

**Example**

In array`[9,3,2,4,8]`, the 3rd largest element is`4`.

In array`[1,2,3,4,5]`, the 1st largest element is`5`, 2nd largest element is`4`, 3rd largest element is`3`and etc.

[**Challenge**](http://www.lintcode.com/en/problem/kth-largest-element/#challenge)

O\(n\) time, O\(1\) extra memory.

http://www.lintcode.com/en/problem/kth-largest-element/

### 解题分析:

这个是快排的一个变种题目， 用的思想是，每次partition ， 从大到小， 如果 第k个数字在左边从左边搜，如果在右面从右边搜，否则就是在中间位置。



### 代码：

```cpp
class Solution {
public:
    /*
     * @param n: An integer
     * @param nums: An array
     * @return: the Kth largest element
     */
    int kthLargestElement(int n, vector<int> &nums) {
        // write your code here
        if (nums.size() < n || nums.empty() || n<=0)
            return -1;
        return quickSelect(n, nums, 0, nums.size()-1);
    }
    
    int quickSelect(int k,  vector<int>& A, int left, int right)
    {
        int pivot = A[left + (right-left)/2];
        int i = left, j = right;
        while(i<=j)
        {
            while(i<=j && A[i] > pivot) i++;
            while(i<=j && A[j] < pivot) j--;
            if (i<=j) swap(A[i++],A[j--]);
        }
        if (left + k-1 <= j)
            return quickSelect(k, A, left, j);
        if (left + k-1 >= i)
            return quickSelect(k-i+left, A, i,right);
        return A[j+1];
    }
};
```

### 复杂度分析:

o\(n\)




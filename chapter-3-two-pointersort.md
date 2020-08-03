## 464 Sort Integers II

Given an integer array, sort it in ascending order. Use quick sort, merge sort, heap sort or any O\(nlogn\) algorithm.

Have you met this question in a real interview?

Yes

**Example**

Given`[3, 2, 1, 4, 5]`, return`[1, 2, 3, 4, 5]`.

[http://www.lintcode.com/en/problem/sort-integers-ii/](http://www.lintcode.com/en/problem/sort-integers-ii/)

[https://leetcode.com/problems/sort-an-array/](https://leetcode.com/problems/sort-an-array/)

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

2020/08/02

```go
func sortArray(nums []int) []int {
    //MergeSort(nums)
    QuickSort(nums)
    return nums
}

func QuickSort(nums []int) {
    quickSort(nums, 0, len(nums)-1)
}

func quickSort(nums []int, beg, end int) {
    // partion
    if beg >= end {
        return;
    }
    l, r := beg, end
    p := nums[beg + ( end - beg ) / 2]

    for l <= r {
        for l <= r && nums[l] < p {
            l++
        }

        for l <= r && nums[r] > p {
            r--
        }
        if l <= r {
            nums[l], nums[r] = nums[r], nums[l]
            l++
            r--
        }
    }
    quickSort(nums, beg, r)
    quickSort(nums, l, end)
}


func MergeSort(nums []int) {
    buffer := make([]int, len(nums))
    mergeSort(nums, buffer, 0, len(nums)-1)
}

func mergeSort(nums []int, buffer []int, beg int, end int) {
    if beg >= end {
        return;
    }
    mid := beg + ( end - beg) / 2;
    mergeSort(nums, buffer, beg, mid)
    mergeSort(nums, buffer, mid+1, end)
    s, l, r := beg, beg, mid + 1
    for l <= mid && r <= end {
        if nums[l] < nums[r]{
            buffer[s] = nums[l]
            l++
        } else {
            buffer[s] = nums[r]
            r++
        }
        s++
    }   
    for l <= mid {
        buffer[s] = nums[l]
        l++
        s++
    }
    for r <= end {
        buffer[s] = nums[r]
        r++
        s++
    }

    for beg <= end {
        nums[beg] = buffer[beg]
        beg++
    }

}
```

## \*\*\*\*\*\*\*5 Kth Largest Elemen

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

[http://www.lintcode.com/en/problem/kth-largest-element/](http://www.lintcode.com/en/problem/kth-largest-element/)

[https://leetcode.com/problems/kth-largest-element-in-an-array/](https://leetcode.com/problems/kth-largest-element-in-an-array/)

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

2020/08/02

今天revisit这道题目，自己做错了但又觉得很有趣， 我拿了快排的模板然后去看前面数组长度是不是k-1, 结果就出错了， 纠结问题的地方在于应不应该用l,r 来计算 长度， 答案是不行的。 当初拿到的这个模板是快排模板，只是能保证你及时退出而不能保证index 是对的， 

\[3,2,1,5,6,4\]

2

这个test case ,  用1做Pivot

3, 2, 5,6,4 \| 1

然后用5

6, 5\| 3, 2, 4

那么要早6,5 找k=2 , 快排把两个指针都移到6然后变换指针为了符合模板退出， 这个时候下面的解必然是错的， 因为 右指针为-1了， 拿来算长度怎么也不会对的。

所以才有了以前的解，把beg, k 拿来和 b, e 比， 而不是算长度来比， 这是模板的局限性导致的bug， take it



错误的答案：

```go
func findKthLargest(nums []int, k int) int {
    return findKthLargestImpl(nums, k, 0, len(nums) -1)
}

func findKthLargestImpl(nums []int, k, b, e int) int {
    if k == 1 {
        return nums[b]
    }
    p := nums[b + ( e - b) / 2]
    l, r := b, e
    for l <= r {
        for l <= r && nums[l] > p {
            l++
        }
        
        for l <= r && nums[r] < p {
            r--
        }
        
        if l <= r {
            nums[l], nums[r] = nums[r], nums[l]
            l++
            r--
        }
    }
    ll := r - b + 1
    // now look at r
    if ll == k-1 {
        return nums[r+1]
    } else if ll > k - 1{
        return findKthLargestImpl(nums, k, b, r)
    }
    return findKthLargestImpl(nums, k - ll, r+1, e)
}
```

正确解



```go
func findKthLargest(nums []int, k int) int {
    return findKthLargestImpl(nums, k, 0, len(nums) -1)
}

func findKthLargestImpl(nums []int, k, b, e int) int {
    p := nums[b + ( e - b) / 2]
    l, r := b, e
    for l <= r {
        for l <= r && nums[l] > p {
            l++
        }
        
        for l <= r && nums[r] < p {
            r--
        }
        
        if l <= r {
            nums[l], nums[r] = nums[r], nums[l]
            l++
            r--
        }
    }
    if b + k - 1 <= r {
        return findKthLargestImpl(nums, k, b, r)
    } else if b + k - 1 >= l {
        return findKthLargestImpl(nums, k - ( l - b ), l, e)
    }
    return nums[r+1]
}
```



## 148 Sort Colors

Given an array with_n\_objects colored\_red_,_white\_or\_blue_, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers`0`,`1`, and`2`to represent the color red, white, and blue respectively.

##### Notice

You are not suppose to use the library's sort function for this problem.  
You should do it in-place \(sort numbers in the original array\).

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 0, 1, 2]`, sort it in-place to`[0, 1, 1, 2]`.

[http://www.lintcode.com/en/problem/sort-colors/\#](http://www.lintcode.com/en/problem/sort-colors/#)

### 解题分析:

两次partition , 或者需要enumerate 的来分几种case都用这种方法， 传个functor进去就好

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integer which is 0, 1 or 2 
     * @return: nothing
     */
    void sortColors(vector<int> &nums) {
        // write your code here
        int start = partition(nums, 0, nums.size()-1, 1);
        start = partition(nums, start, nums.size()-1, 2);
    }

    int partition(vector<int>& nums, int beg, int end, int piv)
    {
        while(beg <= end)
        {
            while(beg<=end && nums[beg] < piv) beg++;
            while(beg<=end && nums[end] >= piv) end--;
            if (beg<=end) swap(nums[beg++], nums[end--]);
        }
        return beg;
    }
};
```

### 复杂度分析:

o\(n\)


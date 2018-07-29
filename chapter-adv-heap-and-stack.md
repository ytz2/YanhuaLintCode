## **Heap**

## 42. Trapping Rain Water

Given\_n\_non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![](/assets/TrapWater1.png)  
The above elevation map is represented by array \[0,1,0,2,1,0,1,3,2,1,2,1\]. In this case, 6 units of rain water \(blue section\) are being trapped.**Thanks Marcos**for contributing this image!

**Example:**

```
Input:
 [0,1,0,2,1,0,1,3,2,1,2,1]

Output:
 6
```

---

trap water 这种基本上是两根指针

有一道类似的题，给一堆柱子，问选左右哪个最大，哪个也是两根指针 移动最矮的。

这道题的分析如下：

左右两根指针， 选最小的，代表水位大于等于最小的， 继续移动，一直到下一个比小的要大，这个时候再比较，一直到相遇。

用的原理是最小的板子，如果近邻比他矮，那么可以存最小的高度，比他高则这一段终止，继续下一段。这个还是要画画才能看出来。

---

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.size() <=2)
            return 0;
        int l = 0, r = height.size()-1;
        int sum = 0;
        while(l <= r)
        {
            if (height[l] <= height[r])
            {
                int bar = height[l];
                while (l<=r && height[l] <= bar)
                {
                    sum +=bar - height[l];
                    l++;
                }
            }
            else
            {
                int bar = height[r];
                while(l<=r && height[r] <= bar)
                {
                    sum +=bar - height[r];
                    r--;
                }
            }
        }
        return sum;
    }
};


//更简洁的一个

class Solution {
public:
    int trap(vector<int>& height) {
        if (height.size() <=2)
            return 0;
        int l = 0, r = height.size()-1;
        int sum = 0;
        int lh = height[l], rh = height[r];
        while( l < r)
        {
            if (lh < rh)
            {
                l++;
                if (lh > height[l])
                    sum += lh-height[l];
                else
                    lh = height[l];
            }
            else
            {
                r--;
                if (rh > height[r])
                    sum += rh - height[r];
                else
                    rh = height[r];
            }
        }
        return sum;
    }
};
```

## 407. Trapping Rain Water II

Given an`m x n`matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.

**Note:**  
Bothmandnare less than 110. The height of each unit cell is greater than 0 and is less than 20,000.

**Example:**

```
Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

Return 4.
```

![](/assets/407.png)

---

用PQ+BFS遍历的一个典型的例子：

根据木桶原理，从最矮的地方灌进去， 如果灌的进去就灌，灌不进去更新周边的围墙。

另外注意的是，灌进去后要更新灌好的高度，这样BFS的时候根据neighbor可以方便计算

---

```cpp
class Solution {
public:
    int trapRainWater(vector<vector<int>>& heightMap) {
        if (heightMap.empty() || heightMap[0].empty())
            return 0;
        int m = heightMap.size();
        int n = heightMap[0].size();        
        // 用来找最矮的那个墙
        auto cmp = [&heightMap](const pair<int, int>& l, const pair<int,int>& r){
            return heightMap[l.first][l.second] > heightMap[r.first][r.second];
        };

        // 用来维护周围最矮的那个墙
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
        vector<vector<bool>> visited(m, vector<bool>(n, false));

        //把周围的墙放进去

        for (int i = 0; i < m; i++)
        {
            pq.emplace(i, 0);
            pq.emplace(i, n-1);
        }
        for (int i = 0; i < n; i++)
        {
            pq.emplace(0, i);
            pq.emplace(m-1, i);
        }

        static vector<int> dx{0, 0, 1, -1};
        static vector<int> dy{-1,1, 0,  0};

        // 开始BFS灌水， 找到最小的一角往里灌，如果灌的进去，那么最小的这个还是最小的角，如果灌不进去，那么就不能从这个角度灌了
        // 更新围墙，从下一个角度灌进去
        int ans = 0;
        while(!pq.empty())
        {
            auto p = pq.top();
            pq.pop();
            visited[p.first][p.second] = true;
            for (int i = 0; i < 4; i++)
            {
                int x = p.first + dx[i];
                int y = p.second + dy[i];
                if (x < 0 || x >= m || y < 0 || y >= n || visited[x][y])
                    continue;
                // pour in water now
                visited[x][y] = true;
                if (heightMap[p.first][p.second] > heightMap[x][y])
                {
                    ans += heightMap[p.first][p.second] - heightMap[x][y];
                    //这个地方要敲黑板划重点了： 因为维护的是周围墙的高度， 所以把这个地方最矮的墙copy过来，后面就计算依据这个高度来算
                    //或者换个思路，因为这个地方比周围的矮，所以已经被填到那个高度了
                    heightMap[x][y] = heightMap[p.first][p.second];
                }
                pq.emplace(x,y);
            }
        }
        return ans;
    }
};
```

## 

## 295. Find Median from Data Stream

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,

`[2,3,4]`, the median is`3`

`[2,3]`, the median is`(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

* void addNum\(int num\) - Add a integer number from the data stream to the data structure.
* double findMedian\(\) - Return the median of all elements so far.

**Example:**

```
addNum(1)
addNum(2)
findMedian() -
>
 1.5
addNum(3) 
findMedian() -
>
 2
```

---

老生常谈类的题目， median的特点，左边和右边长度相等，那么需要维护左边比中间小，右边比中间大。 最合适的办法就是维护左边最小堆，右面最大堆。每次选两边插入，同时更新中间的点。

---

```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }

    void addNum(int num) {
        if (lq.empty() || num < lq.top())
            lq.push(num);
        else 
            rq.push(num);
        if (lq.size() - rq.size() == 2)
        {
            rq.push(lq.top());
            lq.pop();
        }
        if (rq.size() - lq.size() == 1)
        {
            lq.push(rq.top());
            rq.pop();
        }
    }

    double findMedian() {
        if (lq.size() - rq.size() == 1)
            return lq.top();

        return double(lq.top() + rq.top()) / 2.;
    }

    // maintain lq and rq
    // to make lq.size() == rq.size() || lq.size() == rq.size()+1
    priority_queue<int> lq; // max heap
    priority_queue<int, vector<int>, greater<int>> rq;  // min heap
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```





## 480. Sliding Window Median

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:

  


`[2,3,4]`, the median is`3`

`[2,3]`, the median is`(2 + 3) / 2 = 2.5`

Given an arraynums, there is a sliding window of sizekwhich is moving from the very left of the array to the very right. You can only see theknumbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,  
Givennums=`[1,3,-1,-3,5,3,6,7]`, andk= 3.

```
Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6

```

Therefore, return the median sliding window as`[1,-1,-1,3,5,6]`.

**Note:**  
You may assume`k`is always valid, ie:`k`is always smaller than input array's size for non-empty array.

  


---

和上题类似，唯一不一样的就是要把之前的去掉再把新的加进来

两个坑

1 while\(\(int\)left.size\(\) - \(int\)right.size\(\) &gt; 1\)  size\_t这个地方会作妖

2 double\(\*left.rbegin\(\)\)/ 2.  + double\(\*right.begin\(\)\) / 2.  这里处理是为了防止overflow

3 另外注意multiset erase的时候要erase iterator，而不是value, 因为value会去掉相同的所有元素

good practice是写一个balance函数，抽插完毕balance一下

---

```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }

    void addNum(int num) {
        if (lq.empty() || num < lq.top())
            lq.push(num);
        else 
            rq.push(num);
        if (lq.size() - rq.size() == 2)
        {
            rq.push(lq.top());
            lq.pop();
        }
        if (rq.size() - lq.size() == 1)
        {
            lq.push(rq.top());
            rq.pop();
        }
    }

    double findMedian() {
        if (lq.size() - rq.size() == 1)
            return lq.top();

        return double(lq.top() + rq.top()) / 2.;
    }

    // maintain lq and rq
    // to make lq.size() == rq.size() || lq.size() == rq.size()+1
    priority_queue<int> lq; // max heap
    priority_queue<int, vector<int>, greater<int>> rq;  // min heap
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

## 






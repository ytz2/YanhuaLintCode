# Find Peak Element II {#find-peak-element-ii}

## Question {#question}

There is an integer matrix which has the following features:

The numbers in adjacent positions are different. The matrix has n rows and m columns.

```
For all i 
<
 m, A[0][i] 
<
 A[1][i] 
&
&
 A[n - 2][i] 
>
 A[n - 1][i].
For all j 
<
 n, A[j][0] 
<
 A[j][1] 
&
&
 A[j][m - 2] 
>
 A[j][m - 1].
```

We define a position P is a peek if:

`A[j][i] > A[j+1][i] && A[j][i] > A[j-1][i] && A[j][i] > A[j][i+1] && A[j][i] > A[j][i-1]`

Find a peak element in this matrix. Return the index of the peak.

Have you met this question in a real interview? Yes Example Given a matrix:

```
[
  [1 ,2 ,3 ,6 ,5],
  [16,41,23,22,6],
  [15,17,24,21,7],
  [14,18,19,20,10],
  [13,14,11,10,9]
]
```

return index of`41`\(which is`[1,1]`\) or index of`24`\(which is`[2,2]`\)

这道题用的思想是地图分割的思路。

从最基本的思路出发

1 首先分析出问题： 最外面一圈的边界比里面都小，那么里面至少存在一个maximum

2 那么可以考虑用二分，但是如何进行二维二分，那么尝试在一维进行二分

我们选取中间一行

15 17 24 21 7 ， 找到最大的24， 那么24比上下都大则24就是， 加入上面比他还大那就往上走，否则就往下走。 这里有一个问题我们会不会从中间这一样走回来？

不会，因为我们选了这一行最大，而且我们只往最大的方位走，那么也就是说我们下面的元素肯定都比当前行大。 所以这个时候会有一个

mlog\(n\) 的解法 出现

3 从一维二分到二维二分进化，因为行列式对称的，很容易就想到了先切行，再切列，再切行。。。。这样就可以每次切掉n/2， 最后是O\(M+N\) 因为我们用了O（m+n\)的复杂度把变为T（m+n / 2\)的问题，类似quick select

```cpp
#include <iostream>
#include <vector>

using namespace std;

class Solution{
public:
    int find(vector<vector<int>>& matrix, int x0, int y0, int x1, int y1, bool flag)
    {
        if (flag)
        {
            int mid = x0 + (x1-x0)/2;
            int ind = y0;
            // find the max ind in x= mid 
            for (int i = y0; i <= y1; i++)
            {
                if (matrix[mid][i] > matrix[mid][ind])
                    ind = i;
            }
            if (matrix[mid][ind] < matrix[mid-1][ind])
                return find(matrix, x0, y0, mid-1, y1, !flag);
            else if (matrix[mid][ind] < matrix[mid+1][ind])
                return find(matrix, mid+1, y0, x1, y1, !flag);
            return matrix[mid][ind];
        }

        int mid = y0 + (y1 - y0) / 2;
        int ind = x0;
        for (int i = x0; i <= x1 ; i++)
        {
            if (matrix[i][mid] > matrix[ind][mid])
                ind = mid;
        }
        if (matrix[ind][mid] < matrix[ind][mid-1])
            return find(matrix, x0, y0, x1, mid-1, !flag);
        else if (matrix[ind][mid] < matrix[ind][mid+1])
            return find(matrix, x0, mid+1, x1, y1, !flag);
        return matrix[mid][ind];
    }

    int findPeak(vector<vector<int>>& matrix)
    {
        return find(matrix, 1, 1, matrix.size()-2, matrix[0].size()-2, true);
    }
};

int main()
{
    vector<vector<int>> matrix;
    matrix.push_back({
        1 ,2 ,3 ,6 ,5
    });
    matrix.push_back({
        16,41,23,22,6
    });
    matrix.push_back({
        15,17,20,21,7
    });
    matrix.push_back({
        14,18,19,20,10
    });
    matrix.push_back({
        13,14,11,10,9
    });
    Solution sol;
    cout << sol.findPeak(matrix) << endl;
    return 0;
}
```

二分答案的思路， 求最大最小，靠猜来找答案的办法 类似sqrt

# ![](/assets/二分答案.png)Sweep Line

![](/assets/sweeplineprob.png)

# 391 Number of Airplanes in the Sky {#find-peak-element-ii}

## Question {#question}

Given an interval list which are flying and landing time of the flight. How many airplanes are on the sky at most?

### Example

For interval list

```
[
  (1,10),
  (2,3),
  (5,8),
  (4,7)
]
```

Return`3`

### Notice

If landing and flying happens at the same time, we consider landing should happen at first.

![](/assets/countFlight.png)

扫描线的本质就是对于起点和终点的事件化，并且根据事件的不同来处理

这道题目的处理方式是++和--的区别

```cpp
/**
 * Definition of Interval:
 * classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param airplanes: An interval array
     * @return: Count of airplanes are in the sky.
     */
    int countOfAirplanes(vector<Interval> &airplanes) {
        // write your code here
        if (airplanes.size() <= 1)
            return airplanes.size();
        vector<pair<int, int>> schedule;
        for (Interval& interval : airplanes){
            schedule.emplace_back(interval.start, 1);
            schedule.emplace_back(interval.end, 0);
        }
        sort(schedule.begin(), schedule.end(), [](pair<int,int>& left, pair<int,int>& right){
            if (left.first == right.first)
                return left.second < right.second;
            return left.first < right.first;
        });
        int count = 0;
        int res = 0;
        for (int i = 0; i < schedule.size(); i++ )
        {
            if (schedule[i].second == 1)
                count++;
            else
                count--;
            res = max(res, count);
        }
        return res;
    }
};
```



# 218 The Skyline Problem {#find-peak-element-ii}

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are**given the locations and height of all the buildings**as shown on a cityscape photo \(Figure A\), write a program to**output the skyline**formed by these buildings collectively \(Figure B\).

[![](https://leetcode.com/static/images/problemset/skyline1.jpg "Buildings")](https://leetcode.com/static/images/problemset/skyline1.jpg)

[![](https://leetcode.com/static/images/problemset/skyline2.jpg "Skyline Contour")](https://leetcode.com/static/images/problemset/skyline2.jpg)

The geometric information of each building is represented by a triplet of integers`[Li, Ri, Hi]`, where`Li`and`Ri`are the x coordinates of the left and right edge of the ith building, respectively, and`Hi`is its height. It is guaranteed that`0 ≤ Li, Ri ≤ INT_MAX`,`0 < Hi ≤ INT_MAX`, and`Ri - Li > 0`. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

For instance, the dimensions of all buildings in Figure A are recorded as:`[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ]`.

The output is a list of "**key points**" \(red dots in Figure B\) in the format of`[ [x1,y1], [x2, y2], [x3, y3], ... ]`that uniquely defines a skyline.**A key point is the left endpoint of a horizontal line segment**. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:`[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]`.

**Notes:**

* The number of buildings in any input list is guaranteed to be in the range
  `[0, 10000]`
  .
* The input list is already sorted in ascending order by the left x position
  `Li`
  .
* The output list must be sorted by the x position.
* There must be no consecutive horizontal lines of equal height in the output skyline. For instance,
  `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]`
  is not acceptable; the three lines of height 5 should be merged into one in the final output as such:
  `[...[2 3], [4 5], [12 7], ...]`



扫描线的本质就是对于起点和终点的事件化，并且根据事件的不同来处理

![](/assets/skyline.png)



```cpp
/**
 * Definition of Interval:
 * classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param airplanes: An interval array
     * @return: Count of airplanes are in the sky.
     */
    int countOfAirplanes(vector<Interval> &airplanes) {
        // write your code here
        if (airplanes.size() <= 1)
            return airplanes.size();
        vector<pair<int, int>> schedule;
        for (Interval& interval : airplanes){
            schedule.emplace_back(interval.start, 1);
            schedule.emplace_back(interval.end, 0);
        }
        sort(schedule.begin(), schedule.end(), [](pair<int,int>& left, pair<int,int>& right){
            if (left.first == right.first)
                return left.second < right.second;
            return left.first < right.first;
        });
        int count = 0;
        int res = 0;
        for (int i = 0; i < schedule.size(); i++ )
        {
            if (schedule[i].second == 1)
                count++;
            else
                count--;
            res = max(res, count);
        }
        return res;
    }
};
```






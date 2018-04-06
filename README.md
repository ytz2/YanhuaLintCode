# 我的Lintcode解题报告

最近发现自己偷懒了，另外发现自己以前解出来的题目自己再解的时候还要再想一遍。写一个解题报告放在git上，这样一则防止自己偷懒，二则方便自己总结



## 38 Search a 2D Matrix II

Write an efficient algorithm that searches for a value in an m x n matrix, return the occurrence of it.

This matrix has the following properties:

* Integers in each row are sorted from left to right.
* Integers in each column are sorted from up to bottom.
* No duplicate integers in each row or column.

Have you met this question in a real interview?

Yes

**Example**

Consider the following matrix:

```
[
  [1, 3, 5, 7],
  [2, 4, 7, 8],
  [3, 5, 9, 10]
]

```

Given target =`3`, return`2`.

http://www.lintcode.com/en/problem/search-a-2d-matrix-ii

### 解题分析:



```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]。
```

复杂度要求——O\(m+n\) time and O\(1\) extra space，同时输入只满足自顶向下和自左向右的升序，行与行之间不再有递增关系，与上题有较大区别。时间复杂度为线性要求，因此可从元素排列特点出发，从一端走向另一端无论如何都需要m+n步，因此可分析对角线元素。

首先分析如果从左上角开始搜索，由于元素升序为自左向右和自上而下，因此如果target大于当前搜索元素时还有两个方向需要搜索，不太合适。

如果从右上角开始搜索，由于左边的元素一定不大于当前元素，而下面的元素一定不小于当前元素，因此每次比较时均可排除一列或者一行元素（大于当前元素则排除当前行，小于当前元素则排除当前列，由矩阵特点可知），可达到题目要求的复杂度。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param matrix: A list of lists of integers
     * @param target: An integer you want to search in matrix
     * @return: An integer indicate the total occurrence of target in the given matrix
     */
    int searchMatrix(vector<vector<int>> &matrix, int target) {
        // write your code here
        int m = matrix.size();
        if (m==0)
            return 0;
        int n = matrix[0].size();
        if (n==0)
            return 0;
        int i = m-1, j=0;
        int res = 0;
        while(i >= 0 && j <n)
        {
            auto val = matrix[i][j];
            if (val == target)
            {
                res++;
                i--;j++;
            }
            else if (val < target)
            {
                j++;
            }
            else
            {
                i--;
            }
        }
        return res;
    }
};
```

### 复杂度分析:

O（m+n\) 



## 600 Smallest Rectangle Enclosing Black Pixels

An image is represented by a binary matrix with`0`as a white pixel and`1`as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location`(x, y)`of one of the black pixels, return the area of the smallest \(axis-aligned\) rectangle that encloses all black pixels.

Have you met this question in a real interview?

Yes

**Example**

For example, given the following image:

```
[
  "0010",
  "0110",
  "0100"
]

```

and x =`0`, y =`2`,  
Return`6`.

http://www.lintcode.com/en/problem/smallest-rectangle-enclosing-black-pixels/\#

### 解题分析:

写了一个BFS， 虽然题目要求是用BS， 我觉得BS写出来和狗屎差不多， BFS就够了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param image: a binary matrix with '0' and '1'
     * @param x: the location of one of the black pixels
     * @param y: the location of one of the black pixels
     * @return: an integer
     */
    int minArea(vector<vector<char>> &image, int x, int y) {
        // write your code here
        int m = image.size();
        if (0==m)
            return 0;
        int n = image[0].size();
        if (0==n)
            return 0;
        if (x <0 || x >=m || y<0 || y>=n || image[x][y] == '0')
            return 0;
        queue<pair<int,int>> q;
        q.push(make_pair(x,y));
        image[x][y] = '0';
        int top = INT_MAX, right = INT_MIN, down = INT_MIN, left = INT_MAX;  
        vector<int> dx{-1, 1, 0, 0};
        vector<int> dy{ 0, 0, -1,1};
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            top = min(top, p.second);
            down = max(down, p.second);
            left = min(left, p.first);
            right = max(right, p.first);
            for (int i = 0; i < 4; i++)
            {
                int new_x = p.first + dx[i];
                int new_y = p.second +dy[i];
                if (new_x >= 0 && new_x < m && new_y >= 0 && new_y < n && image[new_x][new_y] == '1')
                {
                    q.push(make_pair(new_x,new_y));
                    image[new_x][new_y] = '0';
                }
            }
        }
        
        return (right-left+1)*(down-top+1);
    }
};
```

### 复杂度分析:

O（s\) 

Note:

二分搜索实际上是在找行上的最左面，最右面，列上的最左面，最右面， 二分的时候就是把点压缩到一维，ooxx的找左右边界，有点二分答案的味道。

```
class Solution {
public:
    /**
     * @param image a binary matrix with '0' and '1'
     * @param x, y the location of one of the black pixels
     * @return an integer
     */
    int minArea(vector<vector<char>>& image, int x, int y) {
        // Write your code here
        int m = image.size();
        if (m == 0)
            return 0;
        int n = image[0].size();
        if (n == 0)
            return 0;

        int start = y;
        int end = n - 1;
        int mid;
        while (start < end) {
            mid = start + (end - start) / 2 + 1;
            if (checkColumn(image, mid)) {
                start = mid;
            } else {
                end = mid - 1;
            }
        }
        int right = start;
        
        start = 0;
        end = y;
        while (start < end) {
            mid = start + (end - start) / 2;
            if (checkColumn(image, mid)) {
                end = mid;
            } else {
                start = mid + 1;
            }
        }
        int left = start;
        
        start = x;
        end = m - 1;
        while (start < end) {
            mid = start + (end - start) / 2 + 1;
            if (checkRow(image, mid)) {
                start = mid;
            } else {
                end = mid - 1;
            }
        }
        int down = start;
        
        start = 0;
        end = x;
        while (start < end) {
            mid = start + (end - start) / 2;
            if (checkRow(image, mid)) {
                end = mid;
            } else {
                start = mid + 1;
            }
        }
        int up = start;
        
        return (right - left + 1) * (down - up + 1);
    }
    
    bool checkColumn(vector<vector<char>>& image, int col) {
        for (int i = 0; i < image.size(); i++) {
            if (image[i][col] == '1') {
                return true;
            }
        }
        return false;
    }
    
    bool checkRow(vector<vector<char>>& image, int row) {
        for (int j = 0; j < image[0].size(); j++) {
            if (image[row][j] == '1') {
                return true;
            }
        }
        return false;
    }
};
```




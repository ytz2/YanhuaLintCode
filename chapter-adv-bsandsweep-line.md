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










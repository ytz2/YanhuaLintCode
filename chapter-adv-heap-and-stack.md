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
        if (height.size() <= 1) return 0;
        int sum = 0;
        int l = 0, r = height.size() -1;
        int hl = height[l], hr = height[r];

        while( l < r) {
            if (hl <= hr) {
                if (height[l] <= hl) {
                    sum += hl - height[l++]; 
                } else {
                    hl = height[l];
                }
            } else {
                if (height[r] <= hr) {
                    sum += hr - height[r--];
                } else {
                    hr = height[r];
                }
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


class Solution {
public:
    int trapRainWater(vector<vector<int>>& heightMap) {
        if (heightMap.empty() || heightMap[0].empty()) return 0;
        int m = heightMap.size();
        int n = heightMap[0].size();

        auto cmp = [&heightMap, n](const int i, const int j) {
            return heightMap[i/n][i%n] > heightMap[j/n][j%n];
        };
        priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        for (int i = 0; i < m; i++) {
            pq.push(i*n);
            pq.push(i*n + n -1);
            visited[i][0] = visited[i][n-1] = true;
        }
        for (int j = 1; j < n - 1; j++) {
            pq.push(j);
            pq.push((m-1)*n + j);
            visited[0][j] = visited[m-1][j] = true;
        }
        static const vector<int> dx{-1, 1, 0, 0};
        static const vector<int> dy{0, 0, -1, 1};
        int res = 0;
        while(!pq.empty()) {
            auto ind = pq.top();pq.pop();
            int x = ind / n, y = ind % n;
            visited[x][y] = true;

            for (int i = 0; i < 4; i++){
                auto nx = x + dx[i];
                auto ny = y + dy[i];
                if (nx < 0 || nx >= m || ny < 0 || ny >= n || visited[nx][ny])
                    continue;
                if (heightMap[nx][ny] < heightMap[x][y]) {
                    res += heightMap[x][y] - heightMap[nx][ny];
                    heightMap[nx][ny] = heightMap[x][y];  
                }
                pq.push(nx * n + ny);
             }
        }
        return res;
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
        if (left.empty() || num < left.top()) left.push(num);
        else right.push(num);
        if(left.size() - right.size() == 2) {
            right.push(left.top());
            left.pop();
        }
        if ( right.size() - left.size() == 1) {
            left.push(right.top());
            right.pop();
        }
    }

    double findMedian() {
        if (left.size() == right.size()) return double(left.top() +  right.top()) / 2;
        return left.top();
    }

    priority_queue<int> left;
    priority_queue<int, vector<int>, greater<int>> right;
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
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
class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        if (k == 1) return vector<double>(nums.begin(), nums.end());
        multiset<int> tmp(nums.begin(), next(nums.begin(), k));
        multiset<int> left(tmp.begin(), next(tmp.begin(), (k + 1) / 2));
        multiset<int> right(next(tmp.begin(), (k + 1) /2), next(tmp.begin(), k));
        auto med = [&left,&right]() {
            if (left.size() > right.size()) return double(*left.rbegin());
            return (double(*left.rbegin()) + double(*right.begin())) / 2;
        };
        vector<double> result;
        auto balance = [&left, &right]() {
          while(left.size() > right.size() + 1) {
              right.insert(*left.rbegin());
              left.erase(prev(left.end()));
          }  
          while(right.size() > left.size()) {
              left.insert(*right.begin());
              right.erase(right.begin());
          }
        };
        result.push_back(med());
        for (int i = k; i < nums.size(); i++) {
            auto toAdd = nums[i];
            auto toErase = nums[i - k];
            if (toAdd > *left.rbegin()) {
                right.insert(toAdd);
            } else {
                left.insert(toAdd);
            }
            if (toErase <= *left.rbegin()){
                left.erase(left.lower_bound(toErase));
            } else {
                right.erase(right.lower_bound(toErase));
            }
            balance();
            result.push_back(med());
        }
        return result;
    }



};

 class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        vector<double> result;
        multiset<int> container(nums.begin(), next(nums.begin(), k));
        auto midIt = next(container.begin(), (k - 1) /2);
        auto push = [&](){
          if (k%2 == 0) {
              result.push_back((double(*midIt) + double(*next(midIt))) / 2);
          } else {
              result.push_back(*midIt);
          } 
        };
        push();
        for (int i = k; i < nums.size(); i++) {
            auto toAdd = nums[i];
            auto toErase = nums[i - k];
            container.insert(toAdd);
            if (k % 2)  {
                if (toAdd < *midIt) 
                    midIt--;
                if (toErase <= *midIt )
                    midIt++;
                container.erase(container.lower_bound(toErase));
            } else {
                if (toAdd >= *midIt) midIt++;
                if (toErase >= *midIt) midIt--;
                container.erase(prev(container.upper_bound(toErase)));
            }
            push();
        }
        return result;
    }
};
```

## ![](/assets/slidingwindowA.png)

## **Stack**

## 394. Decode String

---

Given an encoded string, return it's decoded string.

The encoding rule is:`k[encoded_string]`, where theencoded\_stringinside the square brackets is being repeated exactlyktimes. Note thatkis guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers,k. For example, there won't be input like`3a`or`2[4]`.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

---

当需要保存状态的时候， 就要想到stack， 还有翻转（reverse list）,  再就是递归转遍历等等

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

## 84. Largest Rectangle in Histogram

---

Given

_n_

non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![](/assets/lc84.png)

**Example:**

```
Input:
 [2,1,5,6,2,3]

Output:
 10
```

```cpp
/*

这道题是一道单调stack的问题，如何转化：
最大的rectangle实际上就是 以高度为起点，左右扫，找到第一个比他小的，然后宽度确定，高度以当前高度为准
就是以此高度为基准的最大面积

单调栈解决的就是左面和右面第一个小于的问题 （递增的时候）

基本上是一个套路模板， 维护一个递增stack， 当遇到小于等于的时候，则栈顶可以确定右面第一个小于的是当前
index,而左面第一个小于的是栈上第二个，则可以确定

中间有个小trick， 遍历的时候最后要放一个-1进去把栈放空

*/


class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int ans = 0;
        stack<int> stk;
        for (int i = 0; i <= heights.size(); i++)
        {
            int v = (i == heights.size() ? -1 : heights[i]);
            while(!stk.empty() && v <= heights[stk.top()])
            {
                int h = heights[stk.top()];
                stk.pop();
                int w = stk.empty()? i : i-stk.top()-1;
                ans = max(ans, w*h);
            }
            stk.push(i);
        }
        return ans;
    }
};
```

## 224. Basic Calculator

---

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open`(`and closing parentheses`)`, the plus`+`or minus sign`-`,**non-negative**integers and empty spaces.

**Example 1:**

```
Input:
 "1 + 1"

Output:
 2
```

**Example 2:**

```
Input:
 " 2-1 + 2 "

Output:
 3
```

**Example 3:**

```
Input:
 "(1+(4+5+2)-3)+(6+8)"

Output:
 23
```

**Note:**

* You may assume that the given expression is always valid.
* **Do not**
  use the
  `eval`
  built-in library function.

这道题做了好久，还是看了答案，总结如下：

1.不需要用逆波兰表达式来做， 杀鸡用牛刀

1. 带括号和加减的计算 不需要考虑precedence, 碰到数往数堆上放，碰到operator往operators上放

2. 触发计算有两个条件

   左边有operator \(这个时候堆上有 一个操作符且不是（\)

   operator为（， 作为计算终止符， 因为我们每次碰到+-都已经计算过一次了，所以\)必然对应的是一个（， pop掉，再触发计算

   注意的一个条件是（（（）））的情况，所以不需要计算的check是

   ops.empty\(\) \|\| ops.top\(\) == ''\(" ， 这个时候需要等待下一个）来pop 堆上的（

```cpp
class Solution {
public:
    int calculate(string s) {
        stack<int> operands;
        stack<char> ops;
        int number = 0;
        for ( int i = 0; i < s.size(); i++)
        {
            auto c = s[i];
            if (c == ' ')
                continue;
            else if (c == '+' || c =='-' || c =='(')
            {
                ops.push(c);
                continue;
            }
            if (c == ')') ops.pop();
            else 
            {
                number = number * 10 + c -'0';
                if (i < s.size()-1 && isdigit(s[i+1]))
                    continue;
                operands.push(number);
                number = 0;
            }
            if (ops.empty() || ops.top() == '(')
                continue;
            int num = operands.top(); operands.pop();
            if (ops.top() == '+') operands.top() += num;
            else operands.top() -= num;
            ops.pop();
        }
        return operands.top();
    }


};
```

## 227. Basic Calculator II

---

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only**non-negative**integers,`+`,`-`,`*`,`/`operators and empty spaces. The integer division should truncate toward zero.

**Example 1:**

```
Input: 
"3+2*2"

Output:
 7
```

**Example 2:**

```
Input:
 " 3/2 "

Output:
 1
```

**Example 3:**

```
Input:
 " 3+5 / 2 "

Output:
 5
```

**Note:**

* You may assume that the given expression is always valid.
* **Do not**
  use the
  `eval`
  built-in library function.
* 这道题和上道题类似，关键点在于想如何触发计算

因为这里有了precedence,所以无论加减乘除都往上放， 唯一触发的计算是乘号和除号， 触发完了放到栈上去

最后栈上剩下的只有加和减， 这个时候把栈倒过来，从头计算 （因为 倒着计算结果不对，1-1+1\)

```cpp
class Solution {
public:
    int calculate(string s) {

        stack<int> operands;
        stack<char> operators;
        int num = 0;
        for (int i = 0; i < s.size(); i++)
        {
            auto c = s[i];
            if (c == ' ') continue;
            if ( c == '+' || c == '-' || c== '*' || c == '/')
            {
                operators.push(c);
                continue;
            }
            if (isdigit(c))
            {
                num = num * 10 + c - '0';
                if (i < s.size() - 1 && isdigit(s[i+1]))
                    continue;
                operands.push(num);
                num = 0;
            }
            if (operators.empty() || operators.top() == '+' || operators.top() == '-')
                continue;
            int v = operands.top(); operands.pop();
            if (operators.top() == '*') operands.top() *= v;
            else operands.top() /= v;
            operators.pop();
        }
        stack<int> operandsdup;
        stack<char> operatorsdup;
        while(!operands.empty())
        {
            operandsdup.push(operands.top());
            operands.pop();
        }
        while(!operators.empty())
        {
            operatorsdup.push(operators.top());
            operators.pop();
        }

        while(!operatorsdup.empty())
        {
            int num = operandsdup.top();operandsdup.pop();
            if (operatorsdup.top() == '+') 
                operandsdup.top() += num;
            else 
                operandsdup.top() =  num -operandsdup.top();
            operatorsdup.pop();
        }

        return operandsdup.top();
    }
};
```

稍微用isNegative flag优化一下

```
class Solution {
```

```cpp
class Solution {
public:
    int calculate(string s) {

        stack<int> operands;
        stack<char> operators;
        int num = 0;
        bool isNegative = false;
        for (int i = 0; i < s.size(); i++)
        {
            auto c = s[i];
            if (c == ' ') continue;
            if ( c == '+' || c == '-' || c== '*' || c == '/')
            {
                operators.push(c);
                if (c == '-')
                    isNegative = true;
                continue;
            }
            if (isdigit(c))
            {
                num = num * 10 + c - '0';
                if (i < s.size() - 1 && isdigit(s[i+1]))
                    continue;
                operands.push(isNegative? -num : num);
                isNegative = false;
                num = 0;
            }
            if (operators.empty() || operators.top() == '+' || operators.top() == '-')
                continue;
            int v = operands.top(); operands.pop();
            if (operators.top() == '*') operands.top() *= v;
            else operands.top() /= v;
            operators.pop();
        }
        int res = 0;
        while(!operands.empty())
        {
            res += operands.top();
            operands.pop();
        }
        return res;
    }
};
```




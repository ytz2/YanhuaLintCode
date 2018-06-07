## 642. Moving Average from Data Stream

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

### Example

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1 // return 1.00000
m.next(10) = (1 + 10) / 2 // return 5.50000
m.next(3) = (1 + 10 + 3) / 3 // return 4.66667
m.next(5) = (10 + 3 + 5) / 3 // return 6.00000
```

https://www.lintcode.com/problem/moving-average-from-data-stream/description

### 解题分析:

一个q就好了

### 代码：

```cpp
class MovingAverage {
public:
    /*
    * @param size: An integer
    */MovingAverage(int size)
        : size(size), total(0)
    {
        // do intialization if necessary
    }

    /*
     * @param val: An integer
     * @return:  
     */
    double next(int val) {
        // write your code here
        total += val;
        q.push(val);
        if (q.size() > size )
        {
            total -= q.front();
            q.pop();
        }
        return double(total)/q.size();
    }
    
private:
    int size; 
    long total;
    queue<int> q;
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param = obj.next(val);
 */
```




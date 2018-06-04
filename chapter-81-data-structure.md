# Stack

## 495. Implement Stack

Implement a stack. You can use any data structure inside a stack except stack itself to implement it.

### Example

```
push(1)
pop()
push(2)
top()  // return 2
pop()
isEmpty() // return true
push(3)
isEmpty() // return false
```

[https://www.lintcode.com/problem/implement-stack/description](https://www.lintcode.com/problem/implement-stack/description)

### 解题分析:

当年面CGG的一道题。。。哈哈哈哈

### 代码：

```cpp
#include <list>
class Stack {
public:
    /*
     * @param x: An integer
     * @return: nothing
     */
    void push(int x) {
        // write your code here
        store.push_front(x);
    }

    /*
     * @return: nothing
     */
    void pop() {
        // write your code here
        if (!isEmpty())
            store.pop_front();
    }

    /*
     * @return: An integer
     */
    int top() {
        // write your code here
        if (!isEmpty())
            return store.front();
        return INT_MAX;
    }

    /*
     * @return: True if the stack is empty
     */
    bool isEmpty() {
        // write your code here
        return store.empty();
    }


private:
    deque<int> store;
};
```



## 494. Implement Stack by Two Queues

Implement a stack by two queues. The queue is first in first out \(FIFO\). That means you can not directly pop the last element in a queue.

### Example

```
push(1)
pop()
push(2)
isEmpty() // return false
top() // return 2
pop()
isEmpty() // return true
```

https://www.lintcode.com/problem/implement-stack-by-two-queues/description

### 解题分析:

两个q，一个存储，一个调整顺序， push to q1, pop/top的时候把q1的丢到q2里，然后处理最后一个元素，最后swap\(q1,q2\)

### 代码：

```cpp
class Stack {
public:
    /*
     * @param x: An integer
     * @return: nothing
     */
    void push(int x) {
        // write your code here
        q1.push(x);
    }

    /*
     * @return: nothing
     */
    void pop() {
        // write your code here
        flapTillLast();
        q1.pop();
        swap(q1, q2);
    }

    /*
     * @return: An integer
     */
    int top() {
        // write your code here
        assert(!q1.empty());
        flapTillLast();
        int res = q1.front();
        q1.pop();
        q2.push(res);
        swap(q1,q2);
        return res;
    }

    /*
     * @return: True if the stack is empty
     */
    bool isEmpty() {
        // write your code here
        return q1.empty();
    }
private:
    
    void flapTillLast()
    {
        if (q1.empty())
            return;
        while(q1.size() != 1)
        {
            q2.push(q1.front());
            q1.pop();
        }
    }
    
    queue<int> q1;
    queue<int> q2;
};
```






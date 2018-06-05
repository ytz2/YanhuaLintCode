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

[https://www.lintcode.com/problem/implement-stack-by-two-queues/description](https://www.lintcode.com/problem/implement-stack-by-two-queues/description)

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



## 224. Implement Three Stacks by Single Array

IImplement three stacks by single array.

You can assume the three stacks has the same size and big enough, you don't need to care about how to extend it if one of the stack is full.

### Example

```
ThreeStacks(5)  // create 3 stacks with size 5 in single array. stack index from 0 to 2
push(0, 10) // push 10 in the 1st stack.
push(0, 11)
push(1, 20) // push 20 in the 2nd stack.
push(1, 21)
pop(0) // return 11
pop(1) // return 21
peek(1) // return 20
push(2, 30)  // push 30 in the 3rd stack.
pop(2) // return 30
isEmpty(2) // return true
isEmpty(0) // return false
```

https://www.lintcode.com/problem/implement-three-stacks-by-single-array/description

### 解题分析:

网上有很fancy的动态内存， 实在是太复杂了， 写一个constant mem的版本

### 代码：

```cpp
class ThreeStacks {
public:
    /*
    * @param size: An integer
    */ThreeStacks(int size) {
        // do intialization if necessary
        size_ = size;
        for (int i = 0; i < 3; i++)
            indices_.push_back(i*size_-1);
        buffer_.reserve(3*size);
    }

    /*
     * @param stackNum: An integer
     * @param value: An integer
     * @return: nothing
     */
    void push(int stackNum, int value) {
        // Push value into stackNum stack
        int end = (stackNum+1) * size_ - 1;
        if (indices_[stackNum] == end )
            return; 
        buffer_[++indices_[stackNum]] = value;
    }

    /*
     * @param stackNum: An integer
     * @return: the top element
     */
    int pop(int stackNum) {
        // Pop and return the top element from stackNum stack
        int beg = stackNum * size_ - 1;
        if (indices_[stackNum] == beg)
            return 0;
        return buffer_[indices_[stackNum]--];
    }

    /*
     * @param stackNum: An integer
     * @return: the top element
     */
    int peek(int stackNum) {
        // Return the top element
        if (indices_[stackNum] == stackNum * size_ - 1)
            return 0;
        return buffer_[indices_[stackNum]];
    }

    /*
     * @param stackNum: An integer
     * @return: true if the stack is empty else false
     */
    bool isEmpty(int stackNum) {
        // write your code here
        
        return indices_[stackNum] == stackNum * size_ -1;
    }
    
private:
    int size_;
    vector<int> buffer_;
    vector<int> indices_;
};
```




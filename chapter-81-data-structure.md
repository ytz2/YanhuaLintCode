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

[https://www.lintcode.com/problem/implement-three-stacks-by-single-array/description](https://www.lintcode.com/problem/implement-three-stacks-by-single-array/description)

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

## 494.Implement Stack by Two Queues

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

画一下图就可以了, sk2是从stk1 过去的，倒转的时候 stk2的最顶上就是q的top了。

### 代码：

```cpp
class MyQueue {
public:
    MyQueue() {
        // do intialization if necessary
    }

    /*
     * @param element: An integer
     * @return: nothing
     */
    void push(int element) {
        // write your code here
         stk1_.push(element);
    }

    /*
     * @return: An integer
     */
    int pop() {
        // write your code here
        int res = top();
        assert(!stk2_.empty());
        stk2_.pop();
        return res;
    }

    /*
     * @return: An integer
     */
    int top() {
        // write your code here
        if (!stk2_.empty())
            return stk2_.top();
        flap();
        assert(!stk2_.empty());
        return stk2_.top();
    }

private:
    void flap()
    {
        while(!stk1_.empty())
        {
            stk2_.push(stk1_.top());
            stk1_.pop();
        }
    }
private:
    stack<int> stk1_, stk2_;
};
```

## 494.Implement Stack by Two Queues

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

画一下图就可以了, sk2是从stk1 过去的，倒转的时候 stk2的最顶上就是q的top了。

### 代码：

```cpp
class MyQueue {
public:
    MyQueue() {
        // do intialization if necessary
    }

    /*
     * @param element: An integer
     * @return: nothing
     */
    void push(int element) {
        // write your code here
         stk1_.push(element);
    }

    /*
     * @return: An integer
     */
    int pop() {
        // write your code here
        int res = top();
        assert(!stk2_.empty());
        stk2_.pop();
        return res;
    }

    /*
     * @return: An integer
     */
    int top() {
        // write your code here
        if (!stk2_.empty())
            return stk2_.top();
        flap();
        assert(!stk2_.empty());
        return stk2_.top();
    }

private:
    void flap()
    {
        while(!stk1_.empty())
        {
            stk2_.push(stk1_.top());
            stk1_.pop();
        }
    }
private:
    stack<int> stk1_, stk2_;
};
```

## 955.Implement Queue by Circular Array

IImplement queue by circulant array. You need to support the following methods:

1. `CircularQueue(n):`
   initialize a circular array with size n to store elements
2. `boolean isFull():`
   return
   `true`
   if the array is full
3. `boolean isEmpty():`
   return
   `true`
   if there is no element in the array
4. `void enqueue(element):`
   add an element to the queue
5. `int dequeue():`
   pop an element from the queue

### Example

```
CircularQueue(5)
isFull()   =
>
 false
isEmpty() =
>
 true
enqueue(1)
dequeue()  =
>
 1
```

[https://www.lintcode.com/problem/implement-queue-by-circular-array/description](https://www.lintcode.com/problem/implement-queue-by-circular-array/description)

### 解题分析:

维护一个front指向当前头，一个rear，指向最后元素的下一位， 维护一个size \(因为是circular的，算不清楚size\)。 这样就可以用Mod来更新front, rear, ++/-- size了

### 代码：

```cpp
class CircularQueue {
public:
    /**
     * @return:  return true if the array is full
     */
    CircularQueue(int n) {
        // do intialization if necessary
        front = 0;
        rear = 0;
        size = 0;
        buffer_ = vector<int>(n, 0);
    }
    bool isFull() {
        // write your code here
        return size == buffer_.size();
    }

    /**
     * @return: return true if there is no element in the array
     */
    bool isEmpty() {
        // write your code here
        return size == 0;
    }

    /**
     * @param element: the element given to be added
     * @return: nothing
     */
    void enqueue(int element) {
        // write your code here
        assert(!isFull());
        buffer_[rear] = element;
        rear = ++rear % buffer_.size();
        size++;
    }

    /**
     * @return: pop an element from the queue
     */
    int dequeue() {
        // write your code here
        assert(!isEmpty());
        auto val = buffer_[front];
        front = ++front%buffer_.size();
        size--;
        return val;
    }

private:
    int front;
    int rear;
    int size;
    vector<int> buffer_;
};
```



## 130.Heapify

Given an integer array, heapify it into a min-heap array.

For a heap array A, A\[0\] is the root of heap, and for each A\[i\], A\[i \* 2 + 1\] is the left child of A\[i\] and A\[i \* 2 + 2\] is the right child of A\[i\].



### Example

Given \[3,2,1,4,5\], return \[1,2,3,4,5\] or any legal heap array.

### Challenge

O\(n\) time complexity

https://www.lintcode.com/problem/heapify/leaderboard

[https://www.lintcode.com/problem/implement-queue-by-circular-array/description](https://www.lintcode.com/problem/implement-queue-by-circular-array/description)

### 解题分析:

heapfy用两种方法: siftup， 一直换到老子比儿子小  ， 因为从k只能反推一个爹出来，所以要从1 到 n遍历

                                 siftdown,   一直换到老子比儿子小， 因为k可以搞两个出来，所以从n/2 到0反着搞一遍就好了 o\(n\)

### 代码：

```cpp
class Solution {
public:
    /*
     * @param A: Given an integer array
     * @return: nothing
     */
    void heapify(vector<int> &A) {
        // write your code here
        //for (int i = 1; i < A.size(); i++)
        //    siftup(A,i);
        for (int i = A.size()/2; i >=0; i--)
            siftdown(A, i);
    }
    
    void siftup(vector<int>& A, int k)
    {
        while(k > 0)
        {
            int father = (k - 1)/2;
            if (A[father] < A[k])
                break;
            swap(A[father], A[k]);
            k = father;
        }
    }
    
    void siftdown(vector<int>& A, int k)
    {
        while(k < A.size())
        {
            int child1 = 2 * k + 1;
            int child2 = 2 * k + 2;
            int smallest = k;
            if (child1 < A.size() )
                smallest = A[child1] < A[smallest] ? child1 : k;
            if (child2 < A.size() )
                smallest = A[child2] < A[smallest] ? child2 : smallest;
            if (k == smallest)
                break;
            swap(A[smallest], A[k]);
            k = smallest;
        }
    }
    
};
```




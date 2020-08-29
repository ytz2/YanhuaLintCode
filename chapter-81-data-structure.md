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
class Stack {
public:
    void push(int x) {
        if (data.size() == size) {
            data.push_back(x);
            size++;
        } else {
            data[size] = x;
            size++;
        }
    }
    void pop() {
        size--;
    }

    int top() {
        return data[size-1];
    }

    bool isEmpty() {
        return size == 0;
    }

private:
    vector<int> data;
    size_t size;
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

[https://leetcode.com/problems/implement-stack-using-queues/](https://leetcode.com/problems/implement-stack-using-queues/)

### 解题分析:

两个q，一个存储，一个调整顺序， push to q1, pop/top的时候把q1的丢到q2里，然后处理最后一个元素，最后swap\(q1,q2\)

### 代码：

```cpp
class MyStack {
public:
    /** Initialize your data structure here. */
    MyStack() {

    }

    /** Push element x onto stack. */
    void push(int x) {
        q1.push(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        flap();
        auto res = q2.front();
        q2.pop();
        return res;
    }

    /** Get the top element. */
    int top() {
        flap();
        auto res = q2.front();
        q1.push(q2.front());
        q2.pop();
        return res;
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return q1.empty() && q2.empty();
    }

private:
    void flap() {
        while(q1.size() > 1) {
            q2.push(q1.front());
            q1.pop();
        }
        swap(q1, q2);
    }
private:
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

## 494.Implement Queue by Two Stacks

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

[https://leetcode.com/problems/implement-queue-using-stacks/](https://leetcode.com/problems/implement-queue-using-stacks/)

### 解题分析:

画一下图就可以了, sk2是从stk1 过去的，倒转的时候 stk2的最顶上就是q的top了。

### 代码：

```cpp
class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        stk1.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if (stk2.empty()) flap();
        auto res = stk2.top();
        stk2.pop();
        return res;
    }

    /** Get the front element. */
    int peek() {
        if (stk2.empty()) flap();
        return stk2.top();
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return stk1.empty() && stk2.empty();
    }
private:
    void flap() {
        while(!stk1.empty()) {
            stk2.push(stk1.top());
            stk1.pop();
        }
    }
private:
    stack<int> stk1, stk2;    
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
    CircularQueue(int n) {
        n_ = n;
        data_ = vector<int>(n + 1, 0);
    }
    bool isFull() {
        return next(tail) == head;
    }
    bool isEmpty() {
        return head == tail;
    }
    void enqueue(int element) {
        data_[tail] = element;
        if (isEmpty()) head = tail;
        tail = next(tail);
    }
    int dequeue() {
        auto v = data_[head];
        head = next(head);
        return v;
    }
    
private:

    int next(int v) {
        return (v + 1) % (n_ + 1);
    }
    
    int n_ = 0;
    vector<int> data_;
    int head = 0;
    int tail = 0;
};
```

## 130.Heapify

Given an integer array, heapify it into a min-heap array.

For a heap array A, A\[0\] is the root of heap, and for each A\[i\], A\[i \* 2 + 1\] is the left child of A\[i\] and A\[i \* 2 + 2\] is the right child of A\[i\].

### Example

Given \[3,2,1,4,5\], return \[1,2,3,4,5\] or any legal heap array.

### Challenge

O\(n\) time complexity



解题分析:

heapfy用两种方法: siftup， 一直换到老子比儿子小  ， 因为从k只能反推一个爹出来，所以要从1 到 n遍历

```
                             siftdown,   一直换到老子比儿子小， 因为k可以搞两个出来，所以从n/2 到0反着搞一遍就好了 o\(n\)
```

### 代码：

```cpp
class Solution {
public:

    void heapify(vector<int> &A) {
        //for (int i = A.size() / 2; i >= 0; i--) helper(A, i);
        for (int i = 0; i < A.size(); i++) helper2(A, i);
    }
    
    void helper(vector<int>& A, int index) {
        while(index < A.size()) {
            int minInd = index;
            auto l = 2 * index + 1;
            auto r = l + 1;
            if (l < A.size() && A[l] < A[minInd]) minInd = l;
            if (r < A.size() && A[r] < A[minInd]) minInd = r;
            if (index == minInd) break;
            swap(A[index], A[minInd]);
            index = minInd;
        }
    } 
    
    void helper2(vector<int>& A, int index) {
        while(index > 0) {
            int parent = (index - 1) / 2;
            if (A[parent] > A[index]) swap(A[parent], A[index]);
            else break;
            index = parent;
        }
    }
};
```




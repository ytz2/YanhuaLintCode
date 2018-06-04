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

https://www.lintcode.com/problem/implement-stack/description

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




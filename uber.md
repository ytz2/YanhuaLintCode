## 636. Exclusive Time of Functions

---

Given the running logs of**n**functions that are executed in a nonpreemptive single threaded CPU, find the exclusive time of these functions.

Each function has a unique id, start from**0**to**n-1**. A function may be called recursively or by another function.

A log is a string has this format :`function_id:start_or_end:timestamp`. For example,`"0:start:0"`means function 0 starts from the very beginning of time 0.`"0:end:0"`means function 0 ends to the very end of time 0.

Exclusive time of a function is defined as the time spent within this function, the time spent by calling other functions should not be considered as this function's exclusive time. You should return the exclusive time of each function sorted by their function id.

**Example 1:**

```
Input:

n = 2
logs = 
["0:start:0",
 "1:start:2",
 "1:end:5",
 "0:end:6"]

Output:
[3, 4]

Explanation:

Function 0 starts at time 0, then it executes 2 units of time and reaches the end of time 1. 
Now function 0 
calls function 1
, function 1 starts at time 2, executes 4 units of time and end at time 5.
Function 0 is running again at time 6, and also end at the time 6, thus executes 1 unit of time. 
So function 0 totally execute 2 + 1 = 3 units of time, and function 1 totally execute 4 units of time.
```

**Note:**

1. Input logs will be sorted by timestamp, NOT log id.
2. Your output should be sorted by function id, which means the 0th element of your output corresponds to the exclusive time of function 0.
3. Two functions won't start or end at the same time.
4. Functions could be called recursively, and will always end.
5. 1 
   &lt;
   = n 
   &lt;
   = 100

[https://leetcode.com/problems/exclusive-time-of-functions/description/](https://leetcode.com/problems/exclusive-time-of-functions/description/)

```cpp
class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        vector<int> result(n, 0);
        stack<int> stk;
        string tag;
        int id, t;
        int prev = 0;
        for (auto& log : logs)
        {    
            parse(log, id, tag, t);
            if (tag[0] == 's')
            {
                if (!stk.empty())
                {
                    result[stk.top()] += t - prev;
                }
                stk.push(id);
            }
            else
            {
                result[stk.top()] += ++t - prev;
                stk.pop();
            }
            prev = t;
        }
        return result;
    }

    void parse(string& log, int& id, string& tag, int& t)
    {
        stringstream ss(log);
        string token;
        getline(ss, token, ':');
        id = stoi(token);
        getline(ss, token, ':');
        tag = token;
        getline(ss, token);
        t = stoi(token);
    }
};
};
```

## 36. Valid Sudoku

---

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits 
   `1-9`
   without repetition.
2. Each column must contain the digits 
   `1-9`
    without repetition.
3. Each of the 9
   `3x3`
   sub-boxes of the grid must contain the digits 
   `1-9`
    without repetition.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)  
A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character`'.'`.

**Example 1:**

```
Input:

[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]

Output:
 true
```

**Example 2:**

```
Input:

[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]

Output:
 false

Explanation:
 Same as Example 1, except with the 
5
 in the top left corner being 
    modified to 
8
. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Note:**

* A Sudoku board \(partially filled\) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits
  `1-9`
  and the character
  `'.'`
  .
* The given board size is always
  `9x9`

[https://leetcode.com/problems/valid-sudoku/description/](https://leetcode.com/problems/valid-sudoku/description/)

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        bool used1[9][9] = {false};
        bool used2[9][9] = {false};
        bool used3[3][3][9] = {false};
        for (int i = 0; i < 9; i++)
        {
            for (int j = 0; j<9; j++)
            {
                if (board[i][j] == '.')
                    continue;
                int v = board[i][j] - '0'-1;
                int ii = i/3;
                int jj = j/3;

                if (used1[i][v] || used2[v][j] || used3[ii][jj][v])
                    return false;
                used1[i][v] = used2[v][j] = used3[ii][jj][v] = true;
            }
        }
        return true;
    }
};
```



## 50. Pow\(x, n\)

---

Implement[pow\(_x_,_n_\)](http://www.cplusplus.com/reference/valarray/pow/), which calculates _x_raised to the power_n_\(xn\).

**Example 1:**

```
Input:
 2.00000, 10

Output:
 1024.00000

```

**Example 2:**

```
Input:
 2.10000, 3

Output:
 9.26100

```

**Example 3:**

```
Input:
 2.00000, -2

Output:
 0.25000

Explanation:
 2
-2
 = 1/2
2
 = 1/4 = 0.25

```

**Note:**

* -100.0 
  &lt;
  _x_
  &lt;
   100.0
* _n_
  is a 32-bit signed integer, within the range \[−2
  31
  , 2
  31 
  − 1\]

https://leetcode.com/problems/powx-n/description/  


```cpp
class Solution {
public:
    double myPow(double x, int n) {
        return myPowHelper(x, n);
    }
    
    double myPowHelper(double x, long n)
    {
        if (n < 0)
            return 1./myPowHelper(x, -n);
        if (n == 0)
            return 1;
        if (n == 1)
            return x;
        double v = myPowHelper(x, n/2);
        if (n%2)
            return v*v*x;
        return v*v;  
    }
};
```




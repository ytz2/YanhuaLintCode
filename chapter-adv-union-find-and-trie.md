## 323. Number of Connected Components in an Undirected Graph

Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there iGiven`n`nodes labeled from`0`to`n - 1`and a list of undirected edges \(each edge is a pair of nodes\), write a function to find the number of connected components in an undirected graph.

**Example 1:**

```
Input: 
n = 5
 and 
edges = [[0, 1], [1, 2], [3, 4]]


     0          3
     |          |
     1 --- 2    4 


Output: 
2
```

**Example 2:**

```
Input: 
n = 5
 and 
edges = [[0, 1], [1, 2], [2, 3], [3, 4]]


     0           4
     |           |
     1 --- 2 --- 3


Output:  
1
```

**Note:**  
You can assume that no duplicate edges will appear in`edges`. Since all edges are undirected,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in`edges`.

看到这种图联通不连通就想到UF了， 这里是UF的一个变种，找有几个subset.维护一个n就可以了，最开始为n个，每连一个就--



另外还有一个考察的地方可能是query每个subset的size,其实就是维护一个size\[n\] 每次connect的时候

father\[i\] = j

size\[j\] += size\[i\]就可以了

这样query的时候找到所有father是自己的，然后去query他的size就是subset size

```cpp
class Solution {
public:

    int countComponents(int n, vector<pair<int, int>>& edges) {
        father_ = vector<int>(n, 0);
        size = n;
        for (int i = 0; i < n; i++)
            father_[i] = i;

        for (auto& p : edges)
            connect(p.first, p.second);
        return size;

    }

    void connect(int i, int j)
    {
        int ri = find(i);
        int rj = find(j);
        if (ri != rj)
        {
            father_[ri] = rj;
            size--;
        }
    }

    int find(int v)
    {
        if (father_[v] != v)
            father_[v] = find(father_[v]);
        return father_[v];
    }

    int size;
    vector<int> father_;
};
```




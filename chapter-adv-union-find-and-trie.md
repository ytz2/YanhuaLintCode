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

另外还有一个考察的地方可能是query每个subset的size,其实就是维护一个size\[n\] 每次connect的时候。 应该说这道题的变种是数岛II。

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

## 684. Redundant Connection

In this problem, a tree is an**undirected**graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes \(with distinct values 1, 2, ..., N\), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of`edges`. Each element of`edges`is a pair`[u, v]`with`u < v`, that represents an**undirected**edge connecting nodes`u`and`v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge`[u, v]`should be in the same format, with`u < v`.

**Example 1:**

```
Input:
 [[1,2], [1,3], [2,3]]

Output:
 [2,3]

Explanation:
 The given undirected graph will be like this:
  1
 / \
2 - 3
```

**Example 2:**

```
Input:
 [[1,2], [2,3], [3,4], [1,4], [1,5]]

Output:
 [1,4]

Explanation:
 The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

**Note:**

The size of the input 2D-array will be between 3 and 1000.

Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

**Update \(2017-09-26\):**  
We have overhauled the problem description + test cases and specified clearly the graph is an**undirected**graph. For the**directed**graph follow up please see[**Redundant Connection II**](https://leetcode.com/problems/redundant-connection-ii/description/)\). We apologize for any inconvenience caused.

这道题的类比是valid graph tree.

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        vector<int> father(edges.size()+1, 0);
        for (int i = 1; i <= edges.size(); i++)
            father[i] = i;

        for (auto& edge : edges)
            if (!Union(father, edge[0], edge[1]))
                return edge;
        return {};
    }

    bool Union(vector<int>& father, int i, int j)
    {
        int ri = Find(father, i);
        int rj = Find(father, j);
        if (ri == rj)
            return false;
        father[ri] = rj;
        return true;
    }

    int Find(vector<int>& father, int i)
    {
        if (father[i] != i)
            father[i] = Find(father, father[i]);
        return father[i];
    }
};
```



## 685. Redundant Connection II

In this problem, a rooted tree is a**directed**graph such that, there is exactly one node \(the root\) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with N nodes \(with distinct values 1, 2, ..., N\), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of`edges`. Each element of`edges`is a pair`[u, v]`that represents a**directed**edge connecting nodes`u`and`v`, where`u`is a parent of child`v`.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

**Example 1:**  


```
Input:
 [[1,2], [1,3], [2,3]]

Output:
 [2,3]

Explanation:
 The given directed graph will be like this:
  1
 / \
v   v
2-->3

```



**Note:**  


The size of the input 2D-array will be between 3 and 1000.

Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.



有向图和无向图不一样，有向图还要找到变成树的结构有两种情况



第一种情况： 无环： 退化成无向图UF查找， 比如例子

第二种情况： 有环： 一个节点有两个parent,那么找出构造这两个parent的边作为candidate, 删掉其中一条， UF查找，如果还是有环，那么肯定就是没删掉的另外一条边， 或者UF找到的边。 如果没环了，说明就是被删掉那条边了

  


```cpp



class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        
        int n = edges.size();
        vector<int> father(n+1, 0);
        vector<int> parent(n+1, 0);
        vector<int> ans1, ans2;
        
        
        for (int i = 0; i <=n; i++)
            father[i] = i;
        
        // check how many parents one node have 
        for (auto& edge : edges)
        {
            int u = edge[0];
            int v = edge[1];
            // u->v, v's parent is u
            
            // ok, we have more than one parent
            if (parent[v] > 0)
            {
                // do below:
                // 1. record the duplicate edge
                ans2 = edge;
                ans1 = {parent[v] ,v};
                // delete the edge
                edge[0] = -1;
                edge[1] = -1;
            }
            parent[v] = u;
        }
        
        for (auto& edge : edges)
        {
            if (edge[0] == -1)
                continue;
            if (!Union(father, edge[0], edge[1]))
                return ans1.empty()? edge : ans1;
        }
        

        return ans2;
    }
    
    int Find(vector<int>& father, int i){
      if (father[i] != i)
          father[i] = Find(father, father[i]);
      return father[i];
    };
    
    bool Union(vector<int>& father, int i, int j){
        int root_i = Find(father,i);
        int root_j = Find(father,j);
        if (root_i == root_j)
            return false;
        father[root_i] = root_j;
        return true;
    };
    
};
```




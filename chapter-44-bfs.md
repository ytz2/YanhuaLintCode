# BFS

## Scenario:



#### 1. 层级遍历

   Binary Tree Level Order Traversal,  Serialize and Deserialize Binary Tree, Convert Binary Tree to LinkedLists by Depth, Binary ZigZag Traversal, Binary Tree Level Order Traversal II. 



基本上固定的BFS套路

q.push\(root\)

while\(!q.empty\(\)\)

{

      int n = q.size\(\);

     for i in \[0:n\] 

             a = q.pop\(\)

             handle

             next level check

}



#### 2. 点到面

或者说应该是典型的flood fill问题。

Number of IsLands， Clone Graph, zoombie in matrix, Connected Nodes in Graph, Building Post Office II, Smallest Rectangle Enclosing Black Pixels

应该算是图的遍历， 基本的模式是给一个N维矩阵， 或者给一个图，要求遍历中couting

这个算unleveled traversal, 所以基本套路也是可以寻找的：

遍历过程：



q.push\(start\)

isVisited-&gt; mark start

while \(!q.empty\(\)\)

   v= q.pop\(\);

   for each neighbor of v:

         check to process:

              q.push\(each\)

              isVisited-&gt; mark each



基本有两个技巧吧：

1.图用一个set来存visited, 矩阵的话一般都是0， 1， 2直接在矩阵上mark,就省了

2.点到面的问题容易犯错的是漏掉起始点， 比如 Connected Nodes in Graph 



#### 3 . 拓扑排序

Course Schedule , Course Schedule 2, Valid Tree,  Sequence Reconstruction

拓扑排序问题的固定套路：

0： optional , 从边的关系转化到点的关系

unordered_map&lt;Node\*, unordered_set&lt;Node\*&gt;&gt; graph 

1:  建立入度

for each in graph

      for neighbor in each.neighbor

            indegrees\[neighbor\]++

2. 确定入度为0

for each in candidates:

          cannot find in degrees

              q.push\(each\)

              visited\(each\)

3. BFS

while\(!q.empty\(\)\)

         node = q.front\(\)

        handle \(node\)

         q.pop\(\)

        for each in node.neighbors:

             if \(each in indegrees and --indegrees\[node\] == 0\)

                       indegrees erase node, 

                       visited \(node\)

                       q.push\(neighbor\)

   

Valid Tree记住一个结论： 充分必要条件 n个点对应n-1条边且无环。 证明无环是一个拓扑排序的过程（也可以用并查集）

                

#### 4 最短路径

Knight Shortest Path, Word Ladder , Remove Substrings, degrees, Sliding Puzzle II, 

这种一般问的是最短多少步可以搞定，算是图中点到点的问题， 解决这类问题要考虑几个要素：

1. transition matrix 就是怎么往neighbor走 ,因为题目的定义通常不直接
2. visited 这个是否可以hash,如果不能hash, work around \(to string?, custom hash？ many solutions\)

具体套路还是固定的， 是属于层级遍历，然后返回多少层的时候搞定

level = -1

q.push\(start\)

visited\(start\)

while\(!q.empty\(\)\)

      level++;

       auto n = q.size\(\)

       for i in 0:n:

            node = q.pop\(\)

            handle\(node\) // return

            for each in node's neighbor:

                    check to continue:

                              q.push\(each\)

                              visited\(each\)



word ladder学会了一个技巧， 如果一次只编辑一个字母的话， 遍历26个字母比遍历字典强（depends\)        

```
                for (int k = 0; k<start.size(); k++)
                {
                    char orig = node[k];
                    for (char c='a'; c<='z';c++)
                    {
                        node[k] = c;
                        if (visited.find(node) == visited.end() && dict.find(node) != visited.end())
                        {
                            visited.insert(node);
                            q.push(node);
                        }
                    }
                    node[k] = orig;
                }
```



Remove substrings 别掉坑里。。。当string remove substring后，还会有同样的。。。 aaaaabbbbb remove ab.... aaabbb，其实应该一直remove下去

Sliding Puzzle 是一个典型的hash蛋疼，二维数组变一维string来偷懒hash的例子。



#### 5 并查集

```
class UnionFind{
public:
    UnionFind(int n)
    {
        parent_ = vector<int>(n, 0);
        rank_ = vector<int>(n, 0);
        for (int i = 0; i < n; i++)
            parent_[i] = i;
    }
    
    int Find(int x)
    {
        if (parent_[x] != x)
            parent_[x] = Find(parent_[x]);
        return parent_[x];
    }
    
    bool Union(int x, int y)
    {
        int rx = Find(x);
        int ry = Find(y);
        if (rx == ry)
            return false;
        int rankx = rank_[rx];
        int ranky = rank_[ry];
        if (rankx < ranky)
        {
            parent_[rx] = ry;
        }
        else if (rankx > ranky)
        {
            parent_[ry] = rx;
        }
        else
        {
            parent_[rx] = ry;
            rank_[ry]++;
        }
        return true;
    }
private:
    vector<int> parent_;
    vector<int> rank_;
};

```

并查集解决一个 subset merging和检查两个元素是否属于同一set的问题， amortized O（1）

这个小众数据结构对于解决图里有环的问题是一方良药

Number of Islands II, valid tree

并查集的两个优化： 1 compression via search 2. lower rank merge to higher rank during union 




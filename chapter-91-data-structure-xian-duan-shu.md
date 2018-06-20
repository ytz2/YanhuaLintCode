# Segment Tree

## 线段树的表达

```
            [0,3]
           (val=4)
         /         \
     [0,1]         [2,3]
    (val=4)       (val=3)
    /    \         /    \
 [0,0]  [1,1]   [2,2]  [3,3]
(val=1)(val=4) (val=2)(val=3)
```

\[1, 4, 2, 3\]

第一层： \[0, 3\]区间， 最大4

第二层： \[0, 1\]区间， 最大4

```
            \[2, 3\]区间， 最大3
```

第三层： \[0, 0\]区间， 最大1

```
            [1, 1\]区间， 最大4

           \[2,  2\]区间， 最大2

           \[3,  3\]区间， 最大3
```

如上：

线段树是把一个array表达成以\[ind0, ind1\]为线段的树结构

所以定义SegmentTreeNode如下

```cpp
struct SegmentTreeNode{

    SegmentTreeNode(int s, int e, int v)
        : start(s), end(e), val(v), left(nullptr), right(nullptr)
    {
    }
    ~SegmentTreeNode()
    {
        if (left)
            delete left;
        if (right)
            delete right;
    }
    int start;
    int end;
    int val;
    SegmentTreeNode* left;
    SegmentTreeNode* right;

};
```

## 线段树的构建

根据线段树的定义， 树的构建是beg, end, arr，左右切一下，一直切到一个一个的元素为止



```cpp
    void build(const std::vector<int>& arr)
    {
        root = buildHelper(0, arr.size()-1, arr);
    }

    SegmentTreeNode* buildHelper(int beg, int end, const std::vector<int>& arr)
    {
        if (beg > end)
            return nullptr;
        SegmentTreeNode* node = new SegmentTreeNode(beg, end, arr[beg]);
        if (beg == end)
            return node;
        int mid = ( beg + end ) / 2;
        node->left = buildHelper(beg, mid, arr);
        node->right = buildHelper(mid+1, end, arr);
        node->val = std::max(node->left?node->left->val:INT_MIN, node->right? node->right->val : INT_MIN);
        return node;
    }
```

## 线段树的查询

![](/assets/segtree.png)

第一层会查询试图查询\[1, 7\], 发现区间不存在，然后根据mid位置拆分\[1, 4\]和\[5, 7\]  
第二层会查询\[1, 4\],\[5, 7\], 发现\[1, 4\]已经存在，返回即可，\[5, 7\]仍旧需要继续拆分  
第三层会查询\[5, 6\],\[7, 7\], 发现\[5, 6\]已经存在，返回即可，\[7, 7\]仍旧需要继续拆分  
第四层会查询\[7, 7\]

任意长度的线段，最多被拆分成logn条线段树上存在的线段，所以查询的时间复杂度为O\(log\(n\)\)

```cpp
    int query(int beg, int end)
    {
        return queryHelper(root, beg, end);
    }
    
        
    int queryHelper(SegmentTreeNode* node, int start, int end)
    {
        if (!node)
            return INT_MIN;
            
        if (start <= node->start && node->end <= end) //如果全部在范围之内， 则取root
            return node->val;
        int mid = (node->start + node->end) /2; // 找到两个儿子的分界点
        int ans = INT_MIN;
        if (node->left && mid >= start) //左儿子的查找条件， 和左儿子有交集
            ans = std::max(ans, queryHelper(node->left, start, end));
        if (node->right && mid + 1 <= end)//和右儿子有交集
            ans = std::max(ans, queryHelper(node->right, start, end));
        return ans;
    }
    
```




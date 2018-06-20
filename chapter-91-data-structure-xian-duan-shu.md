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

```
struct SegmentTreeNode{
```

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




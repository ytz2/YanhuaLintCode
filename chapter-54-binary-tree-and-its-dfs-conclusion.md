## Binary Tree and Its DFS



## Binary Search Tree

1。基本的东西

关键还是了解：左边小于root,右边不小于root. 几乎所有的题目都是从这个性质上展开的。 

我觉得最典型的例子就是Closest Binary Search Tree Value。 算是把BST的这个性质，和C++ 11 map里的

upper_bound , lower\_bound_

这两个API也解释清楚了。 

```cpp
    TreeNode* lower_bound(TreeNode* root, double target)
    {
        if (!root)
            return root;
        if (root->val > target)
            return lower_bound(root->left, target);
        // root->val <= target
        auto node = lower_bound(root->right, target);
        if (node)
            return node;
        return root;
    }
    
    TreeNode* upper_bound(TreeNode* root, double target)
    {
        if (!root)
            return nullptr;
        if (root->val <= target)
            return upper_bound(root->right, target);
        auto node = upper_bound(root->left, target);
        if (node)
            return node;
        return root;
    }
```



拿 lower bound举例，c++的 lower bound 返回的是第一个小于等于的数所以当root大于的val， 没有选择了，只能往左走，因为左边的数都小。 如果root小于等于val,可以往root的右面走试试运气，如果没有，就是root. 

upper bound vice versa.  个人觉得这个思路很有趣，且值得再拿出来看下。



2. 一些例子

closest binary tree value : upper/ lower bound 比较一下

\*\*\*\* kth smallest element in BST : in order 遍历，计数到k 所以o\(h+k\). 这道题我试着用quick select的办法做了一遍，其实更好玩， 思路是in order遍历， 数左边个数， 如果左边大于k,则继续在左边找， 如果是k-1那么root是要找的，否则去右边找 k-left-1。 是一个一边inorder, 一边数数， 一边二分的过程。 

validate bst: 就是我们上面提到的基本性质，首先局部满足，其次整体满足，局部满足知道是root-&gt;left &lt; root &lt; =root-right,整体满足是max\(left\) &lt; root &lt; min\( right\) 。分治的思路就够了。



\*\*\* closest binary search tree value II, 这题让求最近的k个。  这种k个问题，一般是需要无脑维护一个priority queue， 不到k个的时候往里插，到了k个就往外丢或者不丢。 一般复杂度不是最优，但是解题算够了。 但是把这道题标星号是因为我做这道题的时候解决了一个长期都不会的问题。。。怎给comparator额外的参数，然后传给pq， 办法就是正常些operator,然后把cmp\(param\)作为参数传给pq constructor

```
struct cmp{
    
    cmp(double t = 0)
        :target(t)
        {}
    double target;
    bool operator()(TreeNode* left, TreeNode* right)
    {
        return abs(left->val - target) < abs(right->val-target);
    }
};
typedef priority_queue<TreeNode*, vector<TreeNode*>, cmp> PQ;
pq = PQ(cmp(target));
```



BST Iterator: 妈的，熟读并背诵。。。。无聊。 维护一个stack, constructor,就是一路向左知道最左边， next的时候 top是cur,如果cur还有右边，向右拐， 一路向左往stack上丢。要么没有右边，就一路pop一直回到左拐点， //  while\(!stk.empty\(\) && stk.top\(\)-&gt;right == node\)// 

Inorder Predecessor/succesor in BST:  这个是lower bound /upper bound的问题，不过要考虑有重复元素的问题， 这个好解决因为二分搜索里的重复问题本来就是恶心的事情












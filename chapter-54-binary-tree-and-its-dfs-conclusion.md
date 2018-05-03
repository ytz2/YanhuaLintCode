## Binary Tree and Its DFS

## Binary Search Tree

### 1。基本的东西

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

upper bound vice versa.  个人觉得这个思路很有趣，且值得再拿出来看下

### 2 一些例子

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



## 二叉树上求值

这个算是有固定套路的东西了



#### 子树求值问题

最大最小子树（不是路径） ， 打一个擂台。 左边和，右边和，中间， 维护一个最大/最小值，做后序遍历（divide conquer\)一般都是可以解决的

Minimum subtree, subtree with maximum avg \(除了求和还要数数， 思路一样）



#### 路径求值问题



路径问题一般都是前序遍历+回溯 

最基本的一个问题，Binary Tree Paths 打印所有从root出发的路径， 就是前序 push root, left, right, pop root  从理解了这个思路的基础上，一般路径问题都是可以解决的

Binary Tree Sum III 这道题不要求从root出发，但是要求从上到下， 找出所有的sum， 所以维护一个vector,每push一次，就把所有以root为起点的从后往前找，放进去就好了 过程还是前序+ 回溯

Binary Path Sum, 比上题简单， 这道题求得是组大路径和，也不要求起点终点 ， 但是这个 不是上一个思路，而是用D&Q， 左右路径和，选个大的加上当前值返回回去，同时打一个擂台就好了

Binary Tree Path Sum ， 这道题也是异类， 属于放弃治疗类型的。直接就是暴力了。 要求求任start end的和为target,因为放弃治疗了，所以直接遍历每一个节点，然后从当前节点开始往外DFS搜索， 两重DFS， 往外DFS 有三个方向， left, right, parent 然后pop完成回溯。 因为是个树，所以还要放个parent不让他往回走。。。是道奇葩题

Binary tree max path sum ii: 这是从root开始，任意截止的。 模板可以解决。 前序回溯



## 二叉树结构



比较杂乱，直接列题目了

flatten b tree to linked list with pre order, 思路， flat left, return the last node, flat right, return the last node  把 root next 指向 left last 指向right first 返回right last. 中间有些小corner, 左边没有咋办，右边没有咋办等等

blanced binary tree D&Q， 判断依据: local到整体的思路， 本地是balanced,然后整体左深度和右深度也是平衡的

min depth of b tree , max depth of btree min这个有个坑，就是写顺手min\(left, right\)直接上了，实际上root-&gt;left没得话，他的深度是从右边来的，否则上去就是0了





 












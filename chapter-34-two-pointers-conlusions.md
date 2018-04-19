# 小结



## 相向双指针

解决以下几个问题：

各类sum, 各类minus的问题， 经典的题目就是two sum， 基本套路就是排序后，如果大了把大指针变小，小了把小指针变大

two sum data structure design挺有意思的， 要求一边插入一边搜索， multiset大法好，当然hash map大法也挺好

3sum, 4sum， 2 unique pairs 基本考察的是复杂coding的能力+去重的办法 if \(i != 0 && nums\[i-1\] == nums\[i\]\) continue

2sum less/equal ， 要转弯的地方是 当满足beg, end pair后，算一下长度，然后继续打破平衡继续搜。同样的思想：closest to zero,  2sum greater than target,  triangle count

双向指针总的来说简单

## 同向双指针

主要在去重和move elem里面，算是一个满一个快，慢的作为标记，快的作为扫描指针，当快的扫到条件满足移动的时候，把他移动到标记指针地方去，同时标记指针前移

Remove duplicate numbers in array, move zeros

linked list median. cycle, cycle point 

## 排序及其延伸

两个经典的sorting, merge sorting, 先局部再整体 （sort left sort right merge left right\). 还有一个就是quick sort, 先整体再局部，先 parition,再sort 左右。 这个里面有个模板就是无脑i&lt;=j and nums\[mid\] &lt; \| &gt; 第一个Index带等号为了让最后i,j交错。 第二个不带等号是为了让相等元素来回shoot，这样维持平衡。由quickSort衍生出一个quick select， 这个是用O\(N\)的办法把问题减少到T（N/2）最后复杂度是n/2. 思路是先partion, 然后计算k在左面还是右面，然后更新k并继续递归。

kth largest/ kth smallest, sort colors, partion three groups \(这个题要维护三根指针，两根边界指针，一根扫描指针\)

 

## Top K 问题 / 滑动窗口

滑动求平均非常简单，算一次，然后加下一个减去上一个。

滑动求median就恶心了。  两种办法， 插入排序， 先sort,然后加入新的元素：新加的元素要插入到第一个比他大的位置upper\_bound,然后再把之前的元素erase掉，求中位数。  第二种办法，是维护一个排序数组， multiset大法好！！！ 但是需要枚举： 左变删除比median大还是小需要移动， 右边插入比median 大还是小，median是否需要移动。 结果是 左边小的时候 左移，右边插入小的时候右移动。因为multiset插入总是在已有的后面，所以带一个等号



sliding window maximum, 用一个multiset维护最大，然后插入的时候log\(n\)， 删除的时候log\(n\)

data stream median, 维护一个multiset,后者用two queue来模拟一个multiset中，l min que top &lt; r ax queu top 且l +1 == r or l==r，这样l queue 顶部为中位数。 如果是multiset,还是用哪个老办法，维护一个中间指针，但是要考虑奇数偶数

top k in stream, 维护一个q就好了，保证q的大小为k， 如果超了就和顶部最小的比，小了就不管，大了删掉插进去。 最大k用顶部最小ksize pq

神题是top k frequent， 这个题目要一边计数一遍维护一个frequency 最小pq， 但是pq操作是单向的，所以用一个set,因为set 是 binary tree, set size为k, 每次 来的时候先删掉原来的数， 然后更新计数器，再插入，把最不频繁的删掉。 之所以这样做是因为为了利用set的排序功能， 我们自定义了一个comparator ... 插入，搜索，删除的时候计数器不能变， 注意下面的比较， 如果freq相等，则用string作比较，这是为了让set能够查找。 



```
    struct MyCompare
    {
        bool operator() (const string& left, const string& right)
        {
            int leftn = counter_[left];
            int rightn = counter_[right];
            if (leftn != rightn)
                return leftn > rightn;
            return left < right;
        }
    };
    set<string, MyCompare> recorder_;
```

 




















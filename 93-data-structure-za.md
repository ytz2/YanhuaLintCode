## 839. Merge Two Sorted Interval Lists

Merge two sorted \(ascending\) lists of interval and return it as a new sorted list. The new sorted list should be made by splicing together the intervals of the two lists and sorted in ascending order.

### Example

Given list1 =`[(1,2),(3,4)]`and list2 =`[(2,3),(5,6)]`, return`[(1,4),(5,6)]`.

https://www.lintcode.com/problem/merge-two-sorted-interval-lists/description

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
/**
 * Definition of Interval:
 * classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param list1: one of the given list
     * @param list2: another list
     * @return: the new sorted list of interval
     */
    vector<Interval> mergeTwoInterval(vector<Interval> &list1, vector<Interval> &list2) {
        // write your code here
        vector<Interval> res;
        if (list1.empty() && list2.empty())
            return res;
        if (list2.size() < list1.size())
            swap(list1, list2);
        auto sortFunc = [](const Interval& l, const Interval& r){
            return l.start < r.start;
        };
        for (auto& elem : list2)
            list1.push_back(elem);
        sort(list1.begin(), list1.end(), sortFunc);

        auto prev = Interval(list1[0].start, list1[0].start);
        for (int i = 0; i < list1.size(); i++)
        {
            auto& node = list1[i];
            if (prev.end >= node.start)
            {
                if (node.end > prev.end)
                    prev.end = node.end;
            }
            else
            {
                res.push_back(prev);
                prev = node;
            }
        }
        res.push_back(prev);
        return res;
    }
};
```




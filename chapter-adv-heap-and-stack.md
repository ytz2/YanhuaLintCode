## **Heap**

## 42. Trapping Rain Water

Given_n_non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![](/assets/TrapWater1.png)  
The above elevation map is represented by array \[0,1,0,2,1,0,1,3,2,1,2,1\]. In this case, 6 units of rain water \(blue section\) are being trapped.**Thanks Marcos**for contributing this image!

**Example:**

```
Input:
 [0,1,0,2,1,0,1,3,2,1,2,1]

Output:
 6
```

---

trap water 这种基本上是两根指针

有一道类似的题，给一堆柱子，问选左右哪个最大，哪个也是两根指针 移动最矮的。



这道题的分析如下：

左右两根指针， 选最小的，代表水位大于等于最小的， 继续移动，一直到下一个比小的要大，这个时候再比较，一直到相遇。 

用的原理是最小的板子，如果近邻比他矮，那么可以存最小的高度，比他高则这一段终止，继续下一段。这个还是要画画才能看出来。



---

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.size() <=2)
            return 0;
        int l = 0, r = height.size()-1;
        int sum = 0;
        while(l <= r)
        {
            if (height[l] <= height[r])
            {
                int bar = height[l];
                while (l<=r && height[l] <= bar)
                {
                    sum +=bar - height[l];
                    l++;
                }
            }
            else
            {
                int bar = height[r];
                while(l<=r && height[r] <= bar)
                {
                    sum +=bar - height[r];
                    r--;
                }
            }
        }
        return sum;
    }
};
```

## 




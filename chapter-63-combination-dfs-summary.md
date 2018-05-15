Combination DFS要解决的问题是

对于某一个中间步骤的选择与否

来确定： a\) 是否可行 b\)解的个数 c\) 所有解

其中a\) 可以用BFS来代替，而且prefer

       b\) 一般会有相应的DP解， 并且对应着memorized search的解法

       c\) 这个对应着DFS的办法



其中对于中间步骤的处理有如下：

1. 枚举
2. 遍历
3. 在1&2过程中剪枝 + 记忆化搜索





1 枚举 --- 求所有的解

680 split string , 只需要去1-2位数字，枚举即可 

426 restore IP, 同样的是取舍与否， 固定3位数字以内 则为valid

427 Generate Parentheses 有个小trick， 左边大于等于右边且最后左边等于右边， 这个是剪枝的条件， 只需要push pop 左边和push pop 右边即可

 

2 遍历 -- 求所有的解

Palindrome Partitioning , combination sum II, subset II, subset,  factorization , find missing II, word break



3 记忆化搜索 求所有解的partition的经典题，wordbreak ii,的剪枝方法， 在i位置break, 如果解增加了则cache此处可以break 下次访问的时候没必要向下搜索了。

word break III , Palindrom Paritioning II


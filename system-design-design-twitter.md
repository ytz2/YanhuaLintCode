

# **Scenario **

DAU : 100 M

| Use Case | QPS | Storage \(per day\) |
| :--- | :--- | :--- |
| User Registration/Login/Edit | 0.1 \* 100 M / 100k = 100 , peak = 300 | 1G/day \(over estimate\) |
| User Profile display & query |  |  |
| Tweet post | 1k , peak = 3k | 1k per post, 10 G / day |
| News Feed | 20k peak = 100k |  |
| Media upload | 100 , peak 300 | 10 M \* 100 \* 100k = 100 T - 1PB |
| Search | NA |  |
| Follow/Unfollow  | 100, 300 |  |



# Service



![](/assets/tweet.png) 

# Storage

![](/assets/tweet storage.png)

pull model:

![](/assets/pull model.png)

![](/assets/pullexp.png)



Push Model

![](/assets/pushModel.png)

![](/assets/psuh exp.png)



# Scale

handle like/dislike

![](/assets/likeunlike.png)

缓存穿透

如果遭受恶意攻击，数据出问题，导致大量的请求直接hit在db上怎么办？

![](/assets/cachethrough problem1.png)



![](/assets/cacheThroughIssue.png)

![](/assets/conc.png)



过热怎么办？

只让一个来弄

![](/assets/hotkey.png)



或者永不过期，但异步逻辑存在过期，过段时间去更新他

![](/assets/writeback.png)


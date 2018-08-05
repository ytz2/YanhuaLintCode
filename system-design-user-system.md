# Scenario

DAU 100M

| Scenario | QPS | STORAGE |
| :--- | :--- | :--- |
| Register/Login/Modify | 100 - 300 \(0.1 \* 100M/100k\) |  |
| Query | 100k - 300k  \(100 \* 100/0.1\) |  |

# Service & Storage

# ![](/assets/User.png)**Scale: Sharding & Replica**

![](/assets/sharding&replica.png)另外，贴一些前辈们使用redis的经验和教训：

1. 要进行master-slave配置，出现服务故障时可以支持切换。
2. 在master侧禁用数据持久化，只需在slave上配置数据持久化。
3. 物理内存+虚拟内存不足，这个时候dump一直死着，时间久了机器挂掉。这个情况就是灾难。
4. **当redis物理内存使用超过内存总容量的3/5时就会开始比较危险了，就开始做swap,内存碎片大。**
5. **当达到最大内存时，会清空带有过期时间的key，即使key未到过期时间**。 

## Write-behind {#write-behind}

The write-behind pattern changes the timing of the write to the system-of-record. Rather than writing to the system-of-record while the thread making the update waits \(as with write-through\), write-behind queues the data for writing at a later time. This allows the user’s thread to move along more quickly, at the cost of introducing some lag in time before the SoR is updated.

这个是我们处理热键把他设置为永不过期，但是异步的来改他的值得办法。 


# GFS



### Scenario

读写PB量级的文件系统， 支持modify, delete etc



# Service

DFS

# Storage 

FileSystem



首先单机存100G 怎么存？

![](/assets/LinuxFS.png)

![](/assets/LINUXFS.png)

多机怎么办？：

master-slave \(master is down, bad, but can compensate with shadow master\) ? P2P? \(disadvantage, sync is issue\), go with master slave



![](/assets/GFS.png)










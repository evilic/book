具体数学读书笔记与习题整理
==========================

# 目录

# [第1章 递归] (https://github.com/JiYou/book/tree/master/concrete_mathematics)
###[1.1  汉诺塔] (https://github.com/JiYou/book/tree/master/concrete_mathematics/chap01/section01)
###[1.2  平面切分] (https://github.com/JiYou/book/tree/master/concrete_mathematics/chap01/section02)
###[1.3  约瑟夫问题] (https://github.com/JiYou/book/tree/master/concrete_mathematics/chap01/section03)
###[1.4  习题] (https://github.com/JiYou/book/tree/master/concrete_mathematics/chap01/section04)

# [第2章 递归] (https://github.com/JiYou/book/tree/master/ceph_source_code_analysis)
# [第3章 递归] (https://github.com/JiYou/book/tree/master/ceph_source_code_analysis)
# [第4章 递归] (https://github.com/JiYou/book/tree/master/ceph_source_code_analysis)
# [第5章 递归] (https://github.com/JiYou/book/tree/master/ceph_source_code_analysis)
# [第6章 递归] (https://github.com/JiYou/book/tree/master/ceph_source_code_analysis)






Ceph将对象存储、块存储、文件存储统一到一个存储系统中。
Ceph的目标是实现一个高可靠、易管理、免费的存储系统。
借助于Ceph的力量，可以轻易地转变公司IT部门的结构，还可以管理海量数据。
除此之外，Ceph还拥有超强的扩展能力，能够支持成千上万的客户端。
Ceph的数据容量已持续爆炸性增长到拍字节（Petabytes）和艾字节(Exabytes）。

## 三个重要组成部分

- Ceph Node：构成存储所必须的硬件及相应的守护进程。
- Ceph Storage Cluster：由一系列的Ceph Node组成，位于Cluster中的Ceph Node会相互通信以保持数据的备份及动态地分发。
- Ceph Monitor：Ceph Monitor也是由一系列节点组成，只不过其功能主要是用于监控位于Ceph Storage Cluster中的Ceph Node。可能会问，位于Ceph Monitor中的节点是否也是要监控呢？答案是Ceph Monitor Cluster内部已经采用了高可用的机制。

## Ceph功能结构图

按照Ceph的目标，其功能主要是分为三方面：
- 对象存储：（reliable, autonomic, distributed object store gateway, RADOSGW）一个基于桶的REST端口，能够兼容S3（亚马逊提供的对象存储接口）、及Swift接口（OpenStack提供的对象存储接口）。
- 块存储：（reliable block device, RBD）一个可靠、完全分布式的块设备。并且提供了Linux Kernel Client，以及QEMU/KVM相应的驱动。
- Ceph FS：（Ceph file system, CEPHFS）兼容POSIX的分布式文件系统。给Linux Kernel提供了客户端工具，并且同时支持[FUSE](http://fuse.sourceforge.net/)。

Ceph的功能结构如图1.1所示。
除了Ceph的三个目标之外，此外还有一个功能是直接提供给应用程序。
只不过此时实际功能直接是由底层的RADOS提供，其接口命名为LIBRADOS。

*注意：FUSE是指Filesystem in Userspace。*

![Ceph系统功能结构图](./images/architecture.png "Ceph存储系统功能结构图")

图1.1  Ceph系统功能结构图

从图1.1中，可以看出Ceph主要是基于[RADOS](./pdfs/weil-rados-pdsw07.pdf)文件系统实现了其功能。

*注意：RADOS可以参考论文《[RADOS: A Scalable, Reliable Storage Service for Petabyte-scale
Storage Clusters](./pdfs/weil-rados-pdsw07.pdf)》*

需要注意的是两方面：
- 未移动的PG是占了大多数的，也就是说，大部分数据并未发生迁移。
- 移走的PG对于旧有的OSD而言，扩大了旧有的OSD的空间。
- 当rebanlance完成之后，访问数据也不会发生负载尖刺。

![负载均衡](./images/rebanlance.png "负载均衡")

图1.6 负载均衡

## 数据持久性







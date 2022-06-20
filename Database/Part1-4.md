## Disk

#### 数据库基础知识复习

Relational model 包括 Entities,relationships,Attritubes

Database table：

- Rows： 代表每一个records
  - 每一个record都是unique的
  - represent as Tuples
- Column：代表Attritubes
  - have unique name
  - a Domain

A table is an instance of a schema（架构）

Schema就是CREATE TABLE这些，其中包含primary key etc

### Memory

![1655715633020](image/Part1-4/1655715633020.png)

Persistent storage 永久存储

虽然数据需要永久存储，但是所有的操作都在memory中进行

##### Secondary Memory

- Hard disk drives HDDS
  - 慢，便宜，相对不可靠
- Solid state drives SSDs
  - 相比较于硬盘更快更贵

##### Tertiary Memory

脱机储存，大容量低成本比如cd tape

##### Persistent memory

持久内存是非易失性RAM（随机存储其器，直接与CPU交换数据），也称为

- NVM–非易失性存储器
- NVRAM–非易失性RAM
- SCM–存储类内存

特点

- 字节可寻址
- 持久的

类型

- NVDIMM-N

  - DRAM与带电池的闪存配对
  - 与DRAM性能相似
  - 容量小且相对昂贵
- NVDIMM-F

  - 使用DRAM总线的闪存存储
  - 比DRAM慢，更接近闪存性能
  - 容量大，价格便宜

### HDDs

![1655717168386](image/Part1-4/1655717168386.png)

以bit单位储存，分组为byte

一些名词：

surface：磁盘的面，通常一个磁盘有两个面，两个都可以储存数据

Platters：磁盘本人，一个硬盘四个

Track：轨道，可以根据角度分成sectors

Cylinder：所有surface上同一直径上的track的轨道集，通常在追踪track的时候也用cylinder表示。 

disk head：磁盘读取头，每一面都有一个head。移动单位为disk head array。

###### Reading

Queryc处理器request a record- 由buffer mananger 接手request-在读取的时候 整个block都会被读取到main  memory

在磁盘中read& write的单位是BLOCK -  a block contains multiple records

###### Writing

先读后写-可以以单个record的方式写入

###### Accessing Data

* 想要找到access 的data，磁盘需要旋转到磁盘头可读的位置，找寻block所在的track的时间为SEEK TIME
  * min seek time =0
  * average seek time=1/3 max seek time
  * unit as ms
* 等待block的边缘旋转到磁头的时间为Rotational Delay
  * Average Rotational Delay=1/2 Max Rotational Delay
  * Max Rotational Delay = 1/(#of RMP/60s per min)* 1000
  * unit as ms
* 阅读整个block的时间为 Transfer Time
  * Transfer Time= Rotational delay/ #of block in a track
  * unit as ms
* Access Time
  * equals to reading time
  * = seek time+ rotational delay+ transfer time
  * 在read相邻的block时 阅读时间只增加transfer time
  * 在read不相邻track的两个block时就是一个阅读时间*2
  * access优先级是： 同一个block；相邻的block：同一个track：~~同一个cylinder的不同track~~：相邻的cylinder
  * （上面划线的地方是我说写错了 应该是same track different cylinder- 这也是我对cylinder理解的误区）

###### Buffering data

将数据从存储转移到memory

- minimum transfer unit is BLOCK

###### Disk Request - blocks读取顺序的几种方法

FIFO 

- 先request的block就先读取
- 性价比比较低 时间消耗长

Elevator

- 先阅读**同一方向**的data
- 包括多种变形（稍后添加）
- 比FIFO性能更好
- avoid starvation

Shortest-seek

- 先阅读与上一个已读block最近的目标
- 性能也比FIFO好
- cost starvation


#### RAID

Checksums- 附加位

每个扇区包含附加位，其值基于扇区中的数据位；

0- 偶数

1-基数

使用单个校验和位只允许可靠地检测一个位的错误，可以维护多个校验和位，以减少未能注意到错误的可能性

##### Multiple Disks

###### Striping

数据划分单位；either block or bit

在RAID系统中使用 Round Robin 算法分布

Bit striping

分布方式：D1b1r1，D2b2r1，D3b3r1........

    以record为单位

Block striping

分布方式：D1B1，D2B2,D3B3......

##### Introduction

Redundancy 冗余

通过储存冗余数据来提高数据的可靠性，在这里冗余数据可以用来重建丢失数据

有两种方法，一种是镜像，一种是parity scheme（checksum）

RAID系统由多个磁盘组成，其作用就是用来提高可靠性（冗余）和性能（通过使用striping）

##### Level 0

和普通disk的构建一样，四个磁盘通过striping排列

- 没有redundancy所以可靠性差
- 读写功能良好
- 最便宜

##### Level 1 Mirrored

每个磁盘都有个相同的副本，但是不通过striping排列

- 读取性能类似于single disk，random read性能还行
- 写入性能差
- 可靠但是成本高

##### Level 10 Striping+mirror

Striping排列with镜像磁盘

- read性能与level0 相似
- 比起single disk sequential和random read性能都快两倍
- 写入技能和level 1 一样差

level1 和10 之所以写作性能很差 是因为其存在冗余

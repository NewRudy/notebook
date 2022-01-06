[toc]

# Google 三大核心技术及其衍生

## 简介

参考自[谷歌三篇论文（GFS,MapReduce,BigTable）](https://blog.csdn.net/zhongqi2513/article/details/88708723)

Google于2003、2004、2006年分别发表了GFS、 MapReduce 和 BigTable 相关的 3 篇论文，其中MapReduce 和 BigTable都是以GFS为基础，这三大基础核心技术构建出了完整的分布式运算架构，也奠定了大数据和云计算的基础。

- Google File System(GFS)：可扩展的分布式文件系统，用于大型的、分布式的、对大量数据进行访问的应用。它运行于廉价的普通硬件上，提供容错功能。从根本上说：文件被分割成很多块，使用冗余的方式储存于商用机器集群上。
- MapReduce：描述了大数据的分布式计算方式，主要思想是将任务分解然后在多台处理能力较弱的计算节点中同时处理（Map），然后将结果合并从而完成大数据处理（Reduce）。
- BigTable：BigTable 是建立在 GFS 和 MapReduce 之上的。每个Table都是一个多维的稀疏图   --------------

参考自[Google大数据三大论文读后感](https://www.daimajiaoliu.com/daima/4ed1dc8981003fe)

三篇论文之后，Google继续公布的了一下技术：

- 2012 Spanner



开源实现（Hadoop）及Google自身的发展，参考自[技术硬核】从Google F1 看HTAP数据库的诞生](https://www.modb.pro/db/112540)

GFS  （HDFS） ---> Colossus

MapReduce  （MapReduc  ---> Spark） --->  Percolator/Dremel

BigTable  （HBase）  ---> F1/Spanner 



参考自[一文系统梳理Google三驾马车](https://jishuin.proginn.com/p/763bfbd66841)，有机会把这个课程学习了



![img](../image/d99386adc2ea03796ec59dd83231aceb.webp)

<div align='center'>大数据技术脉络</div>

![Google 技术 (1)](../image/Google%20%E6%8A%80%E6%9C%AF%20(1).jpg)

<div align='center'>大数据技术脉络</div>



## Google File System（GFS）

Google File System（GFS、GoogleFS），一种专有的分布式文件系统，由Google公司开发，运行于Linux平台上，尽管Google在2003年公布了该系统的一些技术细节，但Google并没有将该系统的软件部分开源，2013年，Google公布了Colossus项目，作为下一代的Google文件系统

### why hard

[为什么分布式存储困难](https://zhuanlan.zhihu.com/p/174768445)

个人理解：分布式的初衷是利用数百台计算机的资源来同时完成大量工作，获取巨大的性能加成（high performance），所以数据需要分片（sharding）存储在这些机器上。然后服务器会宕机，通信也会出现故障，不可能通过人工来修复错误，我们需要一个自动化的容错系统（fault tolerance）。实现容错的最有用的方法就是复制（replication），其中一个出错了，我们就立刻替上它的副本。但是数据在动态的变化，副本之间的复制不能同步的进行更新，一不小心两个数据的副本会不一致（inconsistency），这时候用户请求的数据取决于用户向哪个副本请求数据。显然我们要避免不一致的问题，为了达到一致性，不同的服务器之间需要通过网络进行额外的交互（Network interaction），这种交互会降低性能（low performance）。

![分布式难点](../image/%E5%88%86%E5%B8%83%E5%BC%8F%E9%9A%BE%E7%82%B9.jpg)

<div align='center'>分布式难点</div>

所以说，分布式系统的最大难点就在于各个节点的状态如何同步，也就是 [CAP（consistency，availability，partition tolerance）定理](https://www.ruanyifeng.com/blog/2018/07/cap.html)

1998年，加州大学的计算机科学家 Eric Brewer 提出，分布式系统有三个指标:

- Consistency
- Availability
- Partition tolerance

Eric Brewer 说，这三个指标不可能同时做到。这个结论就叫做 CAP 定理。

前文已经说到，故障总是存在，也即分区容错（Partition tolerance）是无法避免的，剩下的 C、A就只能选择一个。

为什么只能选择一个，举例：

![img](../image/bg2018071602.png)

![img](../image/bg2018071603.png)

如果保证 G2 的一致性，那么 G1 必须在写操作时，锁定 G2 的读操作和写操作。只有数据同步后，才能重新开放读写。锁定期间，G2 不能读写，可用性不成立。

如果保证 G2 的可用性，那么势必不能锁定 G2，所以一致性不成立。

综上所述，G2 无法同时做到一致性和可用性。系统设计时只能选择一个目标。如果追求一致性，那么无法保证所有节点的可用性；如果追求所有节点的可用性，那就没法做到一致性。

总结来说，我们以为分布式能给我们带来绝大的性能提升，但是因为种种原因（最大的原因就是各个节点的状态如何同步），最终实际上的效果差强人意。

### The Google File System

《The Google File System》是 GFS 最经典的论文，没有之一（2003年就有了:cow:)。

- [原文](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf)
- [翻译1](https://www.open-open.com/lib/view/open1328763454608.html)
- [翻译2](https://duanmeng.github.io/2017/12/07/gfs-notes/)

原文简要目录结构：

1. Introduction    简介

2. Design Overview    设计概述

   2.3 Architecture    架构

   2.6  Metadata    元数据

   2.7 Consistency Model 一致性模型

3. System Interactions    系统交互

4. master Operation    Master 节点操作

5. High Availability    容错和判断

简要理解：

#### 1. 简介

参考自[翻译2](https://duanmeng.github.io/2017/12/07/gfs-notes/)和[理解 The Google File System](https://qtozeng.top/2018/11/11/%E7%90%86%E8%A7%A3-The-Google-File-System/)

设计一个通用的分布式文件系统是不现实的，它不仅在实现上异常困难（因为不得不考虑所有应用场景），而且实际使用也难以满足要求（往往存在显而易见的性能或容错瓶颈）。[GFS](https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf) 设计初衷是利用数以千计的廉价机器为`MapReduce`提供底层**可靠且高性能**的分布式数据存储，以应对海量离线数据存储与处理的应用场景，比如存储应用程序持续产生的日志流以提供离线日志分析。由此，其设计目标为容错可靠(`fault tolerance`)、高性能读写(`high-performance read&write`)以及节约网络带宽(`save bandwidth`)。

- **组件故障是常态**（the norm rather than the exception）。该文件系统由数百甚至数千台存由廉价普通（commodity）硬件组装的存储机器，并且被大量的客户机器访问。组件的数量和质量几乎保证了任何时间都有一些组件失效甚至有些无法从当前错误中恢复。我们见过由应用bugs，操作系统bugs和人为错误，还有硬盘错误，内存，连接器，网络和电源供应引起的问题。因此，持续监控，错误探测，容错和自动恢复必须集成到系统中。
- **传统标准下文件都很大**。GB级的文件很常见。每个文件通常包含许多应用对象，比如Web文档。当我们通常要处理快速增长数据集组成的数十亿的对象时，不适合用数十亿大约KB大小的文件去管理。因此，设计假设和参数，例如I/O操作，块大小（blocksize）必须被重新审视（revisited）。
- **大部分文件是因追加数据被突变（mutated）而不是覆盖已有数据**。某个文件里的随机写实践上不存在。一旦写入这些文件只会被读取而且经常是顺序读。多种数据都共有（share）这些特征。一些可能组成数据仓库（constitute large repositories）被数据分析程序扫描。一些可能是运行应用持续生成的数据流。一些可能是归档的数据。一些可能是一台机器产出在另一台机器同时或者后面适时（whether simultaneously or later in time）被处理的中间数据， 基于这种大文件访问模式，追加成为性能优化和原子性保证的关注点，与此同时客户端缓存数据块失去了吸引力。
- **，综合设计应用和文件系统API，通过增加灵活性，对整个系统有好处**。例如，我们放松了（relaxed）GFS的一致性模型来极大的简化了该文件系统且没有对应用施加繁重的负担。我们还引入了一种原子追加操作，这样多个客户端可以对同一文件并发追加，而它们之间不需要额外的同步。本文后面会详细讨论这些。

#### 2. 设计概述

##### 2.3. 架构

参考自[理解 Google File System](https://lianhaimiao.github.io/2018/03/10/%E7%90%86%E8%A7%A3Google-File-System/)

以往的分布式系统，是没有中心节点的，但是在 GFS 中，我们将 Metadata（元数据，可以理解为控制信息）保存在 Master节点中，将用户需要的数据保存在 Chuck Sever 中， 这样我们可以方便的从 Master 中得知各个 Chuck Sever 的运行情况，同时 Master 不会成为制约这个系统的瓶颈，而且我们可以使用廉价的普通硬件作为 Chuck Sever 一举多得。

![GFS system architecture](../image/gfs_sys_arch.png)

<div align='center'>系统架构</div>

GFS将整个系统节点分为三类角色:

- client（客户端） GFS提供给应用程序的访问接口
- Master（主服务器） GFS的管理节点，也就是主节点，负责管理整个文件系统
- Chuck Sever（数据块服务器） 负责具体的数据存储工作

GFS实现机制：

- client 首先访问 Master节点，从 Master 那里知道，自己应该去那些 Chuck Sever 那里去读取、写入数据，然后 client 再访问这个 Chuck Sever 去完成数据读写的工作。**这样的设计方法实现了控制流和数据流的分离**
- clinet 和 Master 之间**只有控制流，没有数据流**，这样就极大的减轻了 Master 的负担。
- client 和 Chuck Sever 之间直接传输数据流，同时由于文件被分成多个 Chuck 进行分布式存储， client 可以同时访问多个 Chuck Sever 从而使得**整个系统的 I/O 高度并行，系统整体性能得到提升**。

GFS特点：

1. 采用中心服务器模型
   - 可以方便地增加 Chuck Sever
   - Master 可以掌握系统内所有 Chuck Sever 的情况，方便进行负载均衡。
   - 不存在元数据的一致性问题 （解释一下，元数据就是存储在 Master 中的信息，一个系统内部会有多个 Master，这些 Master 可以看作彼此的备份。）
2. 不缓存数据
   - 文件操作大部分是流式读写，不存在大量重复读写，使用 Cache 对性能提高不大。
   - Chuck Sever 上数据存取使用本地文件系统，不用Cache 就不需要考虑缓存一致性的问题。
   - 但是 Master 中的 metadata（元数据） 会缓存。
3. 在用户态下实现
   - 利用 POSIX 编程接口，提高了通用性。
   - Master 和 Chuck Sever 都以进程的方式运行，单个进行不影响整个操作系统。
   - GFS和操作系统运行在不同的空间，两者耦合性降低。

##### 2.6. 元数据

Master存储了3种主要类型的元数据：文件和块命名空间（namespace），文件到块的映射和每个块副本的位置。所有元数据保存在master的内存中。前两种类型（namespaces和file-to-块 mapping）也会被通过记录突变（mutations）操作日志（by logging mutations to an operation log）存储在master本地磁盘和复制到远端机器。使用日志使得我们可以简单，可靠地更新master状态和在master崩溃时保持一致性。Master不持久化块位置信息。相反，它在启动时向每个块服务器询问块位置信息，和某个块服务器加入集群时询问。

##### 2.7 一致性模型

参考自[翻译2](https://duanmeng.github.io/2017/12/07/gfs-notes/) 和 [理解 The Google File System](https://qtozeng.top/2018/11/11/%E7%90%86%E8%A7%A3-The-Google-File-System/)

GFS的宽松一致性模型支持我们的高度分布式的应用，`GFS`并没有采用复杂的一致性协议来保证副本数据的一致性，而是通过定义了三种不同的文件状态，并保证在这三种文件状态下，能够使得客户端看到一致的副本。三种状态描述如下：

- `Consitent`状态：当`chunk`被并发执行了操作后，不同的客户端看到的并发执行后的副本内容是一致的
- `defined`状态：在文件处于`consistent`状态的基础上，还要保证所有客户端能够看到在此期间对文件执行的所有并发操作。即当文件操作并发执行时，如果它们是全局有序执行的（执行过程中没有被打断），则由此产生的文件状态为`defined`（当然也是`consistent`）。
- `undefined`状态：如果并发操作文件失败，此时各客户端看到的文件内容不一致，则称文件处于`undefined`状态，当然也处于`inconsistent`状态。

文件命名空间突变（比如，文件创建）是原子的。它们只被master处理：命名空间锁保证原子性和正确性（4.1节）；master的操作日志定义全局操作顺序。

![img](../image/gfs-consistance.JPG)

<div align='center'>突变后文件区域状态</div>

#### 3. 系统交互

![img](../image/gfs-write.JPG)

<div align='center'>写入控制与数据流</div>

我们通过跟随一次写的控制流的步骤解释这个过程:

1. 客户端向master询问哪个块服务器握有这个块的租约和其它副本的位置。如果没有任何块有租约，master选择其中一个副本授予（未显示）。
2. Master回复首要副本的标识（identity）和其它副本的位置。客户端缓存这个数据用于未来的突变（mutations）。它只有当首要副本变得不可达或者回复说不再握有租约才需要再次联系master。
3. 客户端把数据推到（push）所有的副本。这个操作客户端可以任意顺序。每个块服务器会将数据存储在内部的LRU缓冲缓存直到数据被使用或者老化。通过数据流与控制流解耦，我们可以通过基于网络拓扑调度繁重的数据流而不管哪个块服务器是首要的来改善性能。3.2节会进一步讨论这些。
4. 一旦所有的副本确认收到数据，客户端发送写请求给primary。这个请求标识（identifies）了之前推送到所有副本的数据。Primary给所有收到的突变赋予连贯有序的序号，提供了必要的序列化。它序号应用突变到本地状态。
5. Primary转发了写请求给所有的二级副本（secondary replicas）。每个副本按照Primary分配的相同序号实施突变。
6. 所有副本回复Primary操作已经完成。
7. Primary回复客户端。任何副本上的任何错误都会报告给客户端。如果出现错误，primary和任意二级副本子集可能已经被成功写入。（如果写操作在primary上就已经失败了，它就不会被分配序列号和转发。）客户端请求被认为失败了，修改的区域处于不一致状态。我们的客户端代码通过重试失败的突变来处理这样的错误。在重试整个写之前，它会重试步骤(3)到步骤(7)。

如果应用的写很大或者跨块边界，GFS客户端会将其分解成多个写操作。它们都遵循上面所述控制流，但是可能被其它客户端的并发操作交错（interleaved）或者重写（overwritten）。因此，共享文件区域最终可能包含来自不同客户端的片段，虽然这些副本将会是一致的，因为各个操作是完全成功的且在所有副本上是相同顺序的。这使得文件区域处于2.7节所定义的一致但是未定义的状态（consistent but undefined）。

值得注意的技术：记录追加（record append），快照

#### 4. master 节点操作

Master执行所有命名空间操作。 此外，它还管理整个系统中的块复制：它进行放置决策，创建新块及其副本，并协调各种系统范围的活动以保持块完全复制，平衡所有块服务器的负载，并回收未使用的存储。 我们现在讨论这些主题。

1. 命名空间管理和锁：许多Master操作可能需要很长时间：例如，快照操作必须撤消快照覆盖的所有块上的块服务器租约。 我们不希望在运行时推迟其它Master操作。 因此，我们允许多个操作处于活动状态，并在命名空间的区域上使用锁定以确保正确的序列化。
2. 副本放置：最大限度地提高数据的可靠性和可用性，并最大化网络带宽利用率。
3. 垃圾回收：文件被删除后，GFS不会立即回收可用的物理存储。它只惰性地在文件和块级别的常规垃圾收集期间这样做。我们发现这个方法让系统更简单，更可靠。

#### 5. 容错和判断

参考自[理解 Google File System](https://lianhaimiao.github.io/2018/03/10/%E7%90%86%E8%A7%A3Google-File-System/)

##### Master 容错

Master维护文件系统所有的元数据（metadata），包括名字空间、访问控制信息、从文件到块的映射以及块的当前位置。

另外，每个 Chuck Sever 上都会保存 Chuck 副本的信息，每个 Chuck 默认有三个副本，这样当某个 Chuck 坏了之后，不会影响 Chuck 数据的读取。

当 Master 发生故障时，在磁盘数据保存完好的情况下，可以快速的恢复所有的 metadata。并且为了防止 Master 彻底死机的情况， GFS 还提供了 Master 的远程备份。

##### Chuck Sever 容错

- GFS采用副本的方式实现Chunk Server的容错
- 每一个Chunk有多个存储副本（默认为三个）
- 对于每一个Chunk，必须将所有的副本全部写入成功，才视为成功写入
- 相关的副本出现丢失或不可恢复等情况，Master自动将该副本复制到其他 Chunk Server
- GFS中的每一个文件被划分成多个Chunk，Chunk的默认大小是64MB
- 每一个Chunk以Block为单位进行划分，大小为64KB，每一个Block对应一个 32bit 的校验和



### 开源技术

#### HDFS



### Colossus

#### 背景

Colossus 的一些背景知识：

- 它是下一代 GFS。
- 其设计增强了存储可扩展性并提高了可用性，以应对数量不断增长的应用程序的大量数据需求。 
- Colossus 引入了分布式元数据模型，该模型提供了更具可扩展性和高可用性的元数据子系统。 

![巨像控制平面.jpg](../image/Colossus_control_plane.max-2000x2000.jpg)

<div align='center'>Colossus 架构图</div>

- Client Library：客户端库是应用程序或服务与 Colossus 交互的方式，也是整个文件系统中最复杂的部分。
- Colossus Control Plane：可扩展的元数据服务，它由许多 Curator 组成。客户直接与策展人交谈以进行控制操作，例如文件创建，并且可以水平扩展。
- Metadata database：Curators 将文件系统元数据存储在 Google 的高性能 NoSQL 数据库[BigTable 中](https://cloud.google.com/bigtable)。构建 Colossus 的最初动机是为了解决我们在尝试适应与搜索相关的元数据时使用 Google 文件系统 (GFS) 遇到的扩展限制。
- D File Servers：Colossus 还最大限度地减少了网络上数据的跃点数。
- Custodians：在维护数据的持久性和可用性以及整体效率、处理磁盘空间平衡和 RAID 重建等任务方面发挥着关键作用。 

#### 简单理解

参考自[Google Spanner原理：地球上最大的单一数据库](https://cloud.tencent.com/developer/article/1131036)

2013年Google提出的下一代文件系统，而关于Colossus目前为止还没有相关的论文，网上只有一些零散介绍：[Colossus](https://link.zhihu.com/?target=https%3A//www.systutorials.com/3202/colossus-successor-to-google-file-system-gfs/)。

Colossus也是一个不得不提起的技术。他是第二代GFS，对应开源世界的新HDFS。GFS是著名的分布式文件系统。初代GFS是为批处理设计的。对于大文件很友好，吞吐量很大，但是延迟较高。所以使用他的系统不得不对GFS做各种优化，才能获得良好的性能。那为什么Google没有考虑到这些问题，设计出更完美的GFS ?因为那个时候是2001年，Hadoop出生是在2007年。如果Hadoop是世界领先水平的话，GFS比世界领先水平还领先了6年。

Colossus是第二代GFS。Colossus是Google重要的基础设施，因为他可以满足主流应用对FS的要求。Colossus的重要改进有：

- 优雅Master容错处理 (不再有2s的停止服务时间)

-  Chunk大小只有1MB (对小文件很友好)

- Master可以存储更多的Metadata(当Chunk从64MB变为1MB后，Metadata会扩大64倍，但是Google也解决了)

Colossus可以自动分区Metadata。使用Reed-Solomon算法来复制，可以将原先的3份减小到1.5份，提高写的性能，降低延迟。客户端来复制数据。具体细节未开源。



## MapReduce

MapReduce是一个编程模型，也是一个处理和生成超大数据集的算法模型的相关实现。用户首先创建一个Map函数处理一个基于 key/value pair的数据集合，输出中间的基于key/value pair的数据集合；然后再创建一个Reduce函数用来合并所有的具有相同中间key值的中间value值。

MapReduce架构的程序能够在大量的普通配置的计算机上实现并行化处理。这个系统在运行时只关心：如何分割输入数据，在大量计算机组成的 集群上的调度，集群中计算机的错误处理，管理集群中计算机之间必要的通信。采用MapReduce架构可以使那些没有并行计算和分布式处理系统开发经验的 程序员有效利用分布式系统的丰富资源。

### MapReduce: Simplified Data Processing on Large Clusters

[原文](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)

[翻译](https://www.open-open.com/lib/view/open1328763069203.html)

原文简要目录结构：

1. 介绍
2. 编程模型
3. 实现

简要理解

#### 介绍

大数据的数据处理一般在概念上容易理解，然后由于输入的数据量巨大，因此想要在可接受的时间内完成运算，只有将这些计算分布在成百上千的主机上。如何处理并行计算、如何分发数据、如何处理错误？所有这些问题综合在一起，需要大量的代码处理，因此也使得原本简单的运算变得难以处理

为了解决上述复杂的问题，我们设计一个新的抽象的问题，使用这个抽象模型，我们摘要表述我们想要执行的简单运算即可，而不必关心并行计算、容错、数据分布、负载均衡等复杂的细节，这些问题都被封装在一个库里面。

大多数运算都包含这样的操作：在输入数据的“逻辑”记录上应用 Map 操作得到一个中间 key/value pair 集合，然后在所有具有相同 key 值的 value 值上应用 Reduce 操作，从而达到合并中间的数据，得到一个想要的结果的目的。使用 MapReduce 模型，再结合用户实现的 Map 和 Reduce 函数，我们就可以非常容易的实现大规模并行化计算，通过 MapReduce 模型自带的 re-execution 功能，也提供了初级的容灾实现方案

MapReduce 的主要贡献是通过简单的接口来实现自动的并行化和大规模的分布式计算，通过使用 MapReduce 模型接口实现在大量普通的 PC 机上高性能计算

#### 编程模型

MapReduce 编程模型的原理是：利用一个 key/value pair 集合来产生一个输出的 key/value pair 集合。

MapReduce 库的用户两个函数表达这个计算：Map 和 Reduce，用户自定义的 Map 函数接受一个输入的 key/value pair 值，然后产生一个中间 key/value pair 值的集合。 MapReduce 库把所有具有相同中间 key 值的中间 value值集合在一起后传递给 Reduce 函数

eg:

```tex
map(String key, String value):
    // key: document name
    // value: document contents
    for each word w in value:
        EmitIntermediate(w, “1″);
reduce(String key, Iterator values):
    // key: a word
    // values: a list of counts
    int result = 0;
    for each v in values:
        result += ParseInt(v);
    Emit(AsString(result));
```

#### 实现

MapReduce  模型有多种实现方式，如何正确选择取决于具体的环境：

- 小型的共享内存式的机器
- 大型 NUMA 架构的多处理器的主机
- 大型的网络连接集群

![image-20211201094605739](../image/image-20211201094605739.png)

<div align='center'>执行概况</div>

操作流程：

1. 用户程序首先调用 MapReduce 库将输入文件分成 M 个数据片度，每个数据片段的大小一般从 16 MB 到 64 MB，然后用户程序在集群中创建大量的程序副本
2. 这些程序副本中的有一个特殊的程序——master。副本中其它程序都是 worker 程序，由 master 分配任务，有 M 个 Map 任务和 R 个 Reduce 任务被分配， master 将一个 Map 任务或 Reduce 任务分配一个空闲的 worker
3. 被分配了 map 任务的 worker 程序读取相关的输入数据片段，从输入的数据片段中解析出 key/value pair，然后把 key/value pair 传递给用户自定义的 Map 函数，由 Map 函数生成并输出中间 key/value pair，并缓存在内存中
4. 缓存中的 key/value pair 通过分区函数分成 R 个区域，之后中周期性的写入到本地磁盘上。缓存的 key/value pair 在本地磁盘上的存储位置将被回传给 master，由 master 负责把这些存储位置再传送给 Reduce worker
5. 当 Reduce worker 程序接收到master程序发来的数据存储位置信息后，适用 RPC 从 Map worker 所在的主机的磁盘上读取这些缓存数据。当 Reduce worker 读取了所有的中间数据，通过对 key 进行排序后使得具有相同 key 值的数据聚合在一起。
6. Reduce worker 程序遍历排序后的中间数据，对于每一个唯一的中间 key 值，Reduce worker 程序将这个 key 值和它相关的种中间 value 值的集合传递给用户定义的 Reduce 函数。Reduce 函数的输出被追加到所属分区的输出文件
7. 当所有的 Map 和 Reduce 任务都完成之后，master 唤醒用户程序

Master 数据结构：Master持有一些数据结构，它存储每一个 Map 和 Reduce 任务的状态（空闲、工作中或完成），以及 Worker 机器（非空闲任务的机器）的标识。Master 就像一个数据管道，中间文件存储区域的位置信息通过这个管道从 Map 传递给 Reduce，因此对于每个已经完成 Map 任务，Master存储了 Map 任务产生的 R 个中间文件存储区域的大小和位置。当 Map 任务完成时，Master 接收到位置和大小爱的更新信息，这些信息被逐步递增的推送给那些正在工作的 Reduce 工作

#### 存储位置

在我们的计算运行环境中，网络带宽是一个相当匮乏的资源。我们通过尽量把输入数据(由GFS管理)存储在集群中机器的本地磁盘上来节省网络带 宽。GFS把每个文件按64MB一个Block分隔，每个Block保存在多台机器上，环境中就存放了多份拷贝(一般是3个拷贝)。MapReduce的 master在调度Map任务时会考虑输入文件的位置信息，尽量将一个Map任务调度在包含相关输入数据拷贝的机器上执行；如果上述努力失败 了，master将尝试在保存有输入数据拷贝的机器附近的机器上执行Map任务(例如，分配到一个和包含输入数据的机器在一个switch里的 worker机器上执行)。当在一个足够大的cluster集群上运行大型MapReduce操作的时候，大部分的输入数据都能从本地机器读取，因此消耗 非常少的网络带宽。

#### 结束语

MapReduce编程模型在Google内部成功应用于多个领域。我们把这种成功归结为几个方面：首先，由于MapReduce封装了并行处 理、容错处理、数据本地化优化、负载均衡等等技术难点的细节，这使得MapReduce库易于使用。即便对于完全没有并行或者分布式系统开发经验的程序员 而言；其次，大量不同类型的问题都可以通过MapReduce简单的解决。比如，MapReduce用于生成Google的网络搜索服务所需要的数据、用 来排序、用来数据挖掘、用于机器学习，以及很多其它的系统；第三，我们实现了一个在数千台计算机组成的大型集群上灵活部署运行的MapReduce。这个 实现使得有效利用这些丰富的计算资源变得非常简单，因此也适合用来解决Google遇到的其他很多需要大量计算的问题。

我们也从MapReduce开发过程中学到了不少东西。首先，约束编程模式使得并行和分布式计算非常容易，也易于构造容错的计算环境；其次，网络带 宽是稀有资源。大量的系统优化是针对减少网络传输量为目的的：本地优化策略使大量的数据从本地磁盘读取，中间文件写入本地磁盘、并且只写一份中间文件也节 约了网络带宽；第三，多次执行相同的任务可以减少性能缓慢的机器带来的负面影响*（alex注：即硬件配置的不平衡），*同时解决了由于机器失效导致的数据丢失问题。





### 开源技术

#### Hadoop  Mapreduce



#### Spark



### Percolator/Dremel





## BigTable

Bigtable是一个分布式的结构化数据存储系统，它被设计用来处理海量数据：通常是分布在数千台普通服务器上的PB级的数据。Google的很 多项目使用Bigtable存储数据，包括Web索引、Google Earth、Google Finance。这些应用对Bigtable提出的要求差异非常大，无论是在数据量上（从URL到网页到卫星图像）还是在响应速度上（从后端的批量处理到 实时数据服务）。尽管应用需求差异很大，但是，针对Google的这些产品，Bigtable还是成功的提供了一个灵活的、高性能的解决方案。利用这个模型，用户可以动态的控制数据的分布和格式。

通常GFS用于存储海量数据，文件系统将数据在各个节点冗余复制。在某种程度上MapReduce可以对GFS进行补充，以便充分利用GFS中廉价的服务器所提供的CPU。和GFS一起构成了处理海量数据的核心力量，构建类似于Google的搜索索引也是一样的。但是这两个系统都缺乏实时随机存取数据的能力，在web应用处理方面还有所欠缺。

### Bigtable: A Distributed Storage System for Structured Data

[原文](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)

[翻译](https://www.open-open.com/lib/view/open1328763508092.html)

原文简要目录结构：

1. 介绍
2. 数据模型

4. BigTable 构件

5. 实施

   5.1 Tablet的位置

   5.2 Tablet 分配

   5.3 Tablet 服务

9. 结果感言

#### 1. 介绍

Bigtable已经在超过60个Google的产品和项目上得到了应用，包括 Google Analytics、Google Finance、Orkut、PersonalizedSearch、Writely和Google Earth。这些产品对Bigtable提出了迥异的需求，有的需要高吞吐量的批处理，有的则需要及时响应，快速返回数据给最终用户。它们使用的Bigtable集群的配置也有很大的差异，有的集群只有几台服务器，而有的则需要上千台服务器、存储几百TB的数据。

在很多方面，Bigtable 和数据库很类似，它使用了很多数据库的实现策略。并行数据库和内存数据库，已经具备高可扩展性和高性能，但是 Bigtable 不支持完整的关系型数据模型，与之相反，Bigtable 为客户提供了简单的数据模型，利用这个模型，客户可以动态控制数据的分布和格式。数据的下标是行和列的名字，名字可以是任意的字符串。Bigtable将存储的数据都视为字符串，但是Bigtable本身不去解析这些字符串，通常客户程序会把各种结构化或者半结构化的数据串行化到这些字符串里。通过仔细选择数据的模式，客户可以控制数据的位置相关性。最后，可以通过BigTable的模式参数来控制数据是存放在内存中还是硬盘上

#### 2. 数据模型

Bigtable 是一个稀疏的、分布式的、持久化存储的多维度排序Map，索引key 是行关键字、列关键字和时间戳，值value是一个未经解析的byte数组

```tex
(row:string,column:string,time:int64)->string
```

![Bigtable-DataModel-Row-Column-Timestamp-Value](../image/2017-08-12-Bigtable-DataModel-Row-Column-Timestamp-Value.jpg-1000width)

这里最重要的就是 `row` 的值，它的长度最大可以为 64KB，对于同一 `row` 下数据的读写都可以看做是原子的；因为 Bigtable 是按照 `row` 的值使用字典顺序进行排序的，每一段 `row` 的范围都会被 Bigtable 进行分区，并交给一个 tablet 进行处理。

##### Rows

可以是任意的字符串（最大支持 64 kb，但是对大多数用户，10-100个字节就足够了）。对同一个行关键字的读或者写操作都是原子的，这个设计决策能够使用户很容易的理解程序在对同一个行进行并发更新操作的行为

Bigtable 通过行关键字的字典顺序来组织数据。表中的每个行都可以动态分区，每个分区叫做一个"Tablet"，Tablet 是数据分布和负载均衡的最小单位

##### Cols

列是访问控制的基本单位。存放在同一列簇下的所有数据通常都属于同一类型（我们可以把同一列簇下的数据压缩在一起）。列簇在使用之前必须先创建，然后才能在列簇中任何的列关键字下存放数据。列簇创建后，其中的任何一个列关键字下都可以存放数据

##### Timestamps

在Bigtable 中，表中的每一个数据项都可以包含同一份数据的不同版本，不同版本的数据通过时间戳来索引。Bigtable时间戳的类型是64位整型，

#### 4. 基础组成

BIgtable 是建立在其它的几个 Google 基础构件之上的。BigTable 使用 Google 的分布式文件系统（GFS）存储日志文件和数据文件。BigTable 集群通常运行在一个共享的机器池中，池中的机器还会运行其它的各种各样的分布式应用程序，BigTable的进程通常要和其它应用的进程共享机器。BigTable 依赖集群管理系统来调度任务、管理共享机器上的资源，处理机器的故障以及监视机器的状态

BigTable内部存储数据的文件是GoogleSSTable格式的。SSTable是一个持久化的、排序的、不可更改的Map结构，而Map是一个key-value 映射的数据结构，key和value的值都是任意的Byte串。可以对SSTable进行如下的操作：查询与一个key值相关的value，或者遍历某个 key值范围内的所有的key-value对。从内部看，SSTable是一系列的数据块（通常每个块的大小是64KB，这个大小是可以配置的）。 SSTable使用块索引（通常存储在SSTable的最后）来定位数据块；在打开SSTable的时候，索引被加载到内存。每次查找都可以通过一次磁盘 搜索完成：首先使用二分查找法在内存中的索引里找到数据块的位置，然后再从硬盘读取相应的数据块。也可以选择把整个SSTable都放在内存中，这样就不 必访问硬盘了。

BigTable还依赖一个高可用的、序列化的分布式锁服务组件，叫做Chubby【8】。一个Chubby服务包括了5个活动的副本，其中的一个副本被选为Master，并且处理请求。只有在大多数副本都是正常运行的，并且彼此之间能够互相通信的情况下，Chubby服 务才是可用的。当有副本失效的时候，Chubby使用Paxos算法【9,23】来保证副本的一致性。Chubby提供了一个名字空间，里面包括了目录和 小文件。每个目录或者文件可以当成一个锁，读写文件的操作都是原子的。Chubby客户程序库提供对Chubby文件的一致性缓存。每个Chubby客户 程序都维护一个与Chubby服务的会话。如果客户程序不能在租约到期的时间内重新签订会话的租约，这个会话就过期失效了(alex注：又用到了lease。原文是：Aclient’s session expires if it is unable to renew its session lease within the leaseexpiration time.)。当一个会话失效时，它拥有的锁和打开的文件句柄都失效了。Chubby客户程序可以在文件和目录上注册回调函数，当文件或目录改变、或者会 话过期时，回调函数会通知客户程序。

#### 5. Tablet Location

BigTable 包括了三个主要的组件：链接到客户程序中的库、一个Master 服务器和多个 Tablet 服务器。针对系统工作负载的情况，BigTable 可以动态的向集群中添加（或者删除）Tablet 服务器

Master服务器主要负责以下工作：为Tablet服务器分配Tablets、检测新加入的或者过期失效的Table服务器、对Tablet服务器进行负载均衡、以及对保存在GFS上的文件进行垃圾收集。除此之外，它还处理对模式的相关修改操作，例如建立表和列族。

每个Tablet服务器都管理一个Tablet的集合（通常每个服务器有大约数十个至上千个Tablet）。每个Tablet服务器负责处理它所加载的Tablet的读写操作，以及在Tablets过大时，对其进行分割。

一个BigTable集群存储了很多表，每个表包含了一个Tablet的集合，而每个Tablet包含了某个范围内的行的所有相关数据。初始状态 下，一个表只有一个Tablet。随着表中数据的增长，它被自动分割成多个Tablet，缺省情况下，每个Tablet的尺寸大约是100MB到 200MB。

##### Tablet 的位置信息

![image-20211202100615428](../image/image-20211202100615428.png)

<div align='center'>Tablet 的层次结构</div>

![Tablet-Location-Hierarchy](../image/2017-08-12-Tablet-Location-Hierarchy.jpg-1000width)

第一层是一个存储在Chubby中的文件，它包含了Root Tablet的位置信息。Root Tablet包含了一个特殊的METADATA表里所有的Tablet的位置信息。METADATA表的每个Tablet包含了一个用户Tablet的集合。RootTablet实际上是METADATA表的第一个Tablet，只不过对它的处理比较特殊 — Root Tablet永远不会被分割 — 这就保证了Tablet的位置信息存储结构不会超过三层。

在METADATA表里面，每个Tablet的位置信息都存放在一个行关键字下面，而这个行关键字是由Tablet所在的表的标识符和Tablet 的最后一行编码而成的。METADATA的每一行都存储了大约1KB的内存数据。在一个大小适中的、容量限制为128MB的METADATA Tablet中，采用这种三层结构的存储模式，可以标识2^34个Tablet的地址

#### Tablet 管理

既然在整个 Bigtable 中有着海量的 tablet 服务器以及数据的分片 tablet，那么 Bigtable 是如何管理海量的数据呢？Bigtable 与很多的分布式系统一样，使用一个主服务器将 tablet 分派给不同的服务器节点。

![Master-Manage-Tablet-Servers-And-Tablets](../image/2017-08-12-Master-Manage-Tablet-Servers-And-Tablets.jpg-1000width)

为了减轻主服务器的负载，所有的客户端仅仅通过 Master 获取 tablet 服务器的位置信息，它并不会在每次读写时都请求 Master 节点，而是直接与 tablet 服务器相连，同时客户端本身也会保存一份 tablet 服务器位置的缓存以减少与 Master 通信的次数和频率。

#### 读和写请求

从读写请求的处理，我们其实可以看出整个 Bigtable 中的各个部分是如何协作的，包括日志、memtable 以及 SSTable 文件。

![Tablet-Serving](../image/2017-08-12-Tablet-Serving.jpg-1000width)

当有客户端向 tablet 服务器发送写操作时，它会先向 tablet 服务器中的日志追加一条记录，在日志成功追加之后再向 memtable 中插入该条记录；这与现在大多的数据库的实现完全相同，通过顺序写向日志追加记录，然后再向数据库随机写，因为随机写的耗时远远大于追加内容，如果直接进行随机写，由于随机写执行时间较长，在写操作执行期间发生设备故障造成数据丢失的可能性相对比较高。

当 tablet 服务器接收到读操作时，它会在 memtable 和 SSTable 上进行合并查找，因为 memtable 和 SSTable 中对于键值的存储都是字典顺序的，所以整个读操作的执行会非常快。

#### 表的压缩

随着写操作的进行，memtable 会随着事件的推移逐渐增大，当 memtable 的大小超过一定的阈值时，就会将当前的 memtable 冻结，并且创建一个新的 memtable，被冻结的 memtable 会被转换为一个 SSTable 并且写入到 GFS 系统中，这种压缩方式也被称作 *Minor Compaction*。

![Minor-Compaction](../image/2017-08-12-Minor-Compaction.jpg-1000width)

每一个 Minor Compaction 都能够创建一个新的 SSTable，它能够有效地降低内存的占用并且降低服务进程异常退出后，过大的日志导致的过长的恢复时间。既然有用于压缩 memtable 中数据的 Minor Compaction，那么就一定有一个对应的 Major Compaction 操作。

![Major-Compaction](../image/2017-08-12-Major-Compaction.jpg-1000width)

Bigtable 会在**后台周期性**地进行 *Major Compaction*，将 memtable 中的数据和一部分的 SSTable 作为输入，将其中的键值进行归并排序，生成新的 SSTable 并移除原有的 memtable 和 SSTable，新生成的 SSTable 中包含前两者的全部数据和信息，并且将其中一部分标记为删除的信息彻底清除。

#### Google Earth

Google通过一组服务为用户提供了高分辨率的地球表面卫星图像，访问的方式可以使通过基于Web的Google Maps访问接口（maps.google.com），也可以通过Google Earth定制的客户端软件访问。这些软件产品允许用户浏览地球表面的图像：用户可以在不同的分辨率下平移、查看和注释这些卫星图像。这个系统使用一个表 存储预处理数据，使用另外一组表存储用户数据。

数据预处理流水线使用一个表存储原始图像。在预处理过程中，图像被清除，图像数据合并到最终的服务数据中。这个表包含了大约70TB的数据，所以需要从磁盘读取数据。图像已经被高效压缩过了，因此存储在Bigtable后不需要再压缩了。

Imagery表的每一行都代表了一个单独的地理区域。行都有名称，以确保毗邻的区域存储在了一起。Imagery表中有一个列族用来记录每个区域 的数据源。这个列族包含了大量的列：基本上市每个列对应一个原始图片的数据。由于每个地理区域都是由很少的几张图片构成的，因此这个列族是非常稀疏的。

数据预处理流水线高度依赖运行在Bigtable上的MapReduce任务传输数据。在运行某些MapReduce任务的时候，整个系统中每台Tablet服务器的数据处理速度是1MB/s。

这个服务系统使用一个表来索引GFS中的数据。这个表相对较小（大约是500GB），但是这个表必须在保证较低的响应延时的前提下，针对每个数据中心，每秒处理几万个查询请求。因此，这个表必须在上百个Tablet服务器上存储数据，并且使用in-memory的列族。



### 开源技术

#### LevelDB

LevelDB 是对 Bigtable 论文中描述的键值存储系统的单机版的实现，它提供了一个极其高速的键值存储系统，并且由 Bigtable 的作者 [Jeff Dean](https://research.google.com/pubs/jeff.html) 和 [Sanjay Ghemawat](https://research.google.com/pubs/SanjayGhemawat.html) 共同完成，可以说高度复刻了 Bigtable 论文中对于其实现的描述。

#### Hbase



### F1



### Spanner





## Spanner

参考自[Google Spanner原理：地球上最大的单一数据库](https://cloud.tencent.com/developer/article/1131036)

Spanner 是Google的全球级的分布式数据库 (Globally-Distributed Database) 。Spanner的扩展性达到了令人咋舌的全球级，可以扩展到数百万的机器，数已百计的数据中心，上万亿的行。更给力的是，除了夸张的扩展性之外，他还能同时通过同步复制和多版本来满足外部一致性，可用性也是很好的。冲破CAP的枷锁，在三者之间完美平衡。

Spanner是个可扩展，多版本，全球分布式还支持同步复制的数据库。他是Google的第一个可以全球扩展并且支持外部一致的事务。Spanner能做到这些，离不开一个用GPS和原子钟实现的时间API。这个API能将数据中心之间的时间同步精确到10ms以内。因此Spanner是个可扩展，多版本，全球分布式还支持同步复制的数据库。他是Google的第一个可以全球扩展并且支持外部一致的事务。Spanner能做到这些，离不开一个用GPS和原子钟实现的时间API。这个API能将数据中心之间的时间同步精确到10ms以内。因此有几个给力的功能：无锁读事务，原子schema修改，读历史数据无block。有几个给力的功能：无锁读事务，原子schSpanner是个可扩展，多版本，全球分布式还支持同步复制的数据库。他是Google的第一个可以全球扩展并且支持外部一致的事务。Spanner能做到这些，离不开一个用GPS和原子钟实现的时间API。这个API能将数据中心之间的时间同步精确到10ms以内。因此有几个给力的功能：无锁读事务，原子schema修改，读历史数据无block。Spanner是个可扩展，多版本，全球分布式还支持同步复制的数据库。他是Google的第一个可以全球扩展并且支持外部一致的事务。Spanner能做到这些，离不开一个用GPS和原子钟实现的时间API。这个API能将数据中心之间的时间同步精确到10ms以内。因此有几个给力的功能：无锁读事务，原子schema修改，读历史数据无block。Spanner是个可扩展，多版本，全球分布式还支持同步复制的数据库。他是Google的第一个可以全球扩展并且支持外部一致的事务。Spanner能做到这些，离不开一个用GPS和原子钟实现的时间API。这个API能将数据中心之间的时间同步精确到10ms以内。因此有几个给力的功能：无锁读事务，原子schema修改，读历史数据无block。ema修改，读历史数据无block。





### Spanner: Google’s Globally-Distributed Database

[原文](https://static.googleusercontent.com/media/research.google.com/en//archive/spanner-osdi2012.pdf)

[翻译](https://mrcroxx.github.io/posts/paper-reading/spanner-osdi2012/)

Spanner是一个可伸缩、全球化分布的数据库，其由Google设计、构建、并部署。在抽象的最高层，Spanner是一个将数据分片（shard）到分布在全世界的多个数据中心中的跨多个Paxos[21]状态机集合上的数据库。Spanner采用多副本以提供全球化的可用性和地理位置优化；客户端自动地在副本间进行故障转移。在数据总量或服务器的数量变化时，Spanner自动地在机器间重分片数据，并自动地在机器间（甚至在数据中心间）迁移数据来平衡负载和应对故障。按照设计，Spanner扩展到跨数百个数据中心的数百万台机器与数万亿个数据库行。





## Borg



### Kubernetes



## Flume





## 开源技术

[大数据中台](https://zhongqi2513.blog.csdn.net/article/details/108275368)

发展阶段

1、单一系统
2、分布式系统
3、平台化（服务业务，支撑作用）
4、中台化（驱动业务，中枢作用）

### 单一系统

数据仓库，数据库

### 分布式系统

上述三篇论文提出一种面向海量数据分析、面向海量异构数据的统御i计算和存储方法，奠定了现代大数据、大规模并行计算的基础。Yahoo对此做了开源实现，就是现在的 Hadoop

演变关系：

GFS—->HDFS(2004)

Google MapReduce—->Hadoop MapReduce (2004)--> Spark (2014)

BigTable—->HBase(2008)

随着Hadoop技术日趋成熟，2010年提出一个新的概念：**数据湖（Data Lake）是一个以原始格式存储数据的存储库或系统**

2018年提出数据河的概念，避免“数据湖”成为“数据沼泽”，流动的“数据河”是关键。因为大部分使用数据湖的企业在数据真的需要使用的时候，往往因为数据湖中的数据质量太差而无法最终使用。数据只有流动起来，才可以不成为数据沼泽，湖泊只是暂存数据河流的基地。数据流动就意味着所有的数据产生，最终要有它的耕种者和使用者。要让数据有效流动起来，就要建立有效的“数据河”（Data River）。数据河（Data River）就是在由源头产生清晰干净的有效数据（去ETL化，数据源头业务就像生态水源一样，不让污水流下去），通过各个河流网，流向各个数据消费端的架构。

### 数据工厂时代：大数据平台

大数据平台是面向数据研发场景的，覆盖数据研发的完整链路的数据工作台。就是为了提高数据研发的效率，降低数据研发的门槛，让数据能够在一个设备流水线上快速地完成加工。

大数据平台按照使用场景，分为数据集成、数据开发、数据测试……任务运维，大数据平台的使用对象是数据开发。大数据平台的底层是以 Hadoop 为代表的基础设施，分为计算、资源调度和存储等。

数据存储：HDFS，HBase，Kudu等
数据计算：MapReduce, Spark, Flink
交互式查询：Impala, Presto
在线实时分析：ClickHouse，Kylin，Doris，Druid，Kudu等
资源调度：YARN，Mesos，Kubernetes
任务调度：Oozie，Azakaban，AirFlow等
....
数据收集，数据迁移，服务协调，安装部署，数据治理等
大数据平台像一条设备流水线，经过大数据平台的加工，原始数据变成了指标，出现在各个报表或者数据产品中。

### 数据价值时代：数据中台

在应用大数据平台架构的时候，你可能遇到这么个问题：因为烟囱式的开发，不同数据应用可能存在相同应用指标，但是运营可能发现这些数据指标的结果不一致，因为不知道该用谁信任谁而导致运营对数据的信任下降。

数据割裂的另外一个问题，就是大量的重复计算、开发，导致的研发效率的浪费，计算、存储资源的浪费，大数据的应用成本越来越高。

如果你是运营，当你想要一个数据的时候，开发告诉你至少需要一周，你肯定想是不是太慢了，能不能再快一点儿？

如果你是数据开发，当面对大量的需求的时候，你肯定是在抱怨，需求太多，人太少，活干不完。

如果你是一个企业的老板，当你看到每个月的账单成指数级增长的时候，你肯定觉得这也太贵了，能不能再省一点，要不吃不消了。

这些问题的根源在于，数据无法共享。

2016 年，阿里巴巴率先提出了“大中台，小前台”战略，推出了数据中台。数据中台的核心，是避免数据的重复计算，通过数据服务化，提高数据的共享能力，赋能数据应用。之前，数据是要啥没啥，中间数据难于共享，无法积累。现在建设数据中台之后，要啥有啥，数据应用的研发速度不再受限于数据开发的速度，然后我们就可以根据需求场景，快速孵化出很多数据应用，这些应用让数据产生价值。

总的来说，数据中台吸收了传统数据仓库、数据湖、大数据平台的优势，同时又解决了数据共享的难题，通过数据应用，实现数据价值的落地。
2016 年，阿里巴巴就提出了数据中台建设的核心方法论：OneData 和 OneService，OneData 就是所有数据只加工一次OneService，数据即服务，强调数据中台中的数据应该是通过 API 接口的方式被访问。

现阶段企业数据应用现状：

数据量小的使用 MySQL：Hive数仓，Spark计算引擎的计算结果导出到MySQL

数据量大的使用HBase + ElasticSearch：解决海量数据中的低延迟高效查询

需要多维分析的可能需要 ClickHouse，Kylin，Greenplum：提供现在分析能力

实时性要求高的需要用到 Redis
![img](../image/20200828111450122.jpg)

<div align='center'>网易数据中台</div>

## 发展

谷歌新一代搜索引擎平台和大数据分析核心技术 Google是GFS MapReduce BigTable的缔造者，但Google 新一代[搜索引擎平台](https://www.zhihu.com/search?q=搜索引擎平台&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"35631048"})正逐步用更强计算能力的系统来替换原有系统，新一代搜索引擎平台有几个核心技术系统：

1. 于Percolator的增量处理索引系统来取代MapReduce批处理索引系统，这个索引系统被称作Caffeine，它比MapReduce批处理索引系统搜索更快。

2. 是专为BigTable设计的分布式存储Colossus，也被称为GFS2（二代Google[文件系统](https://www.zhihu.com/search?q=文件系统&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"35631048"})），它专为建立Caffeine搜索索引系统而用。

3. 是列存储数据库BigTable，但为了更好地支持大数据集的互动分析，Google推出了Dremel和PowerDrill。Dremel被设计用来管理非常大量的大数据集（指数据集的数量和每数据集的规模都大），而PowerDrill则设计用来分析少量的大数据集（指数据集的规模大，但数据集的数量不多）时提供更强大的分析性能。

4. Google Instant提供服务的实时搜索引擎存储和分析架构

5. 是Pregel，这是谷歌更快捷的网络和图算法。 　　在谷歌新一代搜索引擎平台上，每月40亿小时的视频，4.25亿Gmail用户，150,000,000 GB Web索引，却能实现0.25秒搜索出结果

   

Google基础云服务

基于Colossus，谷歌为用户提供计算、存储和应用的云服务。计算服务包括计算的引擎（ComputeEngine）和应用APP的引擎(AppEngine)；存储服务包括云存储（CloudStorge）、云SQL(CLoudSQL)、云数据存储（Cloud DataStore）、永久磁盘等服务；云应用服务包括BigQuery、云终端（Cloud Endpoints）、缓冲、队列等


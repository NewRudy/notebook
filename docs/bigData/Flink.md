# Flink

[toc]

## 摘要

Apache Flink 是一个用于分布式流和批数据处理的开源平台。Flink 的核心是流数据流引擎，为数据流上的分布式计算提供数据分发，通信和容错。Flink 在流引擎之上构建批处理，覆盖本机迭代支持，托管内存和程序优化。

- **概念**：从Flink的[数据流编程模型](https://flink.sojb.cn/concepts/programming-model.html)和[分布式运行时环境](https://flink.sojb.cn/concepts/runtime.html)的基本概念开始。这将有助于您了解文档的其他部分，包括设置和编程指南。我们建议您先阅读这些部分。
- **教程**：
  - [实现并运行DataStream应用程序](https://flink.sojb.cn/tutorials/datastream_api.html)
  - [设置本地Flink群集](https://flink.sojb.cn/tutorials/local_setup.html)
- **编程指南**：您可以阅读我们关于[基本API概念](https://flink.sojb.cn/dev/api_concepts.html)和[DataStream API](https://flink.sojb.cn/dev/datastream_api.html)或[DataSet API的指南，](https://flink.sojb.cn/dev/batch/index.html)以了解如何编写您的第一个Flink程序。

## 概念

### 数据流编程模型

#### 抽象层次

Flink 提供不同级别的抽象来开发流/批处理应用程序：

![编程抽象级别](http://flink.apachecn.org/docs/img/levels_of_abstraction.svg)

<div align='center'>Flink 抽象级别</div>

- 状态流：它通过[Process Function](https://flink.sojb.cn/dev/stream/operators/process_function.html)嵌入到[DataStream API中](https://flink.sojb.cn/dev/datastream_api.html)。它允许用户自由处理来自一个或多个流的事件，并使用一致的容错_状态_。此外，用户可以注册事件时间和处理时间回调，允许程序实现复杂的计算。
- Core API：大多数应用程序不需要上述低级抽象，而是针对**Core API**编程， 如[DataStream API](https://flink.sojb.cn/dev/datastream_api.html)（有界/无界流）和[DataSet API](https://flink.sojb.cn/dev/batch/index.html) （有界数据集）。这些流畅的API提供了用于数据处理的通用构建块，例如各种形式的用户指定的转换，连接，聚合，窗口，状态等。在这些API中处理的数据类型在相应的编程语言中表示为类。
- Table API：**Table API**是为中心的声明性DSL _表_，其可被动态地改变的表（表示流时）。 [Table API](https://flink.sojb.cn/dev/table_api.html)遵循（扩展）关系模型：表有一个模式连接（类似于在关系数据库中的表）和API提供可比的 算子操作，如选择，项目，连接，分组依据，聚合等 Table API程序以声明方式定义_应该执行的逻辑 算子操作，_而不是准确指定 _算子操作代码的外观_。虽然 Table API可以通过各种类型的用户定义函数进行扩展，但它的表现力不如_Core API_，但使用更简洁（编写的代码更少）。此外， Table API程序还会通过优化程序，在执行之前应用优化规则。
- SQL：Flink 提供的最高级别抽象是 SQL，在语义和表达方面类似于 `__Table API_`，在[SQL](https://flink.sojb.cn/dev/table_api.html#sql)抽象与 Table API紧密地相互作用，和SQL查询可以通过定义表来执行 _Table API_。

#### 程序和数据流

Flink 程序的基础构建块是流和转换。从概念上讲，流是（可能永无止境的）数据记录流，而转换是将一个或多个流作为一个或多个流的算子操作。

![DataStream程序及其数据流。](http://flink.apachecn.org/docs/img/program_dataflow.svg)

<div align='center'>Flink 数据流</div>

执行时，Flink 程序映射到流数据流，由流和转换算子组成。每个数据流都以一个或多个源开头，并以一个或多个接收器结束。数据流类似于任意有向无环图。通常程序中的转换与数据流中的算子之间存在一对一的对应关系，但是有时一个转换可能包含多个转换算子。

源流和接收器记录在[流连接器](https://flink.sojb.cn/dev/connectors/index.html)和[批处理连接器](https://flink.sojb.cn/dev/batch/connectors.html)文档中。[DataStream 算子](https://flink.sojb.cn/dev/stream/operators/index.html)和[DataSet转换](https://flink.sojb.cn/dev/batch/dataset_transformations.html)中记录了[转换](https://flink.sojb.cn/dev/batch/dataset_transformations.html)。

#### 并行数据流

Flink 中的本质上是并行和分布式的。在执行期间，`_流_`具有一个或多个流分区，并且每个 `_算子_` 具有一个或多个算子子任务。算子子任务彼此独立，并且可以在不同的线程中执行，可能在不同的机器或容器上执行

算子子任务的数量是该特定算子的并行度。流的并行性始终是其生成算子的并行性。同一程序的不同算子可能具有不同的并行级别。

![并行数据流](http://flink.apachecn.org/docs/img/parallel_dataflow.svg)

<div align='center'>并行计算</div>

流可以以 一对一流 模式或 重新分配流 模式在两个算子之间传输数据：

- 一对一流：（例如，在上图中的*Source_和_map（）* 算子之间）保存数据元的分区和排序。这意味着*map（）* 算子的subtask [1] 将以与*Source* 算子的subtask [1]生成的顺序相同的顺序看到相同的数据元
- 重新分配流： （在上面的*map（）_和_keyBy / window_之间，以及 _keyBy / window_和_Sink之间_）重新分配流。每个 _算子子任务_将数据发送到不同的目标子任务，具体取决于所选的转换。实例是 _keyBy（）* （其通过散列Keys重新分区），*广播（）* ，或*Rebalance （）* （其重新分区随机地）。在_重新分配_交换中，数据元之间的排序仅保存在每对发送和接收子任务中（例如，_map（）的_子任务[1] 和子任务[2]_keyBy / window_）。因此，在此示例中，保存了每个Keys内的排序，但并行性确实引入了关于不同Keys的聚合结果到达接收器的顺序的非确定性。

#### 窗口

聚合事件（计算，总和）在流上的工作方式和批处理方式不同。例如，不可能计算流中的所有数据元，因为流通常是无限的。相反，流上的聚合（计算，总和）等由窗口限定，例如“在最后5分钟内计数”或“最后100个数据元的总和”。

window 可以是时间驱动的（例如：每30秒）或数据驱动的（每100个数据元）。

![时间和计数Windows](http://flink.apachecn.org/docs/img/windows.svg)

<div align='center'>窗口</div>

#### 时间

当在流程序中引用时间时，可以参考不同的时间概念：

- 事件时间：创建事件的时间。它通常是由事件中的时间戳描述，例如由生产传感器或生产服务附加。Flink通过时间戳分配器访问事件事件戳
- 摄取时间：事件在源算子处输入 Flink 数据流的时间
- 处理时间：执行基于时间的算子操作的每个算子的本地时间

![事件时间，摄取时间和处理时间](http://flink.apachecn.org/docs/img/event_ingestion_processing_time.svg)

#### 有状态的算子操作

虽然数据流中的许多算子操作只是一次查看一个单独的事件（例如事件解析器），但某些算子操作会记住多个事件（例如窗口算子）的信息，这些算子操作称为有状态

状态算子操作的状态保持在可以被认为是嵌入式键/值存储的状态中。状态被分区并严格地与有状态算子读取的流一起分发

![状态和分区](http://flink.apachecn.org/docs/img/state_partitioning.svg)

#### 容错检查点

Flink 使用 流重放 和 检查点 的组合实现容错，检查点与每个输入流中的特定点以及每个算子的对应状态相关。通过恢复 算子的状态并从检查点重放事件，可以从检查点恢复流数据流，同时保持一致性_（恰好一次处理语义）_。

检查点间隔是在执行期间用恢复时间（需要重放的事件的数量）来折衷容错开销的手段。

[容错内部](https://flink.sojb.cn/internals/stream_checkpointing.html)的描述提供了有关Flink如何管理检查点和相关主题的更多信息。有关启用和配置检查点的详细信息

#### 流处理批处理

Flink执行[批处理程序](https://flink.sojb.cn/dev/batch/index.html)作为流程序的特殊情况，其中流是有界的（有限数量的数据元）。一个_数据集_在内部视为数据流。因此，上述概念以相同的方式应用于批处理程序，并且它们适用于流程序，除了少数例外：

- [批处理程序的容错](https://flink.sojb.cn/dev/batch/fault_tolerance.html)不使用检查点。通过完全重放流来恢复。这是可能的，因为输入有限。这会使成本更多地用于恢复，但使常规处理更便宜，因为它避免了检查点。
- DataSet API中的有状态 算子操作使用简化的内存/核外数据结构，而不是键/值索引。
- DataSet API引入了特殊的同步（超级步骤）迭代，这些迭代只能在有界流上进行。有关详细信息，请查看[迭代文档](https://flink.sojb.cn/dev/batch/iterations.html)。

### 分布式运行环境

#### 任务和算子链

对于分布式执行，Flink _链_ 算子子任务一起放入 _任务_，每个任务由一个线程执行。将算子链接到任务中是一项有用的优化：他可以 Reduce 线程到线程切换和缓冲的开销，并在降低延迟的同时提高整体吞吐量。

![算子链接到任务](http://flink.apachecn.org/docs/img/tasks_chains.svg)

<div align='center'>并行线程</div>

#### TaskManager, JobManager, 客户端

Flink 运行时包含两种类型的进程：

- JobManagers（也称为Masters）：协调分布式执行，它们安排任务，协调检查点，协调故障恢复等
- TaskManagers（也叫 Workers）：执行 _任务_(或者更具体的说：子任务)的数据流，以及缓冲器和交换数据流

JobManagers 和 TaskManagers 可以通过多种方式启动：作为独立集群直接在计算机上，在容器中，由 Yarn 或 Mesos 等资源框架管理。TaskManagers 连接到 JobManagers，宣布自己可用，并被分配工作

![执行Flink数据流所涉及的过程](http://flink.apachecn.org/docs/img/processes.svg)

#### 任务槽和资源

每个 worker（TaskManager）都是一个 JVM 进程，可以在不同的线程中执行一个或多个子任务。为了控制worker接受的任务数量，worker也有一个任务槽（至少一个）

每个任务槽代表 TaskManager 的固定资源子集。例如，具有三个插槽的 TaskManager 将其 1/3 的托管内存专用于每个插槽。切换资源意味着子任务不会与其它作业的子任务竞争托管内存，而是具有一定数量的保存托管内存。这里不会发生 CPU 隔离，当前插槽只分离任务的托管内存。

通过调整任务槽的数量，用户可以定义子任务如何相互隔离。每个TaskManager 有一个插槽意味着每个任务组在一个单独的JVM中运行（例如，可以在一个单独的容器中启动）。拥有多个插槽意味着更多子任务共享同一个 JVM。同一 JVM 的任务共享TCP连接（通过多路复用）和心跳信息。它们还可以共享数据集和数据结构，从而Reduce 任务开销

![具有任务槽和任务的TaskManager](http://flink.apachecn.org/docs/img/tasks_slots.svg)

默认情况下，Flink 允许子任务共享插槽，即使它们是不同任务的子任务，只要它们来自同一个作业。结果是一个槽可以保存作业的整个管道。允许插槽共享有两个主要好处：

- Flink 集群需要与作业中使用的最高并行度一样多的插槽。无需计算程序总共包含多少任务（具有不同的并行性）
- 更容易获得更好的资源利用率。通过插槽共享，将示例中的基本并行性从2增加到6可以充分利用时隙资源，同时确保繁重的子任务在TaskManagers之间公平分配。

![具有共享任务槽的TaskManagers](http://flink.apachecn.org/docs/img/slot_sharing.svg)

根据经验，一个很好的默认任务槽数是 CPU 核心数。使用超线程，每个插槽需要 2 个或更多硬件线程上下文

#### 状态后台

存储键/值索引的确切数据结构取决于所选的[状态后台](https://flink.sojb.cn/ops/state/state_backends.html)。一个状态后台将数据存储在内存中的哈希映射中，另一个状态后台使用[RocksDB](http://rocksdb.org/)作为键/值存储。除了定义保存状态的数据结构之外，状态后台还实现逻辑以获取键/值状态的时间点SNAPSHOT，并将该SNAPSHOT存储为检查点的一部分。

![检查点和SNAPSHOT](http://flink.apachecn.org/docs/img/checkpoints.svg)

#### 保存点

用Data Stream API编写的程序可以从**保存点**恢复执行。保存点允许更新程序和Flink群集，而不会丢失任何状态。

[保存点](https://flink.sojb.cn/ops/state/savepoints.html)是**手动触发的检查点**，它会获取程序的SNAPSHOT并将其写入状态后台。他们依靠常规的检查点机制。在执行期间，程序会定期在工作节点上创建SNAPSHOT并生成检查点。对于恢复，仅需要最后完成的检查点，并且一旦新的检查点完成，就可以安全地丢弃旧的检查点。

保存点与这些定期检查点类似，不同之处在于它们**由用户触发，**并且在较新的检查点完成时**不会自动过期**。可以[从命令行](https://flink.sojb.cn/ops/cli.html#savepoints)或通过[REST API](https://flink.sojb.cn/monitoring/rest_api.html#cancel-job-with-savepoint)取消作业时创建保存点
# BigDataVoyant

### Data Profiling

 A collection of operations and processes for extracting metadata from a given dataset and thus facilitating decision making on its potential utilization: Schema information, Statistics, Samples, Visualizations(tables, charts, graphs, timelines, etc.)

 一组操作和流程，用于从给定的数据集中提取元数据，从而促进对其潜在利用的决策。

### Big Data Assets

Profiling big data assets has attracted a lot of interest over the past decade, however the focus is on relational data, overlooking the specific requireements of geospatial data.

在过去的十年中，对大数据资产的剖析引起了很多人的兴趣，然而重点是关系型数据，忽略了地理空间数据的特殊要求。

Main challenges: 

- the ability to handle input data from heterogeneous sources
- deal with the computational complexity and scalability of profiling functionalities
- interpreting the output profiles, since it typically requires domain expertise and may depend on the use case(eg: exploration, integration, cleansing)

主要挑战：

- 处理来自异质来源的输入数据的能力
- 处理计算的复杂性和剖析功能的可扩展性
- 解释输出概况，因为这通常需要领域的专业知识，并可能取决于使用情况（例如：探索、整合、清理）。

### Geospatial Data Profiling

GIS software platforms already support Exploratory Spatial Data Analysis(ESDA)

GIS 软件平台已经支持探索性空间数据分析

Data profiling in GIS:

- Is not streamlined, but may involve manual execution of a series of operations
- A step-by-step user intervention is necessary: invoke a given functionality(eg: heatmap creation), specify parameters, map rendering options, etc.
- The cost of purchasing a software licemse may also be a hindrance

GIS 中的数据剖析：

- 不是精简的，而是可能涉及一系列操作的手动执行
- 需要一步步的用户干预：调用特定的功能（例如：创建热图），指定参数，地图渲染选项等
- 购买软件许可证的费用

### BigData Voyant

- Open-source, interactive, modular, and extensible framework specifically tailored for in-depth profiling of geospatial data
- Allows data scientists to control its degree of automation:
  - step-by-step mode
  - fully automated mode
- Supports different types of spatial assets in various formats

- 开源的、互动的、模块化的、可扩展的框架，专门为地理空间数据的深入剖析而定制
- 允许数据科学家控制其自动化程度。
  - 循序渐进模式
  - 全自动化模式
- 支持各种格式的不同类型的空间资产

### Types of spatial datasets

Vector:

- Geometry: Point, Line, Polygon
- Attribute data

Raster(Grid of regularly sized pixels):

- Examples:
  - Imagery(satellite or aerial photos)
  - Thematic(DEM)
- Origanized in Bands
  - Interpreted as colors, eg: in RGB color model
  - Specialized interpretation of multispectral bands for scientific observations

Multi-dimensional(Array-based scientific data)

矢量:

- 几何：点、线、多边形
- 属性数据

栅格（规则尺寸的像素网格）。

- 例子：
  - 图像(卫星或航空照片)
  - 专题(DEM)
- 以波段为单位
  - 解释为颜色，例如：在RGB颜色模型中
  - 专门的为科学观测的多光谱波段

多维数据(基于阵列的科学数据)

### Profiling Framework

![image-20220812104751129](C:/Users/wutian/AppData/Roaming/Typora/typora-user-images/image-20220812104751129.png)

<div align='center'>架构图</div>

![image-20220812104852930](C:/Users/wutian/AppData/Roaming/Typora/typora-user-images/image-20220812104852930.png)

<div align='center'>Vector Spactial Datasets</div>

![image-20220812104945298](C:/Users/wutian/AppData/Roaming/Typora/typora-user-images/image-20220812104945298.png)

<div align='center'>Raster Spatial Datasets</div>

![image-20220812105123665](C:/Users/wutian/AppData/Roaming/Typora/typora-user-images/image-20220812105123665.png)

<div align='center'> Multidimensional Spatial Datasets</div>

### Profiler Engine

Leverages various Python libraries to offer an API for automated metadata extraction and manipulation

利用各种Python库，为自动提取和操作元数据提供一个API

![image-20220812105327498](C:/Users/wutian/AppData/Roaming/Typora/typora-user-images/image-20220812105327498.png)

geoVaex:

A library to address scalatility for vector datasets.

Reads and processes spatial files in a memory efficient manner, using Apache Arrow  inter-process communication(IPC) for manipulating big datasets

Once the software reaches a stable level, we plan an extensive empirical study against real-world geodatasets of various types and formats

一个解决矢量数据集的伸缩性的库。

使用Apache Arrow进程间通信(IPC)来操作大数据集，以一种高效的内存方式读取和处理空间文件。

一旦软件达到稳定的水平，我们计划对各种类型和格式的真实世界地理数据集进行广泛的实证研究。

Key features: vaex(spatial methods), GDAL(read spatial files), PyGEOS(multithreading)

###  Ongoing wordk

- Handle vector data with d > 2 dimensions, eg: road segments with linear referencing
- Add extra metadata, eg: examine whether a road network is connected and can support routing
- Examine whether clustering free parameters could be automated calculated in a data-driven manner(according to data characteristics)
- Extend metadata set with the feedback from stakeholders in the geospatial value chain and open source developers
- Complement metadata with more intuitive, interactive visualizations to further improve their cognitive interpretation



- 处理d>2维的矢量数据，例如：具有线性参考的路段
- 添加额外的元数据，例如：检查一个道路网络是否连接并支持路由。
- 考察是否可以以数据驱动的方式（根据数据特征）自动计算聚类的自由参数
- 利用地理空间价值链上的利益相关者和开源开发者的反馈来扩展元数据集
- 用更直观的交互式可视化来补充元数据，以进一步提高其认知解释能力
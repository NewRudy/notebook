现在我们有一些点：

点应该在坐标系上（什么坐标系）

为了别人也能用这些点，人与人之间的交流，需要定义一种规范（OGC)

如何保存到计算机中，肯定要用OGC规范了，常用的工具有：    常常保存的格式有：（格式不同一般就是结构不一样）

讲一个常用的工具：GDAL，准确的说法应该是GDAL/OGC

wkt规范在GDAL中怎么存的

点在GDAL中怎么存的

点可以了，线和面呢





GIS开发主要看OGC规范![imagea](https://www.osgeo.cn/doc_ogcstd/_images/image002.png)

GDAL使用抽象数据模型(abstract data model)来解析它所支持的数据格式，抽象数据模型包括数据集(dataset)，坐标系统，仿射地理坐标转换(Affine Geo Transform)， 大地控制点(GCPs)， 元数据(Metadata)，栅格波段(Raster Band)，颜色表(Color Table)，子数据集域(Subdatasets Domain)，图像结构域(Image_Structure Domain)，XML域(XML:Domains)。

空间矢量数据交换文件由四部分组成：

第一部分为文件头。包含该文件的基本特征数据，如图幅范围、坐标维数、比例尺等。

第二部分为地物类型参数及属性数据结构。地物类型参数包括地物类型代码、地物类型名称、几何类型、属性表名等。属性数据结构包括属性表定义、属性项个数、属性项名、字段描述等。

第三部分为几何图形数据及注记。包含目标标识码、地物类型码、层名、坐标数据等。注记包含了字体、颜色、字型、尺寸、间隔等。

第四部分为属性数据，包含属性表、属性项等。



1.提出3个问题

2.OGC规范（主要讲解WKT）

3.GDAL的介绍和简单操作

4.GeoJSON的介绍

5.结论



GeoJSON支持以下几何类型：`Point`，`LineString`， `Polygon`，`MultiPoint`，`MultiLineString`，和`MultiPolygon`。具有其他属性的几何对象是`Feature`对象。要素集包含在`FeatureCollection`对象中。

GIS抽象数据模型=》OGC?

GDAL基于OGC?

GeoJson基于OGC？



但是我们测量的结果只是一个.dat格式的文件，所以说我们需要格式的转换。

怎么将测量数据真正应用到实际生产中？

数据没有了标准就好比坐标没有了坐标系——没有实用价值了
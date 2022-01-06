# Landsat影像格式

文件说明：https://earth.esa.int/documents/10174/679851/LANDSAT_Products_Description_Document.pdf

## Landsat8

![image-20201105191258884](C:\Users\wutian\AppData\Roaming\Typora\typora-user-images\image-20201105191258884.png)

## 文件名称含义

#### 文件名

![image-20201105192927708](C:\Users\wutian\AppData\Roaming\Typora\typora-user-images\image-20201105192927708.png)

#### 卫星数据类型

LC = Combined（both OLI and TIRS data）

LO = OLI data only .  Pixel Size : OLI Multispectral bands 30 meters ||  OLI panchromatic band 15 meters

LT = TIRS data only  Pixel Size : TIRS Thermal bands: 100 meters (resampled to 30 meters to match                                       multispectral bands 

#### 产品分类Collection Category

Tier1(T1) - 包括最高质量的Level-1 Precision Terrain(L1TP)地面数据，地理坐标已配准，误差满足(RMSE<12m）

Tier2(T2) - 包括没有达到L1TP标准的影像，包括Level-1 Systematic Terrain(L1GT)和Systematic(L1GS)影像

Real-Time(RT) - 尚未进行改正处理的新影像。

## 数据夹文件内容

#### 共14个文件

​    ①文本文件：分为_ANG与_MTL两个

​    _ANG Angle，是对影像的投影方式，等等数据的记录，应该可以拿来做几何

​    _MTL _MeTadata File影像元数据介绍

​    ③影像文件：均为tif格式，11个波段文件，因为Landsat 8 有两个传感器11个波点，一个BQA影像

## 数据处理（gdal）

// 图像信息
gdalinfo [图像路径]

// 图像融合

gdal_merge.py [-o out_filename] [-of out_format] [-co NAME=VALUE]*
              [-ps pixelsize_x pixelsize_y] [-tap] [-separate] [-q] [-v] [-pct]
              [-ul_lr ulx uly lrx lry] [-init "value [value...]"]
              [-n nodata_value] [-a_nodata output_nodata_value]
              [-ot datatype] [-createonly] input_files

// 裁剪
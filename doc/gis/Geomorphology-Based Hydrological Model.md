# Geomorphology-Based Hydrological Model

[Blog](http://hydro.iis.u-tokyo.ac.jp/~hexg/Model/Model_GBHM.html#:~:text=G%20eomorphology-%20B%20ased%20H%20ydrological%20M%20odel,model%20which%20is%20based%20on%20catchment%20geomorphological%20properties.)



NetCDF(network Common Data Form)网络通用数据格式是一种面向数组型并适于网络共享的数据的描述和编码标准。目前，NetCDF广泛应用于**大气科学、水文、海洋学、环境模拟、地球物理**等诸多领域。用户可以借助多种方式方便地管理和操作 NetCDF 数据集。

## 部署与运行

请在 linux 操作系统中进行部署，下面是简要的部署过程，如遇到问题可以联系
李轶博(li-yb16@mails.tsinghua.edu.cn)

1. 准备工作

准备模型和相关文件

```
$ export basedir=/WORK/thu_ztcong_1/gbhm
$ ls $basedir
esmf-intel-mpich3.rc  README.md  src  test
```


2. 安装依赖

安装 MPICH3, hdf5, netcdf-c, netcdf-fortran, esmf，
可以通过源码编译安装，也可通过系统有包管理器安装dev版本。

下面是 esmf 源码安装的步骤，请根据实际情况调整:

```
$ cd $basedir
$ git clone https://github.com/esmf-org/esmf.git
$ cd esmf
$ git checkout ESMF_7_1_0rp1_beta_snapshot_07
$ source $basedir/esmf-intel-mpich3.rc
$ make -j 4
$ make install
```

3. 编译和运行

在 test 目录中有一测试用算例，编译模型后运行该模拟算例。

```
$ source $basedir/esmf-intel-mpich3.rc
$ cd $basedir/src && make
$ cd $basedir/test && mpirun -n 20 $basedir/src/app.exe $basedir/test/config.rc
Clock ----------------------------------
currTime = 
Time -----------------------------------
2010-01-01T00:00:00
end Time -------------------------------

end Clock ------------------------------

Clock ----------------------------------
...
```

4. 结果检查

在 result 文件夹下是结果，包括径流模拟结果和状态变量历史文件。  status.nc 是什么

```
$ tree $basedir/test/result
test/result/
├── discharge.txt
├── hist
│   ├── hist_20100101.nc
│   ├── hist_20100102.nc
│   ├── hist_20100124.nc
│   ├── hist_20100124.nc
│   ├── hist_20100124.nc
...
```

输入数据

pres_201001.nc

rain_201001.nc

rhum_201001.nc

sun_201001.nc

temp_201001.nc

wind_201001.nc



lai

lai_2010.nc


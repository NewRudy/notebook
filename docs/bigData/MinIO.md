# MinIO

[MinIO](http://docs.minio.org.cn/docs/)

MinIO 是兼容 AWS S3 云存储服务的高性能对象存储，为机器学习、分析和应用程序的数据共奏负载构建高性能基础架构

## 服务器



## 部署

MinIO 是一个云原生的应用程序，旨在 [多租户](https://www.redhat.com/zh/topics/cloud-computing/what-is-multitenancy) 环境中以可持续的方式进行扩展。编排（orchestration）平台为 MinIO 的扩展提供了非常好的支撑，MinIO 支持的 orchestration 平台：

| [`Docker Swarm`](http://docs.minio.org.cn/docs/master/deploy-minio-on-docker-swarm) |
| ------------------------------------------------------------ |
| [`Docker Compose`](http://docs.minio.org.cn/docs/master/deploy-minio-on-docker-compose) |
| [`Kubernetes`](http://docs.minio.org.cn/docs/master/deploy-minio-on-kubernetes) |
| [`DC/OS`](http://docs.minio.org.cn/docs/master/deploy-minio-on-dc-os) |

###  为什么说 MinIO 是云原生（cloud-native）的

云原生这个词代表的是一些思想的集合，比如微服务部署，可伸缩，而不是说把一个单体应用改造成容器部署。一个云原生的应用在设计时就考虑了移植性和可伸缩性，而且可以通过简单的复制即可实现水平扩展，现在兴起的编排平台，如 Swarm， Kubernetes 以及 DC/OS，让大规模集群的复制和管理变得前所未有的简单。

容器提供了隔离的应用执行环境，编排平台通过容器管理以及复制功能提供了无缝扩展，MinIO 继承了这些，针对每个租户提供了存储环境的隔离。

MinIO 是建立在云原生的基础上，有纠删码、分布式和共享存储这些特性。MinIO专注于并且只专注于存储，而且做的还不错，它通过编排平台复制一个 MinIO 实例就实现了水平扩展

### 部署的时候碰到的坑

- 9000 和 9001 没有区分开，腾讯云开放了9000的端口，但是我们实际上访问的端口是 9001
- 启动的时候必须指定端口，否则会是一个动态的端口
- docker 和 普通的部署并没有什么大的不同（可能是我是单机的情况）
- 服务器扩容真恶心，碰到了一个问题 lgdisplay 等命令没有响应，reboot 重启并没有什么不同，这个服务器不咋用其实也没啥

## 客户端

MinIO Client (mc)为ls，cat，cp，mirror，diff，find等UNIX命令提供了一种替代方案。它支持文件系统和兼容Amazon S3的云存储服务（AWS Signature v2和v4）。

```
Copyls       列出文件和文件夹。
mb       创建一个存储桶或一个文件夹。
cat      显示文件和对象内容。
pipe     将一个STDIN重定向到一个对象或者文件或者STDOUT。
share    生成用于共享的URL。
cp       拷贝文件和对象。
mirror   给存储桶和文件夹做镜像。
find     基于参数查找文件。
diff     对两个文件夹或者存储桶比较差异。
rm       删除文件和对象。
events   管理对象通知。
watch    监听文件和对象的事件。
policy   管理访问策略。
session  为cp命令管理保存的会话。
config   管理mc配置文件。
update   检查软件更新。
version  输出版本信息
```



## SDKS



## 搭建图床

### 搭建图床成功

[利用Minio搭建私有图床](https://www.naeco.top/2020/08/11/private-oss-for-image/) ，博主写的很好，但是没有用 nginx，好多工具感觉有坑，得慢慢踩，但是现在自己得好好学一些东西了

![image-20211214210550418](http://111.229.14.128:9001/test/1639487150_image-20211214210550418.png)

部署完 minIO 之后需要创建一个自己的桶（bulket），然后设置为 public 下面的代码选择一个放到 typora 即可，建议 python 代码，这个好配置一点

- js 代码

```js
/* 
 * typora插入图片调用此脚本，上传图片到图床
 */

const path = require('path')
// minio for node.js
const Minio = require('minio') 
const { promises } = require('fs')

// 解析参数， 获取图片的路径，有可能是多张图片
const parseArgv = () => {
  const imageList = process.argv.slice(2).map(u => path.resolve(u))
  return imageList
}

// 入口
const uploadImageFile = async (bulkName = 'test', imageList = []) => {
  // 创建连接
  const minioClient = new Minio.Client({
    // 这里填写你的minio后台域名
    endPoint: '111.229.14.128',
    port: 9001,
    useSSL: false,
    // 下面填写你的accessKey和secretKey
    accessKey: 'opengms',
    secretKey: 'opengms517'
  })

  // 开始上传图片
  const metaData = {}
  const tasks = imageList.map(image => {
    return new Promise(async (resolve, reject) => {
      try {
        // 图片重命名，这里采用最简单的，可以根据自己需求重新实现
        const name = `${Date.now()}_${path.basename(image)}`
        // 具体请看Minio的API文档，这里是将图片上传到blog这个bucket上
        const res = await minioClient.fPutObject(bulkName, name, image, metaData)
        resolve(name)
      } catch (err) {
        reject(err)
      }
    })
  })

  const result = await Promise.all(tasks)
  
  // 返回图片的访问链接
  result.forEach(name => {
    const url = `http://111.229.14.128:9001//test/${name}`
    // Typora会提取脚本的输出作为地址，将markdown上图片链接替换掉
    console.log(url)
  })
}

// 执行脚本
uploadImageFile('test', parseArgv())
```

- python 代码：

```python
from minio import Minio
import os
import time
import sys

minio_conf = {
    'endpoint': '111.229.14.128:9001',
    'access_key': 'opengms', 
    'secret_key': 'opengms517', 
    'secure': False
}

def update(bulkName = 'test', fileList = []):
    minioClient = Minio(**minio_conf)
    for filePath in fileList:
        with open(filePath, 'rb') as file:
            name = str(int(time.time())) + '_' + os.path.basename(filePath)
            file_stat = os.stat(filePath)
            minioClient.put_object(bucket_name=bulkName, object_name=name, data=file, length=file_stat.st_size)
            print('http://111.229.14.128:9001/{0}/{1}'.format(bulkName, name))        


if __name__ == '__main__':
    print(sys.argv[1:])
    update('test', sys.argv[1:])
```


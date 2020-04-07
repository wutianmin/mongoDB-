# MongoDB安装

## 下载

4.2版本 windows64

## 配置环境变量

计算机-属性-高级系统设置-环境变量

在path中添加*C:\Program Files\MongoDB\Server\4.2\bin*

## 在c盘根目录下，创建数据库路径

+ 创建文件夹data
+ 在data中创建文件夹db

更改db路径：在cmd窗口中输入***mongod --dbpath 新的数据库路径***

更改端口（默认端口为27017）：在cmd窗口中输入***mongod --dbpath 数据库路径 --port 端口号***

## 启动MongoDB服务器

==（4.2版本开机自启动）==

打开cmd窗口，输入mongod，启动MongoDB服务器，打开后就不要关闭

+ 服务器用来保存数据

## 启动MongoDB

另外打开一个cmd窗口，输入mongo，启动MongoDB客户端

+ 客户端用来操作服务器，对数据进行增删改查的操作


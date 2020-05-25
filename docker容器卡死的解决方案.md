# docker容器卡死的解决方案

## 问题描述
1. 容器container卡死,如调用`docker stop/kill/restart`等卡住，但是`docker run/ps`等命令可以正常进行;具体参见如下图：
   ![docker命令]('./img/docker容器卡死的解决方案/docker命令.JPG')





## 参考
1. [在docker宿主机上查找指定容器内运行的所有进程的PID](https://www.cnblogs.com/devilwind/p/8612329.html)
2. [DOCKER入门，框架原理,镜像制作和资源列表](https://www.jianshu.com/p/04330a3f2ed9)
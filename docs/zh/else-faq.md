# FAQ

#### 单台服务器上是否可以跑多个容器？

只要服务器资源允许，单台服务器上可以运行成百上千个容器

#### 什么是Docker的C/S模式？

Docker安装后，在宿主机（服务器）端会运行一个 Docker Daemon 守护进程，同时也安装一个Docker客户端与这个守护进程通信。但客户端与守护进程是分离的，即可以在任何地方运行客户端，然后通过远程与守护进程通信。
![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/docker/docker-cs-websoft9.png)

#### 是否可以通过SSH连接容器？

在Container上的 console 控制台可以很方便的使用命令操作
![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/docker/portainer/portainer-console-websoft9.png)

#### 一个容器中可以运行多个应用吗？

可以。但是从微服务架构角度看，建议一个容器运行单个应用或单个进程

#### 容器中的服务是否可以被互联网访问？

通过与宿主机（服务器）进行端口映射，实现被互联网用户访问

#### 是否有可视化的 Docker 管理工具？

有，推荐使用 Portainer

#### 是否可以修改 Docker 根目录？

可以，但不建议修改

#### 部署和安装有什么区别？

部署是将一序列软件按照不同顺序，先后安装并配置到服务器的过程，是一个复杂的系统工程。  
安装是将单一的软件拷贝到服务器之后，启动安装向导完成初始化配置的过程。  
安装相对于部署来说更简单一些。 

#### 云平台是什么意思？

云平台指提供云计算服务的平台厂家，例如：Azure,AWS,阿里云,华为云,腾讯云等

#### 实例，云服务器，虚拟机，ECS，EC2，CVM，VM有什么区别？

没有区别，只是不同厂家所采用的专业术语，实际上都是云服务器


+++

title = "云服务器无法使用mstsc解决方法"

date = "2024-07-26"

description = "检查端口占用"

tags = [

 "Windows",

 "mstsc",

]

categories = [

 "实用技术",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240725210353286.png"

+++



> 使用mstsc连接服务器时遇到了问题，检查后发现是3389端口被占用导致的

**排查步骤**

首先阿里云的workbench可以正常使用，但在主机上使用telnet向服务器的3389端口时连接失败
![image-20240725202147085](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240725202147085.png)

登录服务器查看3389端口的使用情况

```shell
netstat -ano |findstr 3389
```

![image-20240725202607593](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240725202607593.png)

其中有三个TCP协议的进程使用3389端口，分别是0.0.0.0:3389、[ : : ] :3389和172.23.40.89:3389（状态是已连接）

0.0.0.0:3389和[ : : ] :3389都是用于mstsc的windows默认接口，分别用来监听外部IPv4和IPv6地址，状态是监听，也就是等待连接

一个外部的47.96.60.109IPv4连接了本地的3389端口。在IPUU上查找47.96.60.109的信息：
<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240725202942158.png" alt="image-20240725202942158" style="zoom:67%;" />

真相大白，阿里云的workbench进程占用了服务器的本地3389端口（即使是在workbench关闭的情况）

------------------



**解决方法**：修改需要连接的服务器的远程桌面服务在本地监听的端口号

[一键修改小工具]: https://www.hws.com/help/news/409.html
[详细方法参考]: https://blog.csdn.net/yxzone/article/details/115964728


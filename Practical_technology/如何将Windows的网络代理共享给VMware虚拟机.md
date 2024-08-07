+++

title = "如何将Windows的网络代理共享给VMware虚拟机"

date = "2024-07-24"

description = "虚拟机使用主机网络代理"

tags = [

 "虚拟机",

 "VPN",

]

categories = [

 "实用技术",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724093853445.png"

+++

## 如何将Windows的网络代理共享给VMware虚拟机

1. Vmware网络适配器选择NAT模式
   <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724092637633.png" alt="image-20240724092637633" style="zoom:50%;" />
2. cmd输入ipconfig，找到VMnet8，记录下IPv4地址
   <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724092837876.png" alt="image-20240724092837876" style="zoom:50%;" />
3. 打开主机网络代理软件，打开允许LAN，记录程序端口
   <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724093021038.png" alt="image-20240724093021038" style="zoom:50%;" />
4. 打开ubuntu网络设置，网络代理选择手动，在http和https后填入VWnet8的IPv4和代理程序端口
   <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724093356163.png" alt="image-20240724093356163" style="zoom:50%;" />
5. 打开控制面板，进入网络适配器设置，找到主机WLAN网络
   <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724093618511.png" alt="image-20240724093618511" style="zoom:50%;" />
6. 在WLAN属性中将internet连接共享给VMnet8
   <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724093853445.png" alt="image-20240724093853445" style="zoom:50%;" />
7. 检查虚拟机网络是否可用
8. 如果不可用，尝试关闭ubuntu防火墙
9. 任然不可用，修改主机防火墙入站规则
   ![image-20240724094540511](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724094540511.png)
   在入站规则中将带有（专用）标注的规则修改为与普通规则一致![image-20240724094710437](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724094710437.png)
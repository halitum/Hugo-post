+++

title = "使用移动设备远程SSH连接你的主机"

date = "2024-07-25"

description = "一个很酷的东西"

tags = [

 "Windows",

 "SSH",

]

categories = [

 "实用技术",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724110856395.png"

+++



## SSH-Serve配置

1. 以管理员身份运行powershell，查看可用的OpenSSH.Server版本

   ```shell
   Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Server*'
   
   ###
   Name  : OpenSSH.Server~~~~0.0.1.0
   State : NotPresent
   ```

2. 安装OpenSSH.Server

   ```shell
    Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
   ```

3. 开启sshd

   ```shell
   Start-Service sshd
   ```

4. 配置 OpenSSH Server 服务为自动启动，并允许SSH通过防火墙

   ```shell
   Set-Service -Name sshd -StartupType 'Automatic'
   New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
   ```

------------------

*主机配置完成*

## FRP配置

5. 由于我的主机没有独立IPV4，又懒得折腾IPv6，因此使用frp对22端口的ssh连接请求反向代理至云服务器上

   ```ini
   # 在frpc.ini中添加以下配置
   [ssh]
   type = tcp
   local_ip = 127.0.0.1
   local_port = 22
   remote_port = 22231
   ```

6. 在主机上启动frpc

   ```shell
   # powershell
   ~\frpc.exe -c ~\frpc.ini
   ```

7. 在云服务器的安全组中为22231端口放行
   ![image-20240724103404317](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724103404317.png)

8. 在服务器上启动frps

   ```shell
   # powershell
   ~\frps.exe -c ~\frps.ini
   ```

------------------------

*FRP配置完成*

## 移动设备配置

8. 在应用商城下载**阿里云**

9. 在阿里云全部工具中选择**SSH工具**

10. 选择添加主机、手动添加主机

    - Host：云服器公网IPv4

    - 登录名：主机用户名

      ```shell
      # powershell查看主机所有用户
      net user
      ```

      <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724104842561.png" alt="image-20240724104842561" style="zoom:67%;" />

    - 密码：需要登录的用户密码

    - 端口号：（remote_port）22231

11. 点击连接

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724110856395.png" alt="image-20240724110856395" style="zoom:50%;" />

------------------



## 扩展内容：为主机设置frpc开机自启动

打开windows应用**任务计划程序**，选择创建基本任务，填入以下配置——

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240724111433429.png" alt="image-20240724111433429" style="zoom:50%;" />

完成后可以修改任务属性，如关闭程序自动退出等。
+++
title = "使用Azure搭建个人节点"
date = "2024-08-07"
description = ""
tags = [
 "VPS",

 "Azure",

 "网络代理",

]
categories = [
 "实用技术",
]
image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240808112356703.png"

+++
通过公有云创建一台虚拟机前，需要先连接公有云的控制层（如Azure中的ARM——Azure resoureces manager）

[**Azure Portal**](https://portal.azure.com/)是网页端的图形化ARM控制台
(除此之外还可使用本地命令行工具Azure CLI、Azure Powershell或在线命令行工具Azure Cloud Shell)

![Azure Portal Home Page](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807095915328.png)

[*Azure官方教程链接*]([在 Azure 门户中开始 - Microsoft Azure](https://portal.azure.com/#view/Microsoft_Azure_Resources/QuickstartTutorialBlade/checklistId/get-started-with-azure/sectionId/get-started-navigating-the-portal/lessonId/get-started-navigating-azure-portal))

注册Azure账号前你还需要办理一张海外信用卡



## 使用Azure创建项目与虚拟机

创建资源组，方便归类资源

![image-20240807101056493](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807101056493.png)

![image-20240807101038318](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807101038318.png)

![image-20240807101635609](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807101635609.png)

资源组创建完成后就可以在里面创建虚拟机了
![image-20240807101849508](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807101849508.png)

![image-20240807102944895](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807102944895.png)

![image-20240807103230904](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807103230904.png)

这里的区域使用East Asia可能会产生一些问题，更换为韩国中部

![image-20240807103423534](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807103423534.png)

在选择大小时一定要在“所有大小”中找到B1s并选择。所有其他大小的虚拟机都会产生额外费用

![image-20240807103545708](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807103545708.png)

磁盘大小更改成P6

![image-20240807103722089](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807103722089.png)

在网络中新建公共IP，将SKU设为基本，分配设为动态。（为了避免静态IP会产生的额外费用）

如果在这一步没有出现动态分配IP的选项，可能是服务器地区选择了East Asia的原因。此时需要重新设定数据中心所在地。

![image-20240807104013335](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807104013335.png)

![image-20240807104336835](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807104336835.png)

点创建后，选择下载SSH私钥

![image-20240807104503449](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807104503449.png)

等待部署完成

![image-20240807104533247](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807104533247.png)

之后在VPS**资源组**中就能查看到刚刚创建的所有资源

![image-20240807104926999](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807104926999.png)

选择VPS-Server-ip

![image-20240807104907412](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807104907412.png)

将IP拷贝到SSH客户端（如Xshell）

![image-20240807105102094](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807105102094.png)

点击连接

![image-20240807105357209](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807105357209.png)

用户名输入`azureuser`

![image-20240807105514992](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807105514992.png)

在密钥中导入之前下载的密钥

> VPS服务器会在接下来的一年里消耗掉免费额度中的\$113（理想情况）

------------------------

*Azure虚拟机VPS配置完成*



## 配置服务端<方法一>使用ShadowsocksR

[Shadowsocksr服务端](https://github.com/ToyoDAdoubiBackup/shadowsocksr)\n
[Shadowsocks客户端](https://shadowsockshelp.github.io/Shadowsocks/download.html)

**配置python环境**
首先安装conda，步骤参考这篇博客[开始使用WSL](http://www.hell-0.xyz/p/开始使用wsl/)
在bash激活conda后创建ss环境

```bash
conda create -n ss python=2.7
```

```bash
conda activate ss
```

```bash
pip install cymysql
```

**安装ShadowsocksR部署脚本**

```bash
sudo apt-get install git
git clone https://github.com/halitum/shadowsocksr.git
```

> 这个仓库失效了就换个别的备份仓库

```bash
cd shadowsocksr
bash initcfg.sh
```

编辑shadowsocksr文件夹下的user_config.json

```bash
nano user-config.json
```

将server_port改为443，password改为自己的密码。
其他参数的配置参考这篇博客：[详细的 ShadowsocksR 服务端搭建教程 | Clash中文教程网](https://clashv2ray.com/22645.html)

**配置MariaDB**(debian官方指定的MySQL替代品)

```bash
sudo apt install mariadb-server
sudo service mariadb start
```

```bash
# 设置 MariaDB 安全脚本
sudo mysql_secure_installation
```

切换到root用户

```bash
sudo su
export PATH="/home/azureuser/miniconda3/bin:$PATH"
source /home/azureuser/miniconda3/etc/profile.d/conda.sh
conda activate ss
```

为ss账户添加MariaDB的localhost权限

```bash
sudo mysql -u root -p
GRANT ALL PRIVILEGES ON *.* TO 'ss'@'localhost' IDENTIFIED BY 'your_password';
FLUSH PRIVILEGES;
SET PASSWORD FOR 'ss'@'localhost' = PASSWORD('new_password');
FLUSH PRIVILEGES;
```

修改db_transfer.py文件

```python
class DbTransfer(TransferBase):
	def __init__(self):
		super(DbTransfer, self).__init__()
		self.user_pass = {} #记录更新此用户流量时被跳过多少次
		self.cfg = {
			"host": "127.0.0.1",
			"port": 3306,
			"user": "ss",
			"password": "pass", # ————————> 改成你自己的MariaDB密码
			"db": "shadowsocks",
			"node_id": 0,
			"transfer_mul": 1.0,
			"ssl_enable": 0,
			"ssl_ca": "",
			"ssl_cert": "",
			"ssl_key": ""}
		self.load_cfg()
```

启动ss服务，并挂到后台

```bash
cd shadowsocks
nohup python server.py &
```

检查是否成功开启

```bash
(ss) root@VPS-Server:/home/azureuser/shadowsocksr/shadowsocks# cat nohup.out
2024-08-07 13:23:15 INFO     util.py:85 loading libcrypto from libcrypto.so.1.1
2024-08-07 13:23:15 INFO     shell.py:72 ShadowsocksR 3.4.0 2017-07-27
2024-08-07 13:23:15 INFO     server.py:46 current process RLIMIT_NOFILE resource: soft 1024 hard 1048576
2024-08-07 13:23:15 INFO     asyncdns.py:324 dns server: [('*****', 53)]
2024-08-07 13:23:15 INFO     server.py:106 server start with protocol[auth_aes128_md5] password [*****] method [aes-128-ctr] obfs [tls1.2_ticket_auth_compatible] obfs_param []
2024-08-07 13:23:15 INFO     server.py:122 starting server at [::]:443
2024-08-07 13:23:15 INFO     server.py:142 starting server at 0.0.0.0:443
```

检查ss进程

```bash
(ss) root@VPS-Server:/home/azureuser/shadowsocksr/shadowsocks# ps aux | grep server.py
root       19586  0.0  2.5  28496 22212 pts/0    S    13:23   0:00 python server.py
root       19863  0.0  0.0   5268   640 pts/0    S+   13:43   0:00 grep server.py
```

------------------------

## 配置服务端<方法二>使用v2ray脚本

使用v2ray自动安装部署脚本（内含广告，赛博洁癖慎用）

```bash
sudo su
bash <(curl -s -L https://git.io/v2ray-setup.sh)
```

命令运行后v2ray自动开始运行，可以：

- `v2ray status` 确认状态
- `v2ray start` 启动一下

按照下面的例子找一个自己能导入的链接：

- `v2ray link` 生成一个json格式的东东
- `v2ray qr` 生成 vmess 二维码、链接
- `v2ray ssqr` 生成 ss 二维码、链接

------------------------------

*服务端配置完成*



## 配置ShadowsocksR客户端

**步骤 1：下载并配置Clash for windows**

{{< download-button file="http://101.200.73.250:80/files/clash0.19.7z" text="clash0.19.7z" >}}

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240807220536454.png" alt="image-20240807220536454" style="zoom:50%;" />

**步骤 2：获取以下 ShadowsocksR 节点信息**

- 服务器地址：`yoursite.com`
- 端口：`443`
- 密码：`password`
- 加密方式：`aes-128-ctr`
- 协议：`auth_aes128_md5`
- 混淆方式：`tls1.2_ticket_auth`

**步骤 3：编写 Clash 配置文件**

Clash 的配置文件通常是 YAML 格式。创建 `MyConfig.yaml` 文件：

```yaml
yamlCopy codeproxies:
  - name: "SSRNode"
    type: ssr
    server: yoursite.com
    port: 443
    password: "password"
    cipher: aes-128-ctr
    protocol: auth_aes128_md5
    obfs: tls1.2_ticket_auth
    protocol-param: ""
    obfs-param: ""
    udp: true

proxy-groups:
  - name: "Proxy"
    type: select
    proxies:
      - "SSRNode"

rules:
  # IP 地址直接连接
  - IP-CIDR,104.21.46.246/32,DIRECT,no-resolve
  - IP-CIDR,104.18.30.226/32,DIRECT,no-resolve
  - IP-CIDR,192.168.0.0/16,DIRECT

  # 特定域名直接连接
  - DOMAIN-SUFFIX,example.com,DIRECT

  # 特定域名通过代理连接
  - DOMAIN-SUFFIX,example.org,Proxy
  - DOMAIN,github.com,Proxy
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy

  # 中国大陆的 IP 地址直连
  - GEOIP,CN,DIRECT

  # 默认策略
  - MATCH,DIRECT
```

**步骤 4：导入配置文件**

- 点击Profile
- 点击Import
- 选择 `config.yaml` 

最后在选择一个代理模式，选择你创建的节点，即可使用

------------------------------

## 配置v2ray客户端

```bash
v2ray qr
```

ios使用shaowrocket搜码即享

桌面端参考[V2Ray客户端使用教程 · xyz690/v2ray Wiki (github.com)](https://github.com/xyz690/v2ray/wiki/V2Ray客户端使用教程)


+++

title = "开始使用WSL"

date = "2024-07-19"

description = "适用于Linux的Windows子系统"

tags = [

 "Linux",

 "WSL",

]

categories = [

 "实用技术",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240730204447816.png"

+++

### 安装WSL并配置conda

1. 在Windows中安装WSL：以管理员身份运行cmd

   ```bash
   wsl --install -d Ubuntu
   #更新wsl：wsl --update
   ```

   之后可通过开始菜单快捷方式或pwershell/cmd输入wsl进入WSL

2. 注册账号密码

   账号默认只能使用小写字母；输入密码时默认不可见（并非无法输入）

   进入后默认以user身份登录，初始目录~为home

3. 下载并安装miniconda

   ```bash
   wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
   ```

   文件会被存放在你当前的工作目录中

   为安装包添加执行权限（否则报Permission denied）

   ```bash
   chmod +x Miniconda3-latest-Linux-x86_64.sh
   ```

   ```bash
   ./Miniconda3-latest-Linux-x86_64.sh
   ```

   按照屏幕上的提示进行安装。通常会问你是否同意许可条款，以及安装位置（可以接受默认设置）。

   默认安装路径：~/miniconda3

   最后一步询问是否在shell 启动时自动初始化 Conda输入yes
   ![image-20240719103309854](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240719103309854.png)

4. 重载shell（bash）配置文件（否则无法使用conda命令）

   ```bash
   source ~/.bashrc
   ```

5. 全局conda换源

   ```bash
   conda config --add default_channels https://conda.virtaicloud.com/repository/anaconda/main
   conda config --add default_channels https://conda.virtaicloud.com/repository/anaconda/r
   conda config --add default_channels https://conda.virtaicloud.com/repository/anaconda/msys2
   ```

   ------------------------

*conda配置完成*

> 扩展内容：登录root用户并使用其他用户安装的conda
>
> ```bash
> sudo su
> ```
>
> ```bash
> export PATH="/home/azureuser/miniconda3/bin:$PATH"
> source /home/azureuser/miniconda3/etc/profile.d/conda.sh
> ```



### 开始使用apt

1. 更新软件包列表

   ```bash
   sudo apt update
   ```

2. 安装所需软件

   ```bash
   sudo apt install tree
   ```



### 在Windows和WSL之间传输文件

> wsl2通过虚拟硬盘文件（VHDX）管理文件，占用C盘空间，向wsl传输大容量文件时要注意C盘爆红问题
>  虚拟硬盘文件的大小会根据存储的数据自动增长，但不会自动缩小。如果你删除了大量数据，可能需要手动收缩 VHDX 文件

1. 使用windows文件资源管理器

![image-20240719103900796](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20240719103900796.png)

2. 使用rsync

   对于更大的文件或需要同步多个文件/目录时，`rsync` 是一个高效的工具：

   > **基本语法**
   >
   > ```bash
   > rsync [options] source destination
   > ```
   >
   > - **source**：源文件或目录的路径。
   > - **destination**：目标文件或目录的路径。
   >
   > **常用选项**
   >
   > - **-a, --archive**：这是最常用的选项之一，用于归档模式，它等同于 `-rlptgoD`（递归、保持符号链接、保持权限、保持时间戳、保持属组、保持所有者、保持设备文件）。
   > - **-v, --verbose**：详细模式，会显示更多信息。
   > - **-z, --compress**：在传输时压缩数据。
   > - **--delete**：在同步时删除目标中存在而源中不存在的文件。
   > - **-r, --recursive**：递归复制所有子目录。
   > - **-u, --update**：仅更新比目标文件更新的源文件。
   > - **--exclude**：排除匹配模式的文件或目录。
   > - **--include**：包括匹配模式的文件或目录。

   在 WSL 终端中运行

   ```bash
   rsync -avz /mnt/c/Users/yourwindowsusername/path/to/source/ ~/destination/
   ```

   *PS：在传输文件时因C盘空间不足导致的传输终端可能会引发wsl控制台报错：”灾难性故障“。此时关闭wsl，对C盘进行系统错误检查、修复驱动文件、重启电脑之后可能会解决问题。*



### 从WSL直接读取Windows中的文件

在 WSL 中，所有 Windows 的文件都被自动挂载在 `/mnt/` 目录下

**注意事项**

1. **文件权限和元数据**：虽然你可以从 WSL 访问 Windows 文件，但是这些文件的 Linux 文件权限和元数据可能不会完全正确。WSL 会尽力模拟这些权限，但可能有限制。
2. **性能考虑**：从WSL访问/mnt/中的文件性能较差
3. **文件锁和同步**：同时从 Windows 和 Linux 访问同一文件时，需要注意文件锁和数据同步问题，避免数据损坏。
4. **路径注意**：在编写脚本或运行自动化任务时，注意路径分隔符的差异（Windows 使用 `\`，而 Linux 使用 `/`）。
+++

title = "Stable Diffusion笔记"

date = "2023-11-13"

description = ""

tags = [

 "Stable-Diffusion",

]

categories = [

 "实用技术",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021232359208.png"

+++

# 小模型微调用法

## 1.Embeddings（文本嵌入）

> *c站中为Textual Inversion*

文件后缀：.pt

### 使用方法

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021230308244.png" alt="image-20231021230308244" style="zoom: 33%;" />

在webUI文生图中prompt栏直接输入

> 常需降低权重（0.9~0.95），控制相关性（11以下）

## 2.LoRa（低秩适应模型）

文件后缀：.checkpoint或.safetensors（与大模型一致）

### 使用方法

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021230934425.png" alt="image-20231021230934425" style="zoom:33%;" />

### （1.）在webUI文生图中prompt栏输入
`<lora:[lora名]:[权重值]>`

有些lora还包含触发提示词，可加入prompt中提升使用效果

权重值：0.5~0.8

（过高影响画风表现）

### (2.)在额外附加选项栏中添加
![image-20231021232359208](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021232359208.png)

改选单可设置

![image-20231021232602035](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021232602035.png)

### （3.)AdditionnalNetwork

![image-20231021232745042](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021232745042.png)

## 3.Hyperwork（超网络）

文件后缀：.pt

### 使用方法

![image-20231021231550462](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021231550462.png)

在设置中添加附加网络

![image-20231021231727623](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231021231727623.png)



# 清晰度提升

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231113225657517.png" alt="image-20231113225657517" style="zoom:200%;" />

## 1.高清修复

> 无法突破现存限制
>
> 可解决提高分辨率后的多人多头问题

### 清晰度设置：

- 放大清晰度：0.3~0.5
- 增加细节：0.5~0.7

### 放大算法

- 使用R-ESRGAN 4x+

  (二次元使用Anime6B)

- 优先使用模型推荐算法



## 2.SD放大

> 可突破现存限制
>
> 分割充会相对不可控
>
> 偶尔会出现额外元素

- 在图生图中选择脚本：使用SD放大

- 像素重叠（拼接缓冲带）：调大解决画面割裂问题

  > > 最终宽高=（设置的宽高-重叠像素）*放大倍率
  > >
  > > 设置像素重叠后要在设置宽高基础上加上该值

- 放大算法使用R-ESRGAN 4x+

### 若SD放大后画面割裂：降低重绘幅度、提高像素重叠



## 3.附加功能放大算法

> 重回幅度为零
>
> 画面提升效果较小

- 算法优先选择R-ESRGAN 4x+，其他维持默认



# 参数配置技巧

## 模型VAE：影像画面画风

## 采样步数

常用10~30（20步以上提升较小）

## 采样方法

优先使用大模型推荐的

否则使用+Karras

## 其他选项

面部修复（选）

平铺（不选）
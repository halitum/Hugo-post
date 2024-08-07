+++

title = "Python基础"

date = "2023-11-21"

description = ""

tags = [

 "Python",

]

categories = [

 "编程语言",

]

image = "https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231208184400735.png"

+++

## 输入输出

1. **print（）**:基础控制台输出

   将（ ）中的内容输入到控制台（console）中

   - 直接输出字符串要使用引号包裹

     ```python
     print("hello, world")
     ```

   - 使用“end”来控制输出结尾（默认以\n（换行）结尾）

     ```python
     print("hello, world"，end="")	#不换行结束
     print("hello, world"，end="*")	#以*作为结尾
     print("hello, world"，end="\t")	#以tab作为结尾
     ```

   - 一次输出多个内容，默认以空格连接。使用sep控制分隔符

     ```python
     print("hell“，”o, w“，”orld")	#使用“ ”分隔
     print("hell“，”o, w“，”orld"，sep="")		#取消分隔
     print("hell“，”o, w“，”orld"，sep="*")		#使用“*”分隔
     ```

   - 括号中直接填入变量名可输出变量的内容

     ```python
     x = 10
     print(x)	#输出10
     ```

   - 使用f·string可选项来组合输出变量和字符串

     ```python
     x = 10
     print(f"x的值是{x}")	#输出为：x的值是10
     ```

   - 使用三连引号一次性输出多行内容

     ```python
     print("""
     这是第一行
     这是第二行
     这是第三行
     """)
     ```

   - 字符串可使用加法和乘法

     加法：效果等同于strcat

     乘法：将字符串输出n次

   - repr()函数：显示字符串在print中打出的形式

     <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-11-25%20213205.png" alt="屏幕截图 2023-11-25 213205" style="zoom:50%;" />

2. **input（）**:基础控制台输入

   首先将（ ）中的内容输入到控制台（console）中，然后向console返回stdin的输入内容

   返回值为一个字符串（可使用强制字符串转换函数变成其他数据类型）

   > 强制类型转换函数如int（）：将（）变量数据类型转变为整形
   >
   > 使用实例：x = int(input(“请输入一个数字”))

   - 一次输入多个内容，默认以空格分隔。使用split控制分隔符

     ```python
     a,b = input("输入两个内容，以逗号分隔").split(",")
     ```

     

## 错误类型

- SyntaxError 语法错误（代码不会运行）
- RuntimeError 崩溃（代码运行中）
- LogicalError 逻辑错误（代码完成运行，但得不到想要的结果）
- TyoeError 类型错误（输入函数的数据类型与预设不符



## 变量

- 使用“=”对变量进行赋值，新赋的值会覆盖旧的值，且无需考虑数据类型

- 多变量赋值：a = b = c = 2     ;       a, b, c = 1, 2, 3

- 变量的作用域：变量的作用是有范围的，超出作用域后变量不可见

  - 函数外部定义的变量具有全局作用域
  - 函数内部定义的变量只能在函数内部起作用，而与其他函数中定义的同名变量无关
  - 在函数内部修改全局变量的值时需要先在函数内部声明该全局变量：global [变量名]

- Python中的变量本质上是一个标签，它的作用是指向数据对应存储位置的内存地址。

  > **is关键字**
  >
  > 原型： a is b -> bool
  >
  > 其中a和b分别指两个变量的变量名，若变量指向的内存地址（对象）一致，则返回布尔True，反之返回False

  > **不可变变量（Immutable）**：值一旦创建就不能改变的数据类型。一旦试图改变这些类型的值时，实际上是创建了一个新的对象，并将标签（变量）指向这个新创建的对象
  >
  > - 整数（int）
  >
  > - 浮点数（float） 
  > - 复数（complex）
  > - 字符串（str）
  > - 元组（tuple）
  > - 冻结集合（frozenset）

  > **可变变量（Mutable）** ：值可以在不更改其引用的情况下被修改的数据类型，对于这些类型，可以修改它们所包含的值而不需要创建一个新的对象
  >
  > - 列表（list） 
  > - 字典（dict）
  > - 集合（set）
  >
  > 对于可变变量，进行连续赋值时，实际上是将多个变量指向了同一个可变对象，其中任一个发生改变，另一个随之发生改变
  >
  > ![image-20231208184400735](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231208184400735.png)


## 函数

- 由两部分构成——header：定义函数的名称与传入的参数名

  ​                         |_body：包含函数执行的操作

- 定义方式：         def functionName([参数]):

  ​                           	  语句1

  ​                                 语句2

  body中语句均需要缩进，当不在缩进表示函数结束

- return返回：结束函数并返回return后的内容

  - 函数中不一定需要包括return（此时函数返回值为None）
  - return可用来返回一个或多个内容
  - return可以返回变量值，也可返回表达式值（表达式需使用括号包裹）

- 函数组合：当出现函数嵌套的情况时，最先执行的是最内侧的函数

- Helper functions：只用来做一件小任务的函数



## 导入库（包括函数，常数值，其它工具）

- import [库名]

  调用第三方库前需使用pip或conda下载（优先使用pip，conda库不全）

- 调用格式：XX.YY（库名.函数名）



## 基础文件操作

- open()：打开文件

  原型：open（file, mode)

  - file：文件名

  - mode：打开模式 -> 决定了文件对象的功能

  - 返回值：文件对象

    > 文件打开模式：
    >
    > r：以字符串形式只读
    >
    > rb：以二进制形式只读
    >
    > w：覆盖写入
    >
    > a：追加写入

- close()：关闭文件



## 数据类型

### 1. 常见内置数据类型

- int

- float（python中的float对应c中的双精度浮点double）

  > 分类：为一个对象赋值，其范围是离散的（对应数据类型int）
  >
  > 回归：预测一个数，其范围是连续的（对应数据类型float）

- bool：布尔值

  python中逻辑判断表达式的返回值数据类型为bool

- type：类型——属于一种”类“的对象

  type有同名函数type()：其返回值为括号内变量的数据类型

  <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/48CA6A277F5B508F2A136B87945E53B0.png" alt="48CA6A277F5B508F2A136B87945E53B0" style="zoom:67%;" />

  <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/AC78D4B181AD69B349D6710246C88B80.png" alt="AC78D4B181AD69B349D6710246C88B80" style="zoom:67%;" />

  ### 2. 其他数据类型

  - str字符串       _

  - list列表            |

  - tuple元祖         }—-底层实现都是arry数组

  - set集合            |

  - dict字典        –

    > 也可以用map映射实现

  - complexNum复数

  - function函数

  - module模块

    

  ### 3.常用内置常数

  - True布尔真
  - False布尔假
  - None空（或空值）

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231121150926089.png" alt="image-20231121150926089" style="zoom:67%;" />



## 选择语句

### 1. if-elif-else

- 相当于c中的if-elseIf-else

### 2. 三元运算符

- 原型：x = **a** if [condition] else **b**

​		三元运算符用来选择性为变量赋值：若conditon为真，则为x赋值a，否则赋值b

### 3. Match-case

- 相当于c中的switch-case

- 用case _ 来代替default（不是必须的）

- 一个case可以设置多个匹配条件，每个条件用  **|**  隔开（ | 相当于or）

- match可以使用多种匹配模式：

  ![](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/15511A7E341905C31F6631E6B13AEDDF.png)



## 循环语句

> **range()**：用来生成一个整数序列，返回的是一个可迭代的“range”对象
>
> range 函数可以有一到三个参数
>
> 1. 一个参数：range(stop)：生成从 0 到 stop-1 的整数序列。
>    eg：range(3) 生成 0, 1, 2。
>
> 2. 两个参数：range(start, stop)：生成从 start 到 stop-1 的整数序列。
>    eg：range(1, 4) 生成 1, 2, 3。
> 3. 三个参数：range(start, stop, step)：生成从 start 开始，以 step 为步长，到 stop-1 结束的序列。
>    eg：range(1, 8, 2) 生成 1, 3, 5, 7。
>
> **ps**：range(a, b)遵循左闭右开

> **in关键字**：用于测试序列（如列表、元组、字符串等）中是否包含某个特定的元素，或者字典中是否包含特定的键，返回值为布尔值
>
> ```python
> # 列表成员资格测试
> if 'apple' in ['banana', 'apple', 'orange']:
>  print("Apple is in the list")
> 
> # 字符串成员资格测试
> if 'a' in 'cat':
>  print("The letter a is in the word cat")
> 
> # 字典键测试
> my_dict = {'name': 'Alice', 'age': 25}
> if 'name' in my_dict:
>  print("Name is one of the keys in the dictionary")
> ```

### 1. for循环语句

for用于遍历序列（如列表、元组、字符串等）或其他可迭代对象的每个元素

```python
# 基础语法
 for element in iterable:
 	pass
```

- element 是变量，它代表 iterable 中当前的元素。
  iterable 是一个序列或任何其他可迭代对象。
- 每次迭代中，element 会被赋值为 iterable 中的下一个元素，然后执行代码块。

```python
# 遍历列表
for number in [1, 2, 3]:
    print(number)

# 遍历字符串
for letter in "hello":
    print(letter)

# 使用 range() 生成数字序列
for i in range(5):
    print(i)
```

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/76A12FB2A0022634EFC38B91E45C9093.png" style="zoom:50%;" />

### 2. while([条件])循环语句

- 当“条件”表达式的返回值为True时执行while中的指令
  - “条件”表达式的返回值可以是任何数据类型，python会将其自动转化成bool值
  - 非零，非空对象都被视为True
- 可以将else语句与while组合使用：当循环条件变为假时执行else块
- 可以用break与continue进行循环控制



## 运算符

### 1. 常见内置运算符

- a**b：a的b次方

- a / b：a对b浮点除，返回值数据类型为float

- a // b：a对b整除，返回值数据类型为int（小数部分舍弃）

- a % b：用b对a取余，返回值数据类型为int，涉及负数时运算结果符号与b一致（与c语言不同，且不符合直觉）

  <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/78A55A74A3145254FDC4C6639E881DD7.png" style="zoom: 67%;" />

  > math库提供了一种符合c语言标准，且处理浮点数时精度更高的取余函数：
  >
  > math.fmod(a,b)：用b对a取余，返回值数据类型为float，涉及负数时运算结果符号与a一致
  >
  > <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/58486CB7903ACD4B8AB69DD0916696B9.png" style="zoom:67%;" />



### 2. 类型影响语义

运算符的效果有时受运算数据的数据类型影响

eg：int + int……相加	;	str + str……strcat（str1，str2）



### 3. 运算符优先级

结合律：加减乘除中从左到右运算，次方运算中从右到左运算

eg：4 - 3 - 2 = (4 - 3) - 2	;	4 ** 3 ** 2 = 4 ** (3 ** 2)	



### 4.逻辑运算符——由布尔值参与运算

- and与运算：有一个F就返回F
- or或运算：有一个T就返回T
- not非运算：T返回F，F返回T

> 短路运算：但逻辑运算符左边的返回值已经足够返回结果了，就不会运行右边的表达式



### 5. 基本数学函数

- abs（）：取绝对值

- max/min（a, b, c,……)：返回最大/最小值

- pow（a, b）：次方运算  <==> a**b

- round(a[, n])：四舍五入到指定小数位

  - a：要四舍五入的数字

  - n：可选，表示要保留的小数位数。默认为0，即四舍五入到最接近的整数

  - 当四舍五入的数字位于中间值时（如对 2.5 进行四舍五入），Python 3 的 round 函数会将其四舍五入到最接近的偶数（所谓的“银行家舍入法”）。这是为了减少因四舍五入导致的累积误差。

  - 由于浮点数误差，round存在精度丢失（如：round(2.354, 2)返回2.36）

    <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/2F0A73A2195A70D007F0EEA5A1D16044.png" alt="2F0A73A2195A70D007F0EEA5A1D16044" style="zoom:67%;" />



## 字符串

### 1. 单个字符索引：将字符串作为字符数组处理

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231121183012376.png" alt="image-20231121183012376" style="zoom:50%;" />

- 索引值可以为负数：此时从字符串最后一位开始记计数

<img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231121183659156.png" alt="image-20231121183659156" style="zoom:50%;" />

> 字符串具有不可变性,因此`s[0] = "H"`是不合法的，会引发 TypeError

### 2. 字符串切片：用来获取字符串的一部分（子串）

使用索引来定义起始和结束的位置：

```python
s = "hello"
print(s[1:4])  # 输出 "ell"，从索引 1 到 3（左闭右开）
```

冒号左右不填入数值则使用默认数值：s[0:-1]

第三个参数：step步长

```python
s = "hello"
print(s[::2])  # 先取走h，再隔一个取l，最后取走0，输出“hlo”
```

应用：翻转字符串

```python
s = "hello"
print(s[::-1])  # 输出“olleh
```

### 3. 字符串相关函数

- enumerate()：枚举函数

  获取一个可迭代对象的“索引-值”键值对

  原型：enumerate（iterable， start=0）

  - iterable：可迭代对象
  - start：计数起始值，默认为0
  - 返回值：键值对（两个元素）

  <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-11-25%20220255.png" alt="屏幕截图 2023-11-25 220255" style="zoom:50%;" />

- zip()：拉链函数

  将多个可迭代对象的元素打包成元祖。特点是可以同时遍历多个对象，适合并行迭代

  原型：zip(iterable1, iterable2, …)

  - iterable：可迭代对象

  - 返回值：一个zip迭代器（每次迭代返回一个元祖）

    <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231125223543906.png" alt="image-20231125223543906" style="zoom:50%;" />

  注意：

  - 如果提供的可迭代对象长度不一致，`zip` 会以最短的那个为准停止迭代

- ord()：返回字符对应ASCLL码

- chr()：返回ASCLL码对应字符

  ![image-20231125223837627](https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231125223837627.png)

- eval()：评估函数

  用于返回字符串中表达式的运行结果

  ```python
  x = 1
  result = eval('x + 1')
  print(result)	# 结果为2
  ```



### 4. 字符串相关方法

- split与join：互为可逆的一对操作（方法）

​		split：将字符串分割并返回列表

​		join：将列表内容合并进字符串中

- str.find

  原型：str.find(sub[, start[, end]])

  - sub：子字符串
  - start：搜索的起始索引，默认为0
  - end：搜索的结束位置索引，默认为-1
  - 返回值：若找到子字符串则返回首次出现的子字符串索引，否则返回-1
  - 注意：
    - find方法严格控制大小写



## 字符串拼接

### 1. 使用加号（`+`）操作符（+=同理）

```py
str1 = "Hello"
str2 = "World"
result = str1 + " " + str2  # 结果是"Hello World"
```

### 2. 使用字符串格式化

- 使用 `%` 操作符：

  ```py
  name = "Alice"
  greeting = "Hello, %s!" % name
  ```

- 使用 `str.format()` 方法：

  ```py
  name = "Alice"
  greeting = "Hello, {}!".format(name)
  ```

- 使用 `f-string`（在Python 3.6及以上版本中可用）：

  ```
  name = "Alice"
  greeting = f"Hello, {name}!"
  ```

### 3. 使用 `join()` 方法

当你需要将多个字符串连接成一个字符串时，`join()`方法是一种高效的方式。它在连接大量字符串时比使用加号更为高效。

```py
words = ["Hello", "World"]
result = " ".join(words)  # 结果是"Hello World"
```

### 4. 使用 StringIO

当需要构建大量字符串时，使用`StringIO`对象可以提高性能。`StringIO`可以被视为在内存中的文件对象，你可以像写文件一样写入字符串，然后获取最终的字符串结果。

```py
from io import StringIO

output = StringIO()
output.write("Hello")
output.write(" ")
output.write("World")
result = output.getvalue()  # 结果是"Hello World"
```



- 简单的拼接
  - `+`、`+=`
  - `f-string`

- 大量的字符串拼接
  - `join()`
  - `StringIO`



## hash与id函数

### hash哈希函数

>哈希值：将任意长度的输入转化成一个固定长度的输出，该输出即为哈希值
>
>注意：
>
>- 对于同一输入，哈希算法总是返回同一个固定的哈希值
>- 从哈希值不能反推出原始输入

原型：hash(obj, /)
用于返回给定对象的哈希值

注意：

- 大多数内置数据类型都是可哈希对象
- 在同一程序运行期间，对象的哈希值不会发生改变
- 不能够手动更改一个对象的哈希值（虽然这么做时解释器不会报错）
- 不能返回一个可变元素的哈希值（如列表）>可变元素：在发生更改后内存地址不变的元素
- python3中哈希的相关变量：
  - 哈希对象的内存地址——进程重启时有时复用原进程地址，有时重新分配
  - 哈希种子——总是在进程重启时重置

>哈希碰撞：有小概率会出现两个不同的对象拥有相同的哈希值，此时称这两个哈希值发生了哈希碰撞。
>因为哈希碰撞的存在，不能从哈希值反推出对象。
>两个对象相同，那么他们的哈希值一定相同，但哈希值相同，对应对象不一定相同

### id函数

用于返回一个对象的特征值（在CPython中这个特征值是对象的内存地址）

>CPython:由c语言实现的python解释器。作为官方实现，CPython是最广泛使用的Python解释器



### assert()：断言函数（用于程序调试）

在括号中的表达式为假时，程序将报错：AssertError

为真时，该条命令等同于pass



## python的空值

### “无形”空

- 缩进：tab（等同于四个空格）
- 转义：在字符串中出现
  - \：\n \t等
  - %：格式控制符

### “有形”空

- None
  NoneType数据类型的唯一值，是一种被定义的空（c中的Null是一个空指针宏，不能等同于python中的None）

  - 用于表示变量没有被赋予任何具体值。
  - 通常用于函数中，表示函数没有明确的返回值。
  - 在对象引用中，用于表示“空”或“不存在”的对象

  <img src="https://typora-picturelib.oss-cn-beijing.aliyuncs.com/image-20231125224903929.png" alt="image-20231125224903929" style="zoom:50%;" />


- pass：表示做一条“啥都不做”的指令，但不等于“不做”
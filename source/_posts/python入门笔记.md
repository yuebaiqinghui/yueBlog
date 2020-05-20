---
date: 2020/5/8 11:32:37
updated: 2020/5/20 16:32:37
categories:
- python
tags:
- python
---

# python入门

闲着也是闲着，不如学点python，反正和js一样都是脚本语言，动态类型的

```python
#TempConvert.py
TempStr = input("请输入带有符号的温度值: ")
if TempStr[-1] in ['F', 'f']:
    C = (eval(TempStr[0:-1]) - 32)/1.8
    print("转换后的温度是{:.2f}C".format(C))
elif TempStr[-1] in ['C', 'c']:
    F = 1.8*eval(TempStr[0:-1]) + 32
    print("转换后的温度是{:.2f}F".format(F))
else:
    print("输入格式错误")
```
```python
#PythonDraw.py
import turtle
turtle.setup(650, 350, 200, 200)
turtle.penup()
turtle.fd(-250)
turtle.pendown()
turtle.pensize(25)
turtle.pencolor("purple")
turtle.seth(-40)
for i in range(4):
    turtle.circle(40, 80)
    turtle.circle(-40, 80)
turtle.circle(40, 80/2)
turtle.fd(40)
turtle.circle(16, 180)
turtle.fd(40 * 2/3)
turtle.done()
```
#### 序号

除了从0开始的正向序号，竟然还有反向的递减序号，最后一个是-1

## 数据类型

**type()**函数判断数据类型

### 数字类型 Number

* 整数 int

  四种进制：十进制；二进制(0b或0B开头)；八进制(0o或0O开头)；十六进制(0x或0X开头)

* 浮点数 float

  也有0.1+0.2的经典问题，可以拿**round()**函数四舍五入一下

  可以使用科学计数法表示：a e b 表示 a*10^b

  比如 4.3e-3 值为 0.0043       9.6E5值为960000.0

* 复数 complex

  z.real实部 z.imag虚部

操作运算符和JS差不多 x//y是除法结果整数，x**y是幂运算

### 字符串 String

* 除了单引号双引号，还可以由一对三单引号或一对三双引号表示
* 有正反向序号
* 字符串除了可以返回序号字符，还能切片
  * 字符串[M:N] 表示返回从序号M到N的字符串(不包括N)
  * M和N可以缺失，表示从开头到N或者从M到结尾
  * 字符串[M:N:K] K是步长，可以每隔K-1个字符截取
  * [::-1]就表示反向字符串
* 操作符：
  * x+y：连接两个字符串
  * n*x：复制n次x字符串
  * x in s: x是s的字串就返回True，否则False
* 函数：
  * len(x)：返回字符串长度
  * str(x)：变成字符串(加个引号)
  * hex(x)或oct(x)：将整数改成十六进制或者八进制的字符串
  * chr(u)：将Unicode编码返回为字符
  * ord(x)：将字符返回Unicode编码
* 方法：
  * str.lower()或str.upper()：返回大写或小写
  * str.split(sep)：根据sep分割字符串，返回数组
  * str.count(sub)：返回sub在str中出现的次数
  * str.replace(old,new)：替换
  * str.center(width,[,fillchar])：根据宽度width居中，fillchar可选，表示填充字符
  * str.strip(chars)：去掉左右两边的所有chars
  * str.join(iter)：用iter连接每个字符
* 格式化：
  * 槽：'{}'.farmat('')  默认序号一一对应，也可以在槽里写序号
  * 


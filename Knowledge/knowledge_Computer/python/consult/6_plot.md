# matplotlib

## 架构

matplotlib是受MATLAB的启发构建的，MATLAB语言是**面向过程**的，利用函数的调用，MATLAB中可以轻松的利用一行命令来绘制直线，然后再用一系列的函数调整结果。

matplotlib通过matplotlib.pyplot模块，完全仿照MATLAB的函数形式，提供了一套绘图接口。这套函数接口方便MATLAB用户过度到matplotlib包。

架构分为以下三层

Scripting (脚本）层
Artist (表现)层。拥有许多可视化元素，如figure、axes、axis等元素。
Backend (后端)层。包含 pyplot 和 pylab 模块（已弃用，不推荐）
它们之间的访问关系是：

Scripting 访问 Artist， Artist 访问 Backend
理论上各层都可以画出相同的图形，但越底层的操作越细节越困难，越高层越易于人机交互，越容易。也就是说上层是下层的封装，把一些不需要打交道的事情封装好，实际画图只关心效果即可。

https://zhuanlan.zhihu.com/p/345523450

matplotlib 实际上就是提供了 3 种API，用于不同的绘图场景

* The pyplot API
* The object-oriented API
* The pylab API (已弃用)

https://zhuanlan.zhihu.com/p/109245779

* 画板figure，画纸Axes/Sublpot画布，可多图绘画
* 画纸上最上方是标题title，用来给图形起名字
* 坐标轴Axis,横轴叫x坐标轴label，纵轴叫y坐标轴ylabel
* 图例Legend 代表图形里的内容
* 网格Grid，图形中的虚线，True显示网格
* 点 Markers：表示点的形状。
* 坐标轴名称(axis label)
* 坐标轴刻度(tick)
* 坐标轴刻度标签(tick label)
* 坐标轴边界（lim）
* 边框（spine）

https://zhuanlan.zhihu.com/p/137677737

## pyplot 简单但封闭

### 导入

```python
import matplotlib.pyplot as plt
```

### 基本绘图逻辑

1，决定基本绘图列表对象

```python
x=[1, 2, 3, 4]
#y坐标轴上点的数值
y1=[1, 4, 9, 16]
y2=[12, 24, 39, 416]
```

2，定义绘图属性

```python
'''
color：线条颜色，值r表示红色（red）
marker：点的形状，值o表示点为圆圈标记（circle marker）
linestyle：线条的形状，值dashed表示用虚线连接各点
'''
plt.plot(x, y1, color='r',marker='o',linestyle='dashed')
plt.plot(x, y2, color='y',marker='o',linestyle='dashed')
#plt.plot(x, y, 'ro')
'''
axis：坐标轴范围
语法为axis[xmin, xmax, ymin, ymax]，
也就是axis[x轴最小值, x轴最大值, y轴最小值, y轴最大值]
'''
plt.axis([0, 6, 0, 20])
```

4， 文本

```python
#添加文本 #x轴文本
plt.xlabel('x坐标轴')
#y轴文本
plt.ylabel('y坐标轴')
#标题
plt.title('标题')
#添加注释 参数名xy：箭头注释中箭头所在位置
#参数名xytext：注释文本所在位置，
#arrowprops在xy和xytext之间绘制箭头, 
#shrink表示注释点与注释文本之间的图标距离

plt.annotate('我是注释', xy=(2,5), xytext=(2, 10),
            arrowprops=dict(facecolor='black', shrink=0.01),
            )
```

5，图例

```python
plt.legend()
```

6， 保存图形

```python
plt.show()
```

### 子图组图

```python
#创建画板1
fig = plt.figure(1) #如果不传入参数默认画板1
#第2步创建画纸，并选择画纸1
#2行1列的子图矩阵，当前是第一个子图
ax1=plt.subplot(2,1,1)   
#在画纸1上绘图
plt.plot([1, 2, 3])
#选择画纸2
ax2=plt.subplot(2,1,2)
#在画纸2上绘图
plt.plot([4, 5, 6])
#显示图像
plt.show()
```

## OOP 复杂但自定义

```python
from matplotlib.figure import Figure
from matplotlib.backends.backend_agg import FigureCanvasAgg as Figure
```

面向对象绘图需要我们一步一步地创建需要的对象，包括基础的Figure，FigureCanvasAgg（画布），所以需要引入两个类: Figure和mpl.backends.backend_agg.FigureCanvasAgg。所以代码看其来要长得多、复杂得多。

# seaborn

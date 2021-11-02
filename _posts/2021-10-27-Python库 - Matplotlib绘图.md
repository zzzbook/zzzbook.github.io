---
layout: post
title: Python库 - Matplotlib绘图
date: 2021-10-27
categories: Python系列
tags: Python

---

# Python库 - Matplotlib绘图

- 视频参考：[【莫烦Python】Matplotlib Python 画图教程](https://www.bilibili.com/video/BV1Jx411L7LU)

## 基础绘图

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211027203203206.png" alt="image-20211027203203206" style="zoom:50%;" />

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211027203440026.png" alt="image-20211027203440026" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

# 使用 numpy 生成坐标点，[-1, 1] 平分 50 个点
x = np.linspace(-1, 1, 50)
# y = 2 * x
y = x**2
# 传入坐标绘图
plt.plot(x, y)
# 使用 show() 显示
plt.show()
```

## Figure 图像

### 开启多个 Figure

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211027204826776.png" alt="image-20211027204826776" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-1, 1, 50)
y1 = 2 * x
y2 = x ** 2

# 以 figure() 开头，下方代码属于同一个 Figure
plt.figure()
plt.plot(x, y1)
# num 为 Figure 标题序号， figsize 调整窗口大小
plt.figure(num=3, figsize=(5, 5))
plt.plot(x, y2)
# 顺序写代码在同一个 Figure 下，color 颜色，linewidth 宽度，linestyle 样式
plt.plot(x, y1, color='red', linewidth=1.0, linestyle='--')

plt.show()
```

### Figure 参数设置

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211027213104046.png" alt="image-20211027213104046" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-2, 2, 50)
y1 = 2 * x
y2 = x ** 2

plt.figure(num=3, figsize=(5, 5))
plt.plot(x, y2)
plt.plot(x, y1, color='red', linewidth=1.0, linestyle='--')
# 修改坐标范围
plt.xlim((-1, 2))
plt.ylim((-2, 3))
# 设置坐标描述标签
plt.xlabel('I am X')
plt.ylabel('I am Y')
# 设置坐标点
x_ticks = np.linspace(-1, 2, 5)
plt.xticks(x_ticks)
# 设置坐标点对应文本，可以使用 LateX 数学公式
plt.yticks([-2, -1.8, 0, 1.22, 3],
           [r'$label -2\ \alpha$', 'label -1.8', 'label 0', 'label 1.22', 'label 3'])
# 获取图像边框参数
ax = plt.gca()
# 设置上右边框为空
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
# 设置横纵轴为对应边框
ax.xaxis.set_ticks_position('bottom')
ax.yaxis.set_ticks_position('left')
# 移动横纵轴
ax.spines['bottom'].set_position(('data', 0))
ax.spines['left'].set_position(('data', 0))

plt.show()
```

## Legend 图例

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211027215359235.png" alt="image-20211027215359235" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-2, 2, 50)
y1 = 2 * x
y2 = x ** 2

plt.figure(num=3, figsize=(5, 5))
line1, = plt.plot(x, y2, label='up')
line2, = plt.plot(x, y1, color='red', linestyle='--', label='down')
# handles 图例线段，传入 plot 返回值
# labels 新标签值，替换原标签
# loc 图例位置，best 最佳位置，upper right 右上角...
plt.legend(handles=[line1, line2], labels=['new up label', 'new down label'], loc='best')

plt.show()
```

## Annotation 标注

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211030092819030.png" alt="image-20211030092819030" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-1, 1, 25)
y = 2 * x + 1

plt.figure()
plt.plot(x, y)
# ...
x0 = 0.5
y0 = 2 * x0 + 1
# 绘制垂直虚线
plt.plot([x0, x0], [y0, 0], 'k--', linewidth=2.5)
# scatter 绘制点
plt.scatter(x0, y0)
# 绘制标注，xy 坐标点，xycoords 坐标点类型， xytext 文本偏移， textcoords 偏移类型， arrowprops 剪头类型
plt.annotate(r'$2x+1=%s$' % y0, xy=(x0, y0), xycoords='data', xytext=(+30, -30), textcoords='offset points',
             fontsize=16, arrowprops=dict(arrowstyle='->', connectionstyle='arc3,rad=.5'))

# 绘制文字，坐标、文本、文字样式
plt.text(-1, 1, r'$this\ is\ label\ test\ \sigma_i\ \alpha_t$', fontdict={'size': 12, 'color': 'r'})

plt.show()
```

## tick 坐标轴标签

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211030094216636.png" alt="image-20211030094216636" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-1, 1, 25)
y = 2 * x + 1
# ...
# 遍历标签
for label in ax.get_xticklabels() + ax.get_yticklabels():
    # 标签文字大小
    label.set_fontsize(12)
    # 标签背景设置，facecolor 背景颜色，edgecolor 边框颜色，alpha 透明度
    label.set_bbox(dict(facecolor='r', edgecolor='b', alpha=0.4))

plt.show()
```

## Scatter 散点图

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211030095440769.png" alt="image-20211030095440769" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

n = 1024
x = np.random.normal(0, 1, n)
y = np.random.normal(0, 1, n)
# 通过 tan 生成颜色
c = np.arctan2(y, x)
# 绘制散点图，s 大小，c 颜色，alpha 透明度
plt.scatter(x, y, s=50, c=c, alpha=0.5)
# 坐标范围
plt.xlim((-1.5, 1.5))
plt.ylim((-1.5, 1.5))
# 隐藏坐标标签
plt.xticks(())
plt.yticks(())

plt.show()
```

## Bar 柱状图

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211030104023217.png" alt="image-20211030104023217" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

n = 12
X = np.arange(n)
y1 = (1 - X / float(n)) * np.random.uniform(0.5, 1, n)
y2 = (1 - X / float(n)) * np.random.uniform(0.5, 1, n)
# bar 绘制柱状图
plt.bar(X, y1, facecolor='#9999ff', edgecolor='white')
plt.bar(X, -y2, facecolor='#ff9999', edgecolor='white')
# zip 组合遍历
for x, y in zip(X, y1):
    # ha 横向居中方式，va 纵向居中方式
    plt.text(x, y + 0.05, '%.2f' % y, ha='center', va='bottom')

for x, y in zip(X, y2):
    plt.text(x, -y - 0.05, '-%.2f' % y, ha='center', va='top')

plt.show()
```

## Contours 等高线图

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102183828065.png" alt="image-20211102183828065" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

# 生成热力图坐标
def f(x, y):
    return (1 - x / 2 + x ** 5 + y ** 3) * np.exp(-x ** 2 - y ** 2)

n = 256
x = np.linspace(-3, 3, n)
y = np.linspace(-3, 3, n)
X, Y = np.meshgrid(x, y)
# 绘制热力图，传入坐标、分层数为 n+2、透明度。颜色 map
plt.contourf(X, Y, f(X, Y), 6, alpha=0.75, cmap=plt.cm.hot)
# 绘制等高线
C = plt.contour(X, Y, f(X, Y), 6, colors='black', linewidths=.5)
# 绘制等高线标签，inline 文字在等高线上方
plt.clabel(C, inline=True, fontsize=10)

plt.xticks(())
plt.yticks(())
plt.show()
```

## Image 图片

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102213926421.png" alt="image-20211102213926421" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

a = np.array(
    [0.313660827978, 0.365348418405, 0.423733120134,
     0.365348418405, 0.439599930621, 0.525083754405,
     0.423733120134, 0.525083754405, 0.651536351379]).reshape(3, 3)
# interpolation 模糊样式
# origin 正向 upper 反向 lower
plt.imshow(a, interpolation='nearest', cmap='bone', origin='lower')
# shrink 压缩
plt.colorbar(shrink=0.9)

plt.xticks(())
plt.yticks(())
plt.show()
```

## 3D 图像

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102220927746.png" alt="image-20211102220927746" style="zoom:67%;" />

```python
import matplotlib.pyplot as plt
import numpy as np
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = Axes3D(fig)

X = np.arange(-4, 4, 0.25)
Y = np.arange(-4, 4, 0.25)
X, Y = np.meshgrid(X, Y)
R = np.sqrt(X ** 2 + Y ** 2)
Z = np.sin(R)
# rstride 横线跨度 cstride 纵线跨度
ax.plot_surface(X, Y, Z, rstride=2, cstride=2, edgecolor='black', cmap=plt.get_cmap('rainbow'))
# 投影绘制 zdir 投影方向 offset 偏移坐标
ax.contourf(X, Y, Z, zdir='z', offset=-2, cmap=plt.get_cmap('rainbow'))

ax.set_zlim(-2, 2)
plt.show()
```

## Subbplot 分格显示

### 方法一

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102221849172.png" alt="image-20211102221849172" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt

plt.figure()

plt.subplot(2, 1, 1)
plt.plot([0, 1], [0, 1])
plt.subplot(2, 3, 4)
plt.plot([0, 1], [0, 1])
plt.subplot(2, 3, 5)
plt.plot([0, 1], [0, 1])
plt.subplot(2, 3, 6)
plt.plot([0, 1], [0, 1])

plt.show()
```

### 方法二

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102222602150.png" alt="image-20211102222602150" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt

plt.figure()
# 3行3列，起点0开始，colspan 横向跨度，rowspan 纵向跨度
ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
ax2 = plt.subplot2grid((3, 3), (1, 0), colspan=2)
ax3 = plt.subplot2grid((3, 3), (1, 2), rowspan=2)
ax4 = plt.subplot2grid((3, 3), (2, 0))
ax5 = plt.subplot2grid((3, 3), (2, 1))

plt.show()
```

### 方法三

```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec

plt.figure()
# 切片的方式 :表示占据后方空余
gs = gridspec.GridSpec(3, 3)
ax1 = plt.subplot(gs[0, :])
ax2 = plt.subplot(gs[1, :2])
ax3 = plt.subplot(gs[1:, 2])
ax4 = plt.subplot(gs[-1, 0])
ax5 = plt.subplot(gs[-1, -2])

plt.show()
```

### 方法四

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102223156371.png" alt="image-20211102223156371" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt

# f = figure，后面为对应图，可绘制方格
f, ((ax11, ax12), (ax21, ax22)) = plt.subplots(2, 2, sharex=True, sharey=True)

plt.show()
```

## 图中图

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102223820573.png" alt="image-20211102223820573" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt

fig = plt.figure()
x = [1, 2, 3, 4, 5, 6, 7]
y = [1, 3, 4, 2, 5, 8, 6]

left, bottom, width, height = 0.1, 0.1, 0.8, 0.8
ax1 = fig.add_axes([left, bottom, width, height])
ax1.plot(x, y, 'r')
# 方法一 添加 axes
left, bottom, width, height = 0.2, 0.6, 0.25, 0.25
ax2 = fig.add_axes([left, bottom, width, height])
ax2.plot(x, y, 'b')
# 方法二 直接绘制 axes
plt.axes([.6, .2, .25, .25])
plt.plot(y[::-1], x, 'g')

plt.show()
```

## 次坐标轴

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/image-20211102224138330.png" alt="image-20211102224138330" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 10, 0.1)
y1 = 0.05 * x ** 2
y2 = -1 * y1

fig, ax1 = plt.subplots()
# 镜面
ax2 = ax1.twinx()
ax1.plot(x, y1, 'g')
ax2.plot(x, y2, 'b')

plt.show()
```

## Animation 动画

<img src="http://r0ykjq69u.hb-bkt.clouddn.com/animation.gif" alt="animation" style="zoom:50%;" />

```python
import matplotlib.pyplot as plt
from matplotlib import animation
import numpy as np

fig, ax = plt.subplots()
x = np.arange(0, 2 * np.pi, 0.01)
line, = ax.plot(x, np.sin(x))

# 动画函数
def animate(i):
    line.set_ydata(np.sin(x + i / 10))
    return line,

# 初始化函数
def init():
    line.set_ydata(np.sin(x))
    return line,

# frames 帧数/时间点；interval 更新间隔时间；blit 是否更新整个画面
ani = animation.FuncAnimation(fig=fig, func=animate, frames=100, init_func=init, interval=20, blit=True)

plt.show()
```


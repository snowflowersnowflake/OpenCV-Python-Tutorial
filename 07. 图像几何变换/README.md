# [OpenCV-Python教程07：图像几何变换](http://ex2tron.wang/opencv-python-image-geometric-transformation/)

![](http://pic.ex2tron.top/cv2_perspective_transformations_inm.jpg)

学习如何旋转、平移、缩放和翻转图片。<!-- more -->图片等可到[源码处](#引用)下载。

---

## 目标

- 实现旋转、平移和缩放图片
- OpenCV函数：`cv2.resize()`, `cv2.flip()`, `cv2.warpAffine()`

## 教程

> 图像的几何变换从原理上看主要包括两种：基于2×3矩阵的仿射变换（平移、缩放、旋转和翻转等）、基于3×3矩阵的透视变换，感兴趣的小伙伴可参考[番外篇：仿射变换与透视变换](/opencv-python-extra-warpaffine-warpperspective/)。

### 缩放图片

缩放就是调整图片的大小，使用`cv2.resize()`函数实现缩放。可以按照比例缩放，也可以按照指定的大小缩放：

```python
import cv2

img = cv2.imread('drawing.jpg')

# 按照指定的宽度、高度缩放图片
res = cv2.resize(img, (132, 150))
# 按照比例缩放，如x,y轴均放大一倍
res2 = cv2.resize(img, None, fx=2, fy=2, interpolation=cv2.INTER_LINEAR)

cv2.imshow('shrink', res), cv2.imshow('zoom', res2)
cv2.waitKey(0)
```

我们也可以指定缩放方法`interpolation`，更专业点叫插值方法，默认是`INTER_LINEAR`，全部可以参考：[InterpolationFlags](https://docs.opencv.org/4.0.0/da/d54/group__imgproc__transform.html#ga5bb5a1fea74ea38e1a5445ca803ff121)

### 翻转图片

镜像翻转图片，可以用`cv2.flip()`函数：

```python
dst = cv2.flip(img, 1)
```

其中，参数2 = 0：垂直翻转(沿x轴)，参数2 > 0: 水平翻转(沿y轴)，参数2 < 0: 水平垂直翻转。

![](http://pic.ex2tron.top/cv2_flip_image_sample.jpg)

### 平移图片

要平移图片，我们需要定义下面这样一个矩阵，tx,ty是向x和y方向平移的距离：

<div class="MathJax_Display" style="text-align: center;"><span class="MathJax" id="MathJax-Element-1-Frame" tabindex="0" style="text-align: center; position: relative;" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><mi>M</mi><mo>=</mo><mrow><mo>[</mo><mtable rowspacing=&quot;4pt&quot; columnspacing=&quot;1em&quot;><mtr><mtd><mn>1</mn></mtd><mtd><mn>0</mn></mtd><mtd><msub><mi>t</mi><mi>x</mi></msub></mtd></mtr><mtr><mtd><mn>0</mn></mtd><mtd><mn>1</mn></mtd><mtd><msub><mi>t</mi><mi>y</mi></msub></mtd></mtr></mtable><mo>]</mo></mrow></math>" role="presentation"><nobr aria-hidden="true"><span class="math" id="MathJax-Span-1" style="width: 9.169em; display: inline-block;"><span style="display: inline-block; position: relative; width: 7.614em; height: 0px; font-size: 120%;"><span style="position: absolute; clip: rect(1.725em, 1007.39em, 4.558em, -999.997em); top: -3.386em; left: 0em;"><span class="mrow" id="MathJax-Span-2"><span class="mi" id="MathJax-Span-3" style="font-family: MathJax_Math-italic;">M<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.058em;"></span></span><span class="mo" id="MathJax-Span-4" style="font-family: MathJax_Main; padding-left: 0.281em;">=</span><span class="mrow" id="MathJax-Span-5" style="padding-left: 0.281em;"><span class="mo" id="MathJax-Span-6" style="vertical-align: 0em;"><span style="font-family: MathJax_Size3;">[</span></span><span class="mtable" id="MathJax-Span-7" style="padding-right: 0.169em; padding-left: 0.169em;"><span style="display: inline-block; position: relative; width: 3.836em; height: 0px;"><span style="position: absolute; clip: rect(2.447em, 1000.45em, 4.892em, -999.997em); top: -3.997em; left: 0em;"><span style="display: inline-block; position: relative; width: 0.503em; height: 0px;"><span style="position: absolute; clip: rect(3.169em, 1000.45em, 4.169em, -999.997em); top: -4.719em; left: 50%; margin-left: -0.219em;"><span class="mtd" id="MathJax-Span-8"><span class="mrow" id="MathJax-Span-9"><span class="mn" id="MathJax-Span-10" style="font-family: MathJax_Main;">1</span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; clip: rect(3.169em, 1000.45em, 4.169em, -999.997em); top: -3.275em; left: 50%; margin-left: -0.219em;"><span class="mtd" id="MathJax-Span-19"><span class="mrow" id="MathJax-Span-20"><span class="mn" id="MathJax-Span-21" style="font-family: MathJax_Main;">0</span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; clip: rect(2.447em, 1000.45em, 4.892em, -999.997em); top: -3.997em; left: 1.503em;"><span style="display: inline-block; position: relative; width: 0.503em; height: 0px;"><span style="position: absolute; clip: rect(3.169em, 1000.45em, 4.169em, -999.997em); top: -4.719em; left: 50%; margin-left: -0.219em;"><span class="mtd" id="MathJax-Span-11"><span class="mrow" id="MathJax-Span-12"><span class="mn" id="MathJax-Span-13" style="font-family: MathJax_Main;">0</span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; clip: rect(3.169em, 1000.45em, 4.169em, -999.997em); top: -3.275em; left: 50%; margin-left: -0.219em;"><span class="mtd" id="MathJax-Span-22"><span class="mrow" id="MathJax-Span-23"><span class="mn" id="MathJax-Span-24" style="font-family: MathJax_Main;">1</span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; clip: rect(2.503em, 1000.84em, 5.169em, -999.997em); top: -3.997em; left: 3.003em;"><span style="display: inline-block; position: relative; width: 0.836em; height: 0px;"><span style="position: absolute; clip: rect(3.225em, 1000.84em, 4.336em, -999.997em); top: -4.719em; left: 50%; margin-left: -0.442em;"><span class="mtd" id="MathJax-Span-14"><span class="mrow" id="MathJax-Span-15"><span class="msubsup" id="MathJax-Span-16"><span style="display: inline-block; position: relative; width: 0.836em; height: 0px;"><span style="position: absolute; clip: rect(3.225em, 1000.34em, 4.169em, -999.997em); top: -3.997em; left: 0em;"><span class="mi" id="MathJax-Span-17" style="font-family: MathJax_Math-italic;">t</span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; top: -3.831em; left: 0.336em;"><span class="mi" id="MathJax-Span-18" style="font-size: 70.7%; font-family: MathJax_Math-italic;">x</span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; clip: rect(3.225em, 1000.78em, 4.447em, -999.997em); top: -3.275em; left: 50%; margin-left: -0.386em;"><span class="mtd" id="MathJax-Span-25"><span class="mrow" id="MathJax-Span-26"><span class="msubsup" id="MathJax-Span-27"><span style="display: inline-block; position: relative; width: 0.781em; height: 0px;"><span style="position: absolute; clip: rect(3.225em, 1000.34em, 4.169em, -999.997em); top: -3.997em; left: 0em;"><span class="mi" id="MathJax-Span-28" style="font-family: MathJax_Math-italic;">t</span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span><span style="position: absolute; top: -3.831em; left: 0.336em;"><span class="mi" id="MathJax-Span-29" style="font-size: 70.7%; font-family: MathJax_Math-italic;">y<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.003em;"></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span></span><span style="display: inline-block; width: 0px; height: 4.003em;"></span></span></span></span><span class="mo" id="MathJax-Span-30" style="vertical-align: 0em;"><span style="font-family: MathJax_Size3;">]</span></span></span></span><span style="display: inline-block; width: 0px; height: 3.392em;"></span></span></span><span style="display: inline-block; overflow: hidden; vertical-align: -1.263em; border-left: 0px solid; width: 0px; height: 3.137em;"></span></span></nobr><span class="MJX_Assistive_MathML MJX_Assistive_MathML_Block" role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mi>M</mi><mo>=</mo><mrow><mo>[</mo><mtable rowspacing="4pt" columnspacing="1em"><mtr><mtd><mn>1</mn></mtd><mtd><mn>0</mn></mtd><mtd><msub><mi>t</mi><mi>x</mi></msub></mtd></mtr><mtr><mtd><mn>0</mn></mtd><mtd><mn>1</mn></mtd><mtd><msub><mi>t</mi><mi>y</mi></msub></mtd></mtr></mtable><mo>]</mo></mrow></math></span></span></div>

平移是用仿射变换函数`cv2.warpAffine()`实现的：

```python
# 平移图片
import numpy as np

rows, cols = img.shape[:2]

# 定义平移矩阵，需要是numpy的float32类型
# x轴平移100，y轴平移50
M = np.float32([[1, 0, 100], [0, 1, 50]])
# 用仿射变换实现平移
dst = cv2.warpAffine(img, M, (cols, rows))

cv2.imshow('shift', dst)
cv2.waitKey(0)
```

![](http://pic.ex2tron.top/cv2_translation_100_50.jpg)

### 旋转图片

旋转同平移一样，也是用仿射变换实现的，因此也需要定义一个变换矩阵。OpenCV直接提供了 `cv2.getRotationMatrix2D()`函数来生成这个矩阵，该函数有三个参数：

- 参数1：图片的旋转中心
- 参数2：旋转角度(正：逆时针，负：顺时针)
- 参数3：缩放比例，0.5表示缩小一半

```python
# 45°旋转图片并缩小一半
M = cv2.getRotationMatrix2D((cols / 2, rows / 2), 45, 0.5)
dst = cv2.warpAffine(img, M, (cols, rows))

cv2.imshow('rotation', dst)
cv2.waitKey(0)
```

![逆时针旋转45°并缩放](http://pic.ex2tron.top/cv2_rotation_45_degree.jpg)


## 小结

- `cv2.resize()`缩放图片，可以按指定大小缩放，也可以按比例缩放。
- `cv2.flip()`翻转图片，可以指定水平/垂直/水平垂直翻转三种方式。
- 平移/旋转是靠仿射变换`cv2.warpAffine()`实现的。

## 接口文档

- [cv2.resize()](https://docs.opencv.org/4.0.0/da/d54/group__imgproc__transform.html#ga47a974309e9102f5f08231edc7e7529d)
- [cv2.filp()](https://docs.opencv.org/4.0.0/d2/de8/group__core__array.html#gaca7be533e3dac7feb70fc60635adf441)
- [cv2.warpAffine()](https://docs.opencv.org/4.0.0/da/d54/group__imgproc__transform.html#ga0203d9ee5fcd28d40dbc4a1ea4451983)
- [cv2.getRotationMatrix2D()](https://docs.opencv.org/4.0.0/da/d54/group__imgproc__transform.html#gafbbc470ce83812914a70abfb604f4326)

## 引用

- [本节源码](https://github.com/ex2tron/OpenCV-Python-Tutorial/tree/master/07.%20%E5%9B%BE%E5%83%8F%E5%87%A0%E4%BD%95%E5%8F%98%E6%8D%A2)
- [Geometric Transformations of Images](http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_geometric_transformations/py_geometric_transformations.html)

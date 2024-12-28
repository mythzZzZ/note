一些常见opencv原理

https://zhuanlan.zhihu.com/p/404807319





tensorRT

https://zhuanlan.zhihu.com/p/371239130

- TensorRT是可以在**NVIDIA**各种**GPU硬件平台**下运行的一个**C++推理框架**
- 快的原因
  - 算子融合
  - 减枝
- 低精度量化
  - 8-bit 低精度推理中， 我们将一个原本 FP32 的 weight/activation 浮点数张量转化成一个 int8/uint8 张量来处理，8bit占用的字节数更少占用的内存就更少了

- 部署到一些英伟达GPU开发板
- 对于同一输入的多个分支可以进行并行运算











openVINO

https://zhuanlan.zhihu.com/p/91882515

- 在Inter CPU快速部署套件

  

opencv常见函数

https://blog.csdn.net/qq_33952811/article/details/103219555

- 读取：imread()
- 显示：imshow()
- 保存：imwrite()
- 窗口：namedWindow()
- 线：line()
- 矩形：rectangle()
- 圆：circle()
- 多边形：polylines()
- 显示文字：putText()
- 颜色空间转换：cvtColor()
- 判断像素值是否在某区间：inrange()
- 扩展缩放：resize()
- 仿射变换：warpAffine()
- 旋转辅助函数：getRotationMatrix2D()
- 透视变换：getPerspectiveTransform(),warpPerspective()



## 为什么要将图像灰度化

灰度图像相对于彩色图像在存储和计算上具有更高的效率（三维变成一维）





## 图像的梯度是什么

图像梯度是衡量图像灰度变化率的一种方法

## 高斯滤波器的原理

高斯滤波器的原理

- 将构建好的高斯核应用于图像上的每个像素，通过卷积操作计算新像素值。具体地，将高斯核与图像的局部区域进行对应位置的乘法，并将结果求和，作为新像素值。这样做可以使图像中每个像素点的值都受到其周围像素的影响，从而实现平滑处理。
- 高斯滤波器的卷积操作导致了图像中每个像素点的值都受到周围像素的**加权影响**，使得图像中噪声被平滑掉，从而**降低了图像的变化率**。高斯滤波器可以有效地去除高频噪声，并且保留图像的整体特征。



## SIFT特征的匹配

SIFT图像拼接

- 原理：提取每一张图像的特征点，计算特征点之间的距离（欧氏距离）来进行匹配
- 这个特征的是如何获取的 （1.获取关键点 2.筛选关键点 3.给关键点加一个位置 4.在关键点周围计算描述符）
  - 1.在不同的尺度下使用==高斯滤波器构建尺度空间金字塔==,这个金字塔在尺度维度上包含了多个高斯模糊的版本的图像。然后，在==每个尺度空间中检测图像的极值点==
  - 2.对检测到的极值点进行筛选（位置，对比度，极值点是否稳定）
    - 定位是如何实现的？比如在极值点周围的像素上进行拟合或使用差分高斯函数来确定精确的关键点位置
  - 3.为每个关键点分配一个主导方向，以确保关键点描述符具有旋转不变性。通常使用图像局部梯度的方向信息来计算关键点的主导方向
  - 4.在关键点周围的邻域内计算描述符。描述符是一个向量，其中包含了关键点周围区域的局部特征信息。在 SIFT 中，描述符通常是通过计算关键点周围区域的梯度直方图并将其转化为一个向量来实现的。这个向量是通过对图像的局部梯度方向进行统计来生成的，具有一定的尺度不变性和旋转不变性。



## HOG梯度直方图

https://blog.csdn.net/liulina603/article/details/8291093

它通过计算和统计图像局部区域的梯度方向直方图来构成特征

- 灰度化
- Gamma校正
- 计算每个像素梯度值
- 快正则化：把图像划分成多个块，每一个块多个cell，对每一个块进行归一化
  - 梯度值存在一个问题，就是和图像亮度有强依赖关系，对数据进行正则化
- 使用正则化后的的每一个块串接起来构建一个大向量得到HOG特征向量

## Cannny算子

原理：通过增强边缘轮廓来实现检测边缘

- 把图像转化成灰度图，去除噪声，噪声就是灰度变化很大的地方容易识别成伪边缘
- 计算图像梯度，得到可能边缘。计算图像梯度能够得到图像的边缘，因为梯度是灰度变化明显的地方，这样得到所有可能是边缘的集合。
- 非极大值抑制，灰度变化最大的保留下来
- 双阈值筛选







# OpenCV轻松入门

## 第二章图像处理基础

### 1．二值图像

   二值图像是指仅仅包含黑色和白色两种颜色的图像。

### 2．灰度图像

二值图像表示起来简单方便，但是因为其仅有黑白两种颜色，所表示的图像不够细腻。如果想要表现更多的细节，就需要使用更多的颜色

### 3．彩色图像

相比二值图像和灰度图像，彩色图像是更常见的一类图像，它能表现更丰富的细节信息。
神经生理学实验发现，在视网膜上存在三种不同的颜色感受器，能够感受三种不同的颜色



**在 OpenCV 中，通道的顺序是 B→G→R**

**使用 0 表示黑色，使用 255 表示白色**



在图像处理过程中，我们可能会对图像的某一个特定区域感兴趣，该区域被称为感兴趣区域（Region of Interest，ROI）



**函数 cv2.split()能够拆分图像的通道**

**lena=cv2.imread("lenacolor.png")**

- 在opencv中，读取图像是读取三个通道的像素值，lena是一个三位矩阵 [row,col,B/G/R],图像在opencv中是以BGR格式存储

```python
b,g,r=cv2.split(img)
b=cv2.split(a)[0]
g=cv2.split(a)[1]
r=cv2.split(a)[2]

b,g,r=cv2.split(lena)
bgr=cv2.merge([b,g,r])
rgb=cv2.merge([r,g,b])
# 读取灰度
gray=cv2.imread("lena.bmp",0)

# 表示 512 row 512col 3个通道
color.shape= (512, 512, 3)
# 表示512x512个像素
color.size= 786432
# 表示像素值的类型是uint8
color.dtype= uint8
```





## 第三章图像运算



### 3.1 图像加法运算

在图像处理过程中，经常需要对图像进行加法运算。可以通过加号运算符“+”对图像进行加法运算，也可以通过 cv2.add()函数对图像进行加法运

![image-20240527173810378](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527173810378.png)

result2=cv2.add(a,b)



### 3.2 图像加权和

图像像素按照一定的权重求和

add=cv2.addWeighted(face1,0.6,face2,0.4,0)





### 3.3 按位逻辑运算

and运算可以用来当作mask，

![image-20240527174005575](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527174005575.png)

使用mask可以只保留图像中的一部分

![image-20240527174241873](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527174241873.png)



### 3.6 位平面分解



将灰度图像中处于同一比特位上的二进制像素值进行组合，得到一幅二进制值图像，该图像被称为灰度图像的一个位平面，这个过程被称为位平面分解。例如，将一幅灰度图像内所有像素点上处于二进制位内最低位上的值进行组合，可以构成“最低有效位”位平面。

![image-20240527174515449](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527174515449.png)

得到位平面后，可以对位平面进行阈值处理（位平面的像素值只有0和1）

- 每次提取位平面后，要想让二值位平面能够以黑白颜色显示出来，就要将得到的二值位平面进行阈值处理，将其中大于零的值处理为 255



![image-20240527174659963](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527174659963.png)

位平面的作用

- 图像加密，解密，水印





## 第 4 章色彩空间类型转换

GRAY（灰度图像）通常指 8 位灰度图

### 4.1HSV色彩空间

HSV 色彩空间从心理学和视觉的角度出发，指出人眼的色彩知觉主要包含三要素：色调（Hue，也称为色相）、饱和度（Saturation）、亮度（Value），色调指光的颜色，饱和度是指色彩的深浅程度，亮度指人眼感受到的光的明暗程度。





## 第五章几何变换

### 5.1缩放

在 OpenCV 中，使用函数 **cv2.resize()**实现对图像的缩放



对图像放大时，插入的像素值方式

- 最临近插值
- 双线性插值（默认方式）



**注意：**读取图片是是(row,col) dsize是(col,row)

```
dst = cv2.resize( src, dsize[, fx[, fy[, interpolation]]] )
```



### 5.2翻转

cv2.flip()

![image-20240527180621127](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527180621127.png)



### 5.3仿射

仿射

- 平移
- 旋转
- 更复杂的仿射变换



**通过计算仿射矩阵来进行仿射变换**







### 5.4透视

**仿射变换可以将矩形映射为任意平行四边形，透视变换则可以将矩形映射为任意四边形。**





### 5.5 重映射

把一幅图像内的像素点放置到另外一幅图像内的指定位置，这个过程称为重映射







## 第六章阈值处理

阈值处理是指剔除图像内像素值高于一定值或者低于一定值的像素点。



### 二值化阈值

- 二值化阈值处理会将原始图像处理为仅有两个值的二值图像
- 将图像内所有像素值大于 127 的像素点的值设为 255。
- 将图像内所有像素值小于或等于 127 的像素点的值设为 0

### 反二值化阈值处理

- 对于灰度值大于阈值的像素点，将其值设定为 0。
-  对于灰度值小于或等于阈值的像素点，将其值设定为 255。

### 截断阈值化处理

- 对于像素值大于 127 的像素点，其像素值将被设定为 127。
- 对于像素值小于或等于 127 的像素点，其像素值=0

### 超阈值零处理

- 对于像素值大于 127 的像素点，其值将被设定为 0。
- 对于像素值小于或等于 127 的像素点，其值将保持不变

## 低阈值零处理

- 对于像素值大于 127 的像素点，其像素值将保持不变。
-  对于像素值小于或等于 127 的像素点，其像素值将被设定为 0

### 自适应阈值处理

- 适应阈值处理的方式通过计算每个像素点周围临近区域的加权平均值获得阈值，并使用该阈值对当前像素点进行处理。与普通的阈值处理方法相比，自适应阈值处理能够更好地处理明暗差异较大的图像

### Otsu 处理

- Otsu 方法能够根据当前图像给出最佳的类间分割阈值。简而言之，Otsu 方法会遍历所有可能阈值，从而找到最佳的阈值



## 第 7 章图像平滑处理

在尽量保留图像原有信息的情况下，过滤掉图像内部的噪声，这一过程称为对图像的平滑处理，所得的图像称为平滑图像。



**图像平滑处理会对图像中与周围像素点的像素值差异较大的像素点进行处理，将其值调整为周围像素点像素值的近似值。**



### 7.1 均值滤波

均值滤波是指用当前像素点周围 N·N 个像素值的均值来代替当前像素值。使用该方法遍历处理图像内的每一个像素点，即可完成整幅图像的均值滤波





### 7.2 方框滤波

在均值滤波中，滤波结果的像素值是任意一个点的邻域平均值，等于各邻域像素值之和除以邻域面积。而在方框滤波中，可以自由选择是否对均值滤波的结果进行归一化，**即可以自由选择滤波结果是邻域像素值之和的平均值，还是邻域像素值之和。**（方框滤波有两个结果）





### 7.3 高斯滤波

在进行均值滤波和方框滤波时，其邻域内每个像素的权重是相等的。在高斯滤波中，会将中心点的权重值加大，远离中心点的权重值减小，在此基础上计算邻域内各个像素值不同权重的和



### 7.4 中值滤

中值滤波与前面介绍的滤波方式不同，不再采用加权求均值的方式计算滤波结果。它用邻域内所有像素值的中间值来替代当前像素点的像素值，给NxN个像素值排序后取中间那一个值



### 7.5 双边滤波

双边滤波在计算某一个像素点的新值时，不仅考虑距离信息（距离越远，权重越小），还考虑色彩信息（色彩差别越大，权重越小）。双边滤波综合考虑距离和色彩的权重结果，既能够有效地去除噪声，又能够较好地保护边缘信息



## 第 8 章形态学操作

**很重要看书吧**



形态学，即数学形态学（Mathematical Morphology），是图像处理过程中一个非常重要的研究方向。形态学主要从图像内提取分量信息，该分量信息通常对于表达和描绘图像的形状具有重要意义，通常是图像理解时所使用的最本质的形状特征

形态学操作包括

- 腐蚀
- 膨胀
- 等等
- ![image-20240527182833555](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527182833555.png)

![image-20240527182803043](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527182803043.png)





## 第 9 章图像梯度

图像梯度计算的是图像变化的速度。对于图像的边缘部分，其灰度值变化较大，梯度值也较大；相反，对于图像中比较平滑的部分，其灰度值变化较小，相应的梯度值也较小。一般情况下，图像梯度计算的是图像的边缘信息



计算图像梯度

- sobel算子
  - Sobel 算子是一种离散的微分算子，该算子结合了高斯平滑和微分求导运算。该算子利用局部差分寻找边缘，计算所得的是一个梯度的近似值
- Scharr 算子
- Laplacian算子

![image-20240527183618394](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527183618394.png)





cv2.Sobel()中的ddepth参数

- 在函数 cv2.Sobel()的语法中规定，可以将函数 cv2.Sobel()内 ddepth 参数的值设置为-1，让处理结果与原始图像保持一致。但是，如果直接将参数 ddepth 的值设置为-1，在计算时得到的结果可能是错误的。
- 在实际操作中，计算梯度值可能会出现负数。如果处理的图像是 8 位图类型，则在 ddepth的参数值为-1 时，意味着指定运算结果也是 8 位图类型，那么所有负数会自动截断为 0，发生信息丢失。为了避免信息丢失，在计算时要先使用更高的数据类型 cv2.CV_64F，再通过取绝对值将其映射为 cv2.CV_8U（8 位图）类型。所以，通常要将函数 cv2.Sobel()内参数 ddepth 的值设置为“cv2.CV_64F”。





## 第 10 章Canny 边缘检测

![image-20240527183704298](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527183704298.png)

1. 应用高斯滤波去除图像噪声

2. 计算梯度

3. 非极大值抑制

   在获得了梯度的幅度和方向后，遍历图像中的像素点，去除所有非边缘的点。在具体实现时，**逐一遍历像素点，判断当前像素点是否是周围像素点中具有相同梯度方向的最大值**，并根据判断结果决定是否抑制该点。通过以上描述可知，该步骤是边缘细化的过程。如果该点是正/负梯度方向上的局部最大值，则保留该点，如果不是，则抑制该点（归零）。

4. 应用双阈值确定边缘

（1）如果当前边缘像素的梯度值大于或等于 maxVal，则将当前边缘像素标记为强边缘。
（2）如果当前边缘像素的梯度值介于 maxVal 与 minVal 之间，则将当前边缘像素标记为虚边缘（需要保留）。
（3）如果当前边缘像素的梯度值小于或等于 minVal，则抑制当前边缘像素



## 第 11 章图像金字塔

### 高斯金字塔

图像金字塔是由一幅图像的多个不同分辨率的子图所构成的图像集合。该组图像是由单个图像通过不断地降采样所产生的，最小的图像可能仅仅有一个像素点。

![image-20240527184018604](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527184018604.png)

**向上采样，向下采样，这两个操作都是不可逆的**

虽然一幅图像在先后经过向下采样、向上采样后，会恢复为原始大小，但是向上采样和向下采样不是互逆的。也就是说，虽然在经历两次采样操作后，得到的结果图像与原始图像的大小一致，肉眼看起来也相似，但是二者的像素值并不是一致的



### 拉普拉斯金字塔

前面我们介绍了高斯金字塔，高斯金字塔是通过对一幅图像一系列的向下采样所产生的。有时，我们希望通过对金字塔中的小图像进行向上采样以获取完整的大尺寸高分辨率图像，这时就需要用到拉普拉斯金字塔

![image-20240527184228791](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527184228791.png)

![image-20240527184312329](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527184312329.png)





## 第 12 章图像轮廓

边缘检测虽然能够检测出边缘，但边缘是不连续的，检测到的边缘并不是一个整体。图像轮廓是指将边缘连接起来形成的一个整体，用于后续的计算



查找轮廓

-  cv2.findContours()



比较轮廓

- 比较两个轮廓最简单的方法是比较二者的轮廓矩，轮廓矩代表了一个轮廓、一幅图像、一组点集的全局特征。矩信息包含了对应对象不同类型的几何特征，例如大小、位置、角度、形状等。



Hu矩

- Hu 矩是归一化中心矩的线性组合。Hu 矩在图像旋转、缩放、平移等操作后，仍能保持矩的不变性，**所以经常会使用 Hu 距来识别图像的特征**

轮廓拟合

- 矩形
- 圆形
- 椭圆
- 直线
  - 直线穿过图片中间
- 外包三角形
- 逼近多边形



凸包

- 逼近多边形是轮廓的高度近似，但是有时候，我们希望使用一个多边形的凸包来简化它。凸包跟逼近多边形很像，只不过它是物体最外层的“凸”多边形
- ![image-20240527185033031](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527185033031.png)

凸缺陷

![image-20240527185123866](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527185123866.png)



**opencv通过计算距离来比较形状差异**



## 第 13 章直方图处理

直方图的含义

- 统计不同灰度值出现的次数





直方图均匀化

- 直方图均衡化的主要目的是将原始图像的灰度级均匀地映射到整个灰度级范围内，得到一个灰度级分布均匀的图像。这种均衡化，既实现了灰度值统计上的概率均衡，也实现了人类视觉系统（Human Visual System，HVS）上的视觉均衡。

![image-20240527185553506](https://zhangwenkk333.oss-cn-beijing.aliyuncs.com/image/image-20240527185553506.png)







## 第 14 章傅里叶变换



图像处理一般分为空间域处理和频率域处理

**空间域处理是直接对图像内的像素进行处理**。空间域处理主要划分为灰度变换和空间滤波两种形式

什么是频率域处理？

- 率域处理是先将图像变换到频率域，然后在频率域对图像进行处理，最后再通过反变换将图像从频率域变换到空间域
- **傅里叶变换能够将图像从空间域变换到频率域**，而逆傅里叶变换能够将频率域信息变换到空间域内







## 第 15 章模板匹配

模板匹配是指在当前图像 A 内寻找与图像 B 最相似的部分，一般将图像 A 称为输入图像，将图像 B 称为模板图像。模板匹配的操作方法是将模板图像 B 在图像 A 上滑动，遍历所有像素以完成匹配。







## 第 16 章霍夫变换



什么是霍夫变换

- 在图像中寻找直线、圆形以及其他简单形状的方法

原理是什么？

- 其基本原理是通过将图像空间中的点转换到参数空间，从而将形状检测问题转化为参数空间中的峰值检测问题







# 图像的锐化



什么是锐化？

锐化的作用是增强图像中的边缘和细节，使图像看起来更加清晰。具体来说，锐化通过提高像素与其邻**域的对比度**，使过渡区域（如边缘或纹理）变得更加明显，从而让图像中的细节更加突出。

锐化有什么作用？

**增强边缘**：锐化可以增强图像中的边缘，使不同对象之间的轮廓更加明显

**细节提升**：锐化能让图像中的细微纹理和细节更加清晰

**提高视觉清晰度**：锐化可以增强图像中的边缘，使不同对象之间的轮廓更加明显



常见锐化方法

**拉普拉斯算子（Laplacian）**：通过计算图像的二阶导数来检测图像中的边缘，**并将这些边缘加回到原图像中，从而实现锐化效果。**

- 为什么二阶导可以检测图像的边缘：一阶导是图像灰度的变化率，会出现极值点。二阶导体现的是一阶导的变化率。当一阶导数从正值变为负值时，二阶导数会经过零点，这就是**零交叉现象**。零交叉点与边缘紧密相关，因此二阶导数可用于准确定位边缘。

**非锐化掩模（Unsharp Masking）**：是一种常见的锐化技术，它通过从图像中减去模糊版本（通常是高斯模糊）来突出边缘和细节。

**高通滤波器（High-Pass Filter）**：将图像中的低频成分（如平滑区域）过滤掉，保留高频成分（如边缘和细节），从而实现锐化。



# 图像平滑

锐化的反义词，降低细节和边缘，增加图像的整体质量

**平滑图像**在图像处理中的主要作用是减少噪声、降低细节，增强图像的整体质量，尤其是在处理噪声干扰较多或需要进一步分析的图像时。



有什么作用？

**降低噪声**

**去除随机噪声**：图像在采集过程中可能会受到环境影响（如光线不足、传感器干扰等），导致随机噪声的产生。平滑滤波器（如均值滤波、双边滤波等）可以通过邻域像素的平均或加权平均来抑制这些噪声。

**均值滤波**（去噪）

对于图像中的每个像素，均值滤波器会取该像素及其邻域（通常为正方形或圆形）内的所有像素值，计算这些像素值的平均值，然后将该平均值赋值给中心像素。

- 实现简单，计算速度快。

- 对随机噪声（如高斯噪声）有良好的去除效果。



**双边滤波**

双边滤波是一种非线性滤波技术，它在平滑图像的同时保留边缘信息。与均值滤波不同，双边滤波在加权平均时考虑了空间距离和像素值的差异。

双边滤波考虑的权重

- 像素之间的距离，离中心像素越近的像素权重越大。

- 像素值之间的相似性，相似的像素值权重更大。
- 能有效去除噪声的同时保留边缘信息，适合处理复杂图像
- 比均值滤波在保留细节方面表现更好





**图像平滑减少细节，突出重要特征**

在某些情况下，图像中的细节（如微小的纹理、复杂的边缘）可能干扰关键信息的提取。平滑图像可以减少不必要的细节，使关键特征（如大轮廓、边缘）更加显著。

- **平滑背景**：通过平滑滤波可以使背景区域更加均匀，从而突出前景物体的边界和形状，便于目标检测和分割。



### 总结

- **均值滤波**是一种简单的线性平滑技术，适合快速去除随机噪声，但可能导致图像细节丢失。
- **双边滤波**是一种更复杂的非线性平滑技术，能够有效去噪的同时保留边缘，适合处理含有细节的图像，但计算开销较大。



# 常见算子



## 梯度算子

**Sobel算子**：用于检测图像的水平和垂直边缘。

**Prewitt算子**：与Sobel类似，但计算方式略有不同。

**Scharr算子**：相比Sobel，计算更加精确



## 边缘检测算子

**Canny算子**：一种多阶段边缘检测算法，能够有效找到图像中的边缘并减少噪声。

**Laplacian算子**：基于二阶导数的边缘检测方法，常用于检测图像中快速变化的区域。



## **形态学算子** (Morphological Operators)

形态学操作主要用于图像的二值化和形状分析，广泛用于噪声去除、对象提取等任务。

- **膨胀 (Dilation)**：使图像中的物体变大。
- **腐蚀 (Erosion)**：使图像中的物体变小。
- **开运算 (Opening)**：先腐蚀再膨胀，主要用于去除噪声。
- **闭运算 (Closing)**：先膨胀再腐蚀，主要用于填补物体中的空洞。



## **傅里叶变换算子** (Fourier Transform Operator)

用于将图像从空间域转换到频率域，分析图像的频率成分。常用于图像复原、滤波等操作





## **Haar 小波算子** (Haar Wavelet Transform)

在图像处理中的多尺度分析中广泛使用，尤其是在图像压缩（如JPEG2000）和边缘检测中。

### 





## **自适应阈值算子** (Adaptive Thresholding)

该算子根据图像的局部区域的亮度值，动态调整阈值以实现更准确的二值化处理。





## **Gabor算子** (Gabor Filters)

用于纹理分析和边缘检测。Gabor滤波器是一种局部化的频域滤波器，具有方向和频率选择性。
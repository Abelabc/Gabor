# Gabor
（图文无关₍ᐢ..ᐢ₎♡，目前Ableabc也是初学者，一起学习吧）

简单讲解的视频地址：[BiliBili](https://www.bilibili.com/video/BV1fu4y1f7j8/?spm_id_from=333.999.0.0&vd_source=ef1fa2ad7e7e221ada15ff8f9748890d)


# skimage.filters.gabor（）函数返回的是图像变换后的实部和虚部，在图像识别领域一般使用其模作为图像特征
在图像处理领域，以Dennis Gabor命名的Gabor滤波器，是一种用于纹理分析的线性滤波器，即它主要分析的是，图像在某一特定区域的特定方向上是否有特定的频率内容。当代许多视觉科学家认为，Gabor滤波器的频率和方向的表达与人类的视觉系统很相似，尽管并没有实验性证据和函数原理能证明这一观点。研究发现，Gabor滤波器特别适合于纹理表示和辨别。在空间域中，2D Gabor滤波器是由正弦平面波调制的高斯核函数。
其脉冲响应定义为定义为一个正弦波（对于二维Gabor滤波器是平面波）乘以高斯函数。因为乘法卷积性质（卷积定理），由于乘法卷积性质，Gabor滤波器的脉冲响应的傅立叶变换是调和函数的傅立叶变换和高斯函数傅立叶变换的卷积。该滤波器具有表示正交方向的实部和虚部。两个分量可以构成复数或单独使用。

-复数形式:
![](https://pic2.zhimg.com/v2-870f94917f38b7b1293012d51dec2739_r.jpg)
-实部:
![](https://pic4.zhimg.com/80/v2-c0ed55e677e8de2c989d25a5be3e8c6f_720w.webp)
-虚部：
![](https://pic3.zhimg.com/80/v2-8f7f71ad4fb10d20426754c6f91ee5de_720w.webp)
如果你了解傅立叶变换的基础，那么你应该知道，图像可以视为各个方向的一系列不同频率的正弦波叠加而成。变换中的“像素”表示波的强度，“像素”的位置表示波的频率和方向。 在实践中，人们只想选择特定频率和特定方向的特定波。Gabor变换是许多所谓的带通滤波器之一，它允许你“切割”傅立叶变换，只隔离特定的信息。另一个重要信息是，每个傅立叶“像素”是一个复数值（包含实部和虚部）。
#### 实现代码 py codes

```html
<gabor.py>
    img = cv2.imread(filename)  # 读图像
    img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)  # 转灰度
    frequency = 0.6
    # gabor变换
    real, imag = filters.gabor(img_gray, frequency=0.6, theta=60, n_stds=5)
    # 取模
    img_mod = np.sqrt(real.astype(float) ** 2 + imag.astype(float) ** 2)
    # 图像缩放（下采样）
    newimg = cv2.resize(img_mod, (0, 0), fx=1 / 4, fy=1 / 4, interpolation=cv2.INTER_AREA)
    tempfea = newimg.flatten()  # 矩阵展平
    tmean = np.mean(tempfea)  # 求均值
    tstd = np.std(tempfea)  # 求方差
    newfea = (tempfea - tmean) / tstd  # 数值归一化
```

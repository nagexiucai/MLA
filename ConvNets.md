# Conv Nets: A Modular Perspective
作者以模块化视角剖析卷积网络。

## 简介
近几年，深度神经网络导致很多模式识别问题突破性进展，例如计算机视觉和语音识别。导致这些成果最本质的因素之一就是一种特殊的神经网络，叫做卷积神经网络。

最简单的，卷积神经网络可以看作一种复用很多同一神经元副本的神经网络。这使得网络在保持实参数量的前提下提供多神经元来表达计算机的大型模型，这些实参描述神经元的行为，应该学习得非常小（that need to be learned fairly small）。

![][0]

具有同一神经元的多个副本，这个花招大致类似数学和计算机科学中函数的抽象概念。

编程时，大家写一次函数，多处调用，而不用把相同的代码在不同的地方写上好多遍，这让编程变得高效、健壮（导致更少的问题）。

类似地，卷积神经网络可以学习一个神经元一次、然后很多地方使用，使得训练模型变得容易且降低错误。

## 卷积神经网络结构
假定读者想得到一个神经网络，盯着音频样本、预测有人说话否。

或许还想做更多的分析，如果有人说话。

读者实时获取音频样本的不同点，样本是均匀分布的（evenly spaced）。

![][1]

用神经网络给这些样本分类的最简单方式就是直接将这些样本连接到一个全连接层。

全连接层是一群不同的神经元，每个神经元都连接到每个输入。

![][2]

一个复杂点的方法注意到属性的某种对称性对寻找数据有用。

大家更多地关注数据的局部属性：

- 给定时间内声音的频率如何？
- 声音的频率降低或升高否？

大家实时关注所有样本的同一个属性。

一开始就知道声音的频率是有用的，了解中间的声音频率是有用的，清楚结尾的声音频率也有用的。

此外，注意到这些都是局部属性，只需要考虑音频样本一个小的窗口就可以做出检测。

因此，可以创建一组神经元， $A$ ，它能盯住数据样本的一小段。

对的，$A$ ，盯紧全部这中片段、计算确定的特征。

然后，该卷积层的输出反馈到一个全连接层， $F$ 。

![][3]

上述例子中， $A$ 只关心的片段包括两部分，这不现实！

通常，一个卷积层的窗口应当更大。

接下来的例子， $A$ 关注三个点，这也不现实！

悲哀，这是示意 $A$ 连接到多个点的把戏。

![][4]

卷积层的一个非常帅的属性就是自身的组成。

设计者可以把一个卷积层的输出反馈到另一个层。

网络中的每一层，都可以检测更高级的、更抽象的特征。

接下来，将有一组神经元， $B$ ，用来创建另一个卷积层，堆叠在之前那个卷积层的上边。

![][5]

卷积层之间经常夹杂着池化层。

特别是一种叫做“最大池化”的层，非常流行。

通常，从高级视角来看，大家不关心一个特征所代表的精确度。

如果音频样本的频移或早或晚地轻微发生，那真的没关系么？

一个最大池化层采用前一层小块特征的最大值，输出表明前一层的一个区域中出现该特征否，但位置不精确。

最大池化是种“缩小”，允许后续的卷积层在更大的数据片段上工作，因为池化后的一小片对应池化前的一大片。这让大家对一些很小的数据转换不变（invariant：去抖动）。

![][6]

之前的例子中，应用一维卷积层。然而，卷积层也能对多维数据起作用。

事实上，卷积神经网络最著名的成就就是应用二维卷积神经网络来识别图像。

在一个二维卷积层， $A$ 关注块而非片段。

对于每个块， $A$ 将计算其特征。例如，可能学习检测边缘的存在，或检测文本，或对比两种颜色。

![][7]

前例中，把卷积层的输出反馈到一个全连接层。

但是，也可以编排两个卷积层，向一维情况一样。

![][8]

也可以二维做最大池化。这里，取出一小块的特性的最大值。

归结起来，大家关心一整幅图片，不关心一个边缘的精确点、小到一个像素。

搞清楚在哪一小片像素内就足够了。

![][9]

三维卷积网络有时也会用到，像视频或立体数据（3D医学扫描）。

然而，三维的应用并不多，且很难可视化。

现在，如前所述， $A$ 是一组神经元。大家应当再准确一点： $A$ 究竟是什么？

传统的卷积层， $A$ 是一束并行的神经元，有相同的输入、计算不同的特征。

例如，一个二维卷积层中，某个神经元可能检测水平边缘、另外的神经元可能检测垂直边缘，别的神经元可能检测红绿色比。

![][10]

---
[0]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv2-9x5-Conv2Conv2.png "2D-CNN"
[1]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-9-xs.png "Audio-Samples"
[2]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-9-F.png "Full-Connected-Layer"
[3]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-9-Conv2.png "Convolution-Kernel-Look-At-Two-Points"
[4]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-9-Conv3.png "Convolution-Kernel-Look-At-Multiple-Points"
[5]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-9-Conv2Conv2.png "Another-Convolution-Layer"
[6]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-9-Conv2Max2Conv2.png "max-pooling"
[7]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv2-9x5-Conv2.png "2D-Convolution-Kernel-One-Layer"
[8]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv2-9x5-Conv2Conv2.png "2D-Convolution-Kernel-Two-Layers"
[9]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv2-9x5-Conv2Max2Conv2.png "2D-Convolution-Kernel-Multiple-Layers-With-Max-Pooling"
[10]:http://colah.github.io/posts/2014-07-Conv-Nets-Modular/img/Conv-A.png "What-Is-A-Neur"

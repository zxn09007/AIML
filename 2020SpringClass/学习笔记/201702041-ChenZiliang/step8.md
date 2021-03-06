# 陈子良 Step8总结

## 卷积神经网络
### 经典卷积神经网络模型
+ LeNet:CNN的鼻祖
    + 最初CNN的基本的构架：卷积层、池化层、全连接层。
    + 输入为单通道32x32灰度图
    + 使用6组5x5的过滤器，每个过滤器里有一个卷积核，stride=1，得到6张28x28的特征图
    + 使用2x2的池化，stride=2，得到6张14x14的特征图
    + 使用16组5x5的过滤器，每个过滤器里有6个卷积核，对应上一层的6个特征图，得到16张10x10的特征图
    + 池化，得到16张5x5的特征图
    + 接全连接层，120个神经元
    + 接全连接层，84个神经元
    + 接全连接层，10个神经元，softmax输出
+ AlexNet：先卷积然后在全连接
    + AlexNet有60 million个参数和65000个 神经元，五层卷积，三层全连接网络，最终的输出层是1000通道的softmax.
    + 特点：
        + 比LeNet深和宽的网络：使用了5层卷积和3层全连接，一共8层。特征数在最宽初达到384。
        + 数据增强：针对原始图片256x256的数据，做了随机剪裁，得到224x224的图片若干张。
        + 使用ReLU做激活函数：在全连接层使用DropOut
        + 使用LRN：LRN的全称为Local Response Normalizatio，局部响应归一化，是想对线性输出做一个归一化，避免上下越界。发展至今，这个技术已经很少使用了。
+  ZFNet：其网络结构没什么改进，只是调了调参，性能较Alex提升了不少。
+  VGGNe：主要的贡献是展示出网络的深度（depth）是算法优良性能的关键部分
      +  特点：
        +  选择采用3x3的卷积核是因为3x3是最小的能够捕捉像素8邻域信息的的尺寸。
        + 使用1x1的卷积核目的是在不影响输入输出的维度情况下，对输入进行形变，再通过ReLU进行非线性处理，提高决策函数的非线性。
        + 2个3x3卷积堆叠等于1个5x5卷积，3个3x3堆叠等于1个7x7卷积，感受野大小不变，而采用更多层、更小的卷积核可以引入更多非线性（更多的隐藏层，从而带来更多非线性函数），提高决策函数判决力，并且带来更少参数。
        + 每个VGG网络都有3个FC层，5个池化层，1个softmax层。
        +  在FC层中间采用dropout层，防止过拟合。
+ GoogLeNet：
    + 在加深网络的同时（22层），也在网络结构上做了创新，引入Inception结构代替了单纯的卷积+激活的传统操作。
    + ![](3.png) 蓝色为卷积运算，红色为池化运算，黄色为softmax分类
+ ResNets:称为残差网络。
    + ![](4.png) 基本表示
+ DenseNet：是一种具有密集连接的卷积神经网络
    + 优点：
        + 相比ResNet拥有更少的参数数量
        + 旁路加强了特征的重用
        + 网络更易于训练,并具有一定的正则效果   
        + 缓解了gradient vanishing和model degradation的问题
  
## 实现颜色分类
>>>>代码运行
### CNN
在这个模型中，我们的layer的设计是：

|ID|类型|参数|输入尺寸|输出尺寸|
|---|---|---|---|---|
|1|卷积|2x1x1, S=1|3x28x28|2x28x28|
|2|激活|Relu|2x28x28|2x28x28|
|3|池化|2x2, S=2, Max|2x14x14|2x14x14|
|4|卷积|3x3x3, S=1|2x14x14|3x12x12|
|5|激活|Relu|3x12x12|3x12x12|
|6|池化|2x2, S=2, Max|3x12x12|3x6x6|
|7|全连接|32|108|32|
|8|归一化||32|32|
|9|激活|Relu|32|32|
|10|全连接|6|32|6|
|11|分类|Softmax|6|6|

>>>>代码运行

经过20个epoch的训练后，得到的结果如下：

<img src="Image/color_cnn_loss.png" />

以下是打印输出的最后几行：

```
......
epoch=19, total_iteration=5639
loss_train=0.005293, accuracy_train=1.000000
loss_valid=0.106723, accuracy_valid=0.968000
save parameters
time used: 17.295073986053467
testing...
0.963
```

可以看到我们在测试集上得到了96.3%的准确度，比DNN模型要高出很多，这也证明了CNN在图像识别上的能力。
## 实现几何图形分类
>>>>代码运行
### CNN
在这个模型中，我们的layer的设计是：

|ID|类型|参数|输入尺寸|输出尺寸|
|---|---|---|---|---|
|1|卷积|8x3x3, S=1,P=1|1x28x28|8x28x28|
|2|激活|Relu|8x28x28|8x28x28|
|3|池化|2x2, S=2, Max|8x28x28|8x14x14|
|4|卷积|16x3x3, S=1|8x14x14|16x12x12|
|5|激活|Relu|16x12x12|16x12x12|
|6|池化|2x2, S=2, Max|16x6x6|16x6x6|
|7|全连接|32|576|32|
|8|归一化||32|32|
|9|激活|Relu|32|32|
|10|全连接|5|32|5|
|11|分类|Softmax|5|5|

经过50个epoch的训练后，我们得到的结果如下：

<img src="Image/shape_cnn_loss.png" />

以下是打印输出的最后几行：

```
......
epoch=49, total_iteration=14099
loss_train=0.002093, accuracy_train=1.000000
loss_valid=0.163053, accuracy_valid=0.944000
time used: 259.32207012176514
testing...
0.935
load parameters
0.96
```

可以看到我们在测试集上得到了96.5%的准确度，比DNN模型要高出很多，这也证明了CNN在图像识别上的能力。

## 实现几何图形及颜色分类
>>>>代码运行
### CNN
经过20个epoch的训练后，我们得到的结果如下：

<img src="Image/shape_color_cnn_loss.png" />

以下是打印输出的最后几行：

```
......
epoch=19, total_iteration=6079
loss_train=0.005184, accuracy_train=1.000000
loss_valid=0.118708, accuracy_valid=0.957407
time used: 131.77996039390564
testing...
0.97
load parameters
0.97
```

可以看到我们在测试集上得到了97%的准确度，比DNN模型要高出很多，这也证明了CNN在图像识别上的能力。
## 解决MNIST分类问题
+ 搭建模型：
    + ![](6.png)
+ 可视化：
    + 第一组的卷积可视化：显示了（卷积核数值、卷积核抽象、卷积结果、激活结果、池化结果）
    + 第二组的卷积可视化：输出结果：
        + Conv2：由于是在第一层的特征图上卷积后叠加的结果，所以基本不能理解
        + Relu2：能看出的是如果黑色区域多的话，说明基本没有激活值，此卷积核效果就没用
        + Pool2：池化后分化明显的特征图是比较有用的的特征，比如3、6、12、15、16，信息太多或者太少的特征图，都用途偏小


训练5个epoch后的损失函数值和准确率的历史记录曲线如下：

<img src="Image/mnist_loss.png" />

打印输出结果如下：

```
...
epoch=4, total_iteration=2089
loss_train=0.105681, accuracy_train=0.976562
loss_valid=0.090841, accuracy_valid=0.974600
epoch=4, total_iteration=2111
loss_train=0.030109, accuracy_train=0.992188
loss_valid=0.064819, accuracy_valid=0.981000
epoch=4, total_iteration=2133
loss_train=0.054449, accuracy_train=0.984375
loss_valid=0.060550, accuracy_valid=0.982000
save parameters
time used: 513.3446323871613
testing...
0.9865
```

最后可以得到98.44%的的准确率，比全连接网络要高1个百分点。如果想进一步提高准确率，可以尝试增加卷积层的能力，比如使用更多的卷积核来提取更多的特征。

## Fashion-MNIST分类
>>>>代码运行
### CNN
+ 提出问题：
    + 用10种物品代替了10个数字，下面是它们的部分样本：
        0. T-Shirt，T恤衫（1-3行）
        1. Trouser，裤子（4-6行）
        2. Pullover，套头衫（7-9行）
        3. Dress，连衣裙（10-12行）
        4. Coat，外套（13-15行）
        5. Sandal，凉鞋（16-18行）
        6. Shirt，衬衫（19-21行）
        7. Sneaker，运动鞋（22-24行）
        8. Bag，包（25-27行）
        9. Ankle Boot，短靴（28-30行）
+ 代码结果：

训练10个epoch后得到如下曲线：

<img src="Image/FashionMnistLoss_cnn.png" />

在测试集上得到91.12%的准确率，下面是在测试集上的前几个样本的预测结果：

<img src="Image/FashionMnistResult_cnn.png" ch="555" />

与DNN方案相比，这32个样本里只有一个错误，第4行最后一列，把第9类“短靴”预测成了“凉鞋”，因为这个样本中间有一个三角形的黑色块，与凉鞋的镂空设计很像。
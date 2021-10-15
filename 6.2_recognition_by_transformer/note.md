### 字符映射关系构建

---

##### 统计最长字符个数

​	统计数据集中最长label中包含的字符数量，为后面transformer搭建时间步长提供参考

##### char和id的映射字典构建

​	由于OCR中需要对字符进行预测，我们需要先建立一个字符与其id的映射关系

##### 分析数据集图像尺寸

​	我们需要确定图像的尺寸分布以选择合适的图像裁剪策略



### 将transformer引入OCR

---

​	transformer被广泛应用在NLP领域中，可以解决类似机器翻译这样的sequence to sequence类的问题

​	OCR识别任务的输入序列由图片形式表示，因此我们需要将图片信息构造成类似word embedding的形式的输入

​	我们需要借助一个CNN网络作为backdone提取图像特征得到inputing embedding

#### Dataset构建

---

​	假设图片的尺寸为[batch_size,3,$H{i}$,$W{i}$]

​	经过网络后的特征图尺寸为[batch_size,C,$H{f}$,$W{f}$]

![image-20211015235016397](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211015235016397.png)

在这里我们用resnet-18作为backdone，由于resnet-18的下采样倍数为32，最后一层特征图的channel为512，那么：
$$
H{i} = H{f}*32 = 32
\\
C{f} = 512
$$
接下来我们要确定输入图片的宽度，有两种方案备选，

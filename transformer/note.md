# transformer模型概览

<img src="C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211010115944704.png" alt="image-20211010115944704" style="zoom: 33%;" />

原论文模型图：

<img src="C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211010120202899.png" alt="image-20211010120202899" style="zoom: 50%;" />

​	上图中的左边是`encoder` 编码器，右边是`decoder`（解码器）

​	编码器：

- 由N个相同的层堆叠在一起
- 每一层有两个子层第一个子层是一个`Multi-Head Attention`(多头的自注意机制)，第二个子层是一个简单的`Feed Forward`(全连接前馈网络)。两个子层都添加了一个残差连接+layer normalization的操作。

  解码器：

- 堆叠了N个相同层
- 与编码器器中的每层结构不同的是解码器中加了一层`masked multi-head attention`

模型的输入由`Input Embedding`和`Positional Encoding`(位置编码)两部分组合而成


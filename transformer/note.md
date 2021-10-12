transformer模型概览

<img src="C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211010115944704.png" alt="image-20211010115944704" style="zoom: 33%;" />

原论文模型图：

<img src="C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211010120202899.png" alt="image-20211010120202899" style="zoom: 50%;" />

​	上图中的左边是`encoder` 编码器，右边是`decoder`（解码器）

  #### 	  编码器：

- 由N个相同的层堆叠在一起
- 每一层有两个子层第一个子层是一个`Multi-Head Attention`(多头的自注意机制)，第二个子层是一个简单的`Feed Forward`(全连接前馈网络)。两个子层都添加了一个残差连接+layer normalization的操作。

  #### 解码器：

- 堆叠了N个相同层
- 与编码器器中的每层结构不同的是解码器中加了一层`masked multi-head attention`

模型的输入由`Input Embedding`和`Positional Encoding`(位置编码)两部分组合而成



## 模型输入

#### 1、embedding层

> embedding层的作用是将某种格式的输入数据转变为模型可处理的张量表示

> `Embedding`层输出的可以理解为当前时间步的特征

#### 2、位置编码

> `Positional Encodding`位置编码的作用是为模型提供当前时间步的前后出现顺序的信息
>
> 解释了输入序列中单词顺序的方法

#### 3、两个输入模块

> `encoder`和`decodeer`都有输入模块，但encoder中输入只进行一次推理，decoder中则循环推理

## 编码器层

> 每个编码器层由两个子层连接结构组成
>
> 第一个子层包括一个多头自注意力层和规范化层以及一个残差连接
>
> 第二个子层包括一个前馈全连接层和规范化层以及一个残差连接



## attention

> 人类在观察事物时，无法同时仔细观察眼前的一切，只能聚焦到某一个局部。通常我们大脑在简单了解眼前的场景后，能够很快把注意力聚焦到最有价值的局部来仔细观察，从而作出有效判断。



## self-attention

> self-attention是transformer用来将其他相关单词的理解转换成我们正在处理的单词的一种思路
>
> 举例：`The animal didn't cross the street because it was too tired`中it的意思

#### self-attention的处理过程：

1. 首先self-attention会计算出三个新的向量，在原论文中向量的维度是512维度，这三个向量是embedding向量与一个矩阵相乘得到，矩阵是随机初始化的，维度为(64,512),矩阵的值会在BP过程中一直进行更新

   ​								![image-20211012223858925](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211012223858925.png)

2. 计算self-atteniton的值，该值决定了当我们在某一个位置encode一个词时，对输入句子的其他部分的关注程度，计算方法是Query和Key做点乘![image-20211012230039323](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211012230039323.png)

3. 接下来是把点乘的结果除以一个常数，该常数的值一般是上文提到的矩阵的第一个维度的开方，然后把得到的结果做一个softmax的计算，得到的结果即是每个词对于当前位置的词的相关性大小![image-20211012230052004](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211012230052004.png)

4. 之后再将value和softmax得到的值进行相乘并相加，得到的结果即是self-attention在当前节点的值![image-20211012230329805](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211012230329805.png)

> **这种通过 query 和 key 的相似性程度来确定 value 的权重分布的方法被称为scaled dot-product attention。**



##  多头注意力机制



![image-20211010153310198](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20211010153310198.png)

> 多头注意力机制能让每个注意力机制去优化每个词汇的不同特征部分，从而均衡同一种注意力机制可能产生的偏差，让词义拥有来自更多元表达

mulit-headed attention不仅仅初始化一组矩阵，而是初始化多组，transformer用了八组



## 前馈神经网络

>  由于前馈神经网络无法输入八个矩阵，因此我们先把八个矩阵连在一起，再随机初始化一个矩阵和这个组合好的矩阵相乘，得到最终矩阵。


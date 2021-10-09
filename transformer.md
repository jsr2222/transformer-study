# self-attention

**1.计算每个点和其他点的关联性（自己和自己也计算）得到α1,1、α1,2、α1,3、α1,4；（α1,2由α1的q和α2的k进行dot product产生）--如下图**

![image-20211008195337412](../../Typora/img/image-20211008195337412.png)

![image-20211008193102988](../../Typora/img/image-20211008193102988.png)



**2.对每个相关性进行softmax；**

![image-20211008193516547](../../Typora/img/image-20211008193516547.png)

例如：

![image-20211008194039150](../../Typora/img/image-20211008194039150.png)



**3.根据每个相关性抽取出这个sequence里面重要的信息；**

![image-20211008194530239](../../Typora/img/image-20211008194530239.png)

例如：

![image-20211008194746266](../../Typora/img/image-20211008194746266.png)



**4.对每个点重复上述步骤（输入可以使输入也可以是网络中间的一个隐藏层）；**

![image-20211008194902897](../../Typora/img/image-20211008194902897.png)



## 用矩阵来计算

**1.将上面的q,k,v用矩阵运算表示，则可以把所有点信息放到一个矩阵里面；**

![image-20211008195938130](../../Typora/img/image-20211008195938130.png)



**2.将ki值拼成一个K矩阵，将qi值拼成一个Q矩阵；**

![image-20211008201023508](../../Typora/img/image-20211008201023508.png)

![image-20211008201228109](../../Typora/img/image-20211008201228109.png)



**3.对相关度矩阵进行softmax（也可以relu等等）；**

![image-20211008201356986](../../Typora/img/image-20211008201356986.png)



**4.将第二步得到的相关度矩阵和由vi值拼成的V矩阵相乘；**

![image-20211008201215565](../../Typora/img/image-20211008201215565.png)

![image-20211008201624319](../../Typora/img/image-20211008201624319.png)



## 总结

**Wq，Wk，Wv就是唯三需要学习的参数；**

![image-20211008201922673](../../Typora/img/image-20211008201922673.png)



# multi-head Self-attention

**1.有多个q，不同的q负责不同种类的相关性；（k，v同理）**

![image-20211008203131283](../../Typora/img/image-20211008203131283.png)

![image-20211008203148179](../../Typora/img/image-20211008203148179.png)



**2.算出多个b**

![image-20211008203707872](../../Typora/img/image-20211008203707872.png)



**3.将多个b合并成一个；**

![image-20211008204126074](../../Typora/img/image-20211008204126074.png)



# positional Encoding(位置编码)

**1.因为在self-attention里面没有位置信息；所以要给每一个位置一个独特的位置向量ei；**

![image-20211008210137721](../../Typora/img/image-20211008210137721.png)





![image-20211008212955446](../../Typora/img/image-20211008212955446.png)

# transformer变体

**往右表示运算速度变快，越往上表示越好**

![image-20211008214843090](../../Typora/img/image-20211008214843090.png)



# Transformer

**transformer就是sequence-to-sequence ,输入的是一个序列，输出的也是一个序列，但是输出序列的长度由机器自己决定；**

![image-20211009093906492](../../Typora/img/image-20211009093906492.png)



**Layer Norm的计算方法**

![image-20211009094807265](../../Typora/img/image-20211009094807265.png)



**更多transformer设计架构**

![image-20211009095101506](../../Typora/img/image-20211009095101506.png)



**cross attention 运作过程**

![image-20211009101740090](../../Typora/img/image-20211009101740090.png)

![image-20211009101929560](../../Typora/img/image-20211009101929560.png)



## train过程

例如：输入标签：机器学习

我预测第一个字时真实的onehot编码机这个字的几率为1 ，其他为0，然后我预测也得到一个onehot的编码，真实和预测的做交叉熵，我们希望这个交叉熵越小越好，即机这个字预测所得几率越接近1越好。

![image-20211009102437555](../../Typora/img/image-20211009102437555.png)

![image-20211009104118080](../../Typora/img/image-20211009104118080.png)



**优化时可以用reinforcement learning(RL)**



训练时用交叉熵，预测时用BLEU score，挑BLEU score最高的model；

遇到无法优化的损失函数，把该损失函数当做RL的reward，把decoder当做engine，把他作为RL reinforcement的问题来解决

![image-20211009105917063](../../Typora/img/image-20211009105917063.png)



## exposure bias

因为测试的时候decoder输入是自己预测出来的，所以可能是错的；

而训练的时候decoder的输入是完全正确的，所以会出现 **exposure bias**的问题：

==解决方法：==在训练时人为给一些错的输入，这个方法叫做Scheduled Sampling

![image-20211009111235348](../../Typora/img/image-20211009111235348.png)

![image-20211009111743576](../../Typora/img/image-20211009111743576.png)

![image-20211009111850186](../../Typora/img/image-20211009111850186.png)
#### IGD阅读笔记

1. 标题：[**Deep One-Class Classification via Interpolated Gaussian Descriptor**](https://arxiv.org/pdf/2101.10043.pdf)，基于高斯差值描述符的深度一分类模型

2. 来自：University of Adelaide；AAAI2022

3. 阅读目的：希望将最小化超球面的思路引入SCST模型，微调教师网络

4. 主要贡献：一个新的OCC模型/一个新的OCC优化方法/提出新的OCC基准，可以评估小样本，异常样本的训练稳健性

5. 模型描述：

   <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s4rlqgr5j30st07qael.jpg" alt="image.png" style="zoom:80%;" />

6. 论文公式详解：

   + <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s50qfxofj304100nt8i.jpg" alt="1664782577075.png" style="zoom:80%;" />

     > 通用分类器的表示方法，x是属于分布Px中的一个变量，分类器求解其属于类别0的概率pΘ

   + <img src="C:\Users\12559\AppData\Roaming\Typora\typora-user-images\image-20221003153740435.png" alt="image-20221003153740435" style="zoom:80%;" />

     > 编码器的表示方法，将图像输入X转化为输出Z，一个映射器

   + <img src="C:\Users\12559\AppData\Roaming\Typora\typora-user-images\image-20221003153904788.png" alt="image-20221003153904788" style="zoom:80%;" />

     > 高斯异常分类器，：
     >
     > 1. <img src="C:\Users\12559\AppData\Roaming\Typora\typora-user-images\image-20221003154132467.png" alt="image-20221003154132467" style="zoom:80%;" /><img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5ak7hstj309603w0sn.jpg" alt="image.png" style="zoom:50%;" />
     >
     >    高斯分布的概率密度函数，高斯分布又叫做正太分布，左式是高维正太分布的表达式，右式是我们熟悉的一维正太分布，重要的参数是C (协方差)和 μ (分布的中心)，可以根据这个方程求解点落在某个区间的概率
     >
     > 2. <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5e0wqc0j30c50abtaj.jpg" alt="image.png" style="zoom:80%;" />
     >
     >    以上红色和绿色分别表示两个高斯分布，如何求解蓝色点属于哪个分布？
     >
     >    本质上是求解：
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5h64tizj308r01vt8m.jpg" alt="image.png" style="zoom:80%;" />
     >
     >    x属于类别1还是类别2？，w1和w2分别代表类别1和类别2
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5i9xjq4j30de04gq3h.jpg" alt="image.png" style="zoom:80%;" />
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s6wyzh9ej30sj04vgq2.jpg" alt="image.png" style="zoom:80%;" />
     >
     >    计算两个P()的方法，使用贝叶斯公式，其中P(w1)和P(w2)是先验知识，两个类别出现的概率，例如已知类别1出现的概率的0.8而类别2出现的概率是0.2，那么P(w1)和P(w2)分别是0.8和0.2。其他的P(x|w1)等解释可见上图，其中需要注意的是P(x|w1)和P(x|w2)直接套前面的高斯概率密度公式即可，表示类别1/2中出现x这种参数的概率是多少。
     >
     >    因此，待求解的式子可以做以下变换：
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5s6gqsvj30j20adgmt.jpg" alt="1664784153036.png" style="zoom:80%;" />
     >
     >    在C1=C2且两个类别出现概率相同的情况下，本质上就是比较x点距离两个分布中心的距离。
     >
     >    如果不相等呢：
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5ttdd35j30ks03rt98.jpg" alt="1664784246423.png" style="zoom:80%;" />
     >
     >    α表示两个类别出现概率的比值，C1和C2两个协方差矩阵都在。
     >
     >    因此可以直接推导高斯分类器，C1=C2的情况：
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5y6pwcmj30qi09ljst.jpg" alt="image.png" style="zoom:80%;" />
     >
     >    C1≠C2的情况：
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5yci6boj30cy03naac.jpg" alt="image.png" style="zoom:80%;" />
     >
     >    总结如下：
     >
     >    <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s5xoo1maj30qu084q5r.jpg" alt="1664784461841.png" style="zoom:80%;" />
     >
     >    第一步：求出两个分布的C和μ，第二部：设置参数α，即两个分布出现的先验概率，第三步：直接写出高斯分类器的函数P(w1|x)，上图中有个小错误，“其中”两字之后应该是P(x|w1)，即为高斯分布的概率密度函数

     因此文章中的高斯异常分类器，只要得到高斯分布的一些参数就可以对输入的图像进行分类

   + <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s62vbzahj302l00rt8h.jpg" alt="image.png" style="zoom:80%;" />

     > 解码器，输入的是编码器的输出z，输出的是重建图像x'，目前还不清楚解码器干啥用的

   + <img src="http://tva1.sinaimg.cn/large/007QnVXJly1h6s6culrwwj306p00pa9w.jpg" alt="image.png" style="zoom:80%;" />

     > 一个评价模块，用于预测α的取值，而α是插值参数，αz1 + (1 − α)z2是插值的结果，其中z1和z2分别是两个正常图像的编码向量。观察此等式，发现这个评价者模块是这样运作的，先将插值的结果经过decoder变为g(插值)，然后计算dn，这个是什么？

     
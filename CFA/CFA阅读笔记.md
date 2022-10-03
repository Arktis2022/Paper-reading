### CFA阅读笔记

1. 标题：coupled-hypersphere-based Feature Adaptation for Target-Oriented Anomaly Localization

2. 来自：Inha University

3. 代码网址：[GitHub - sungwool/CFA_for_anomaly_localization](https://github.com/sungwool/CFA_for_anomaly_localization)

4. 要点：

   + a learnable patch descriptor --> learns and embeds  target-oriented features.（一个可学习的补丁描述符，用于学习并嵌入面向目标的特征？）
   + scalable memory bank，可扩展内存库，不知道干啥的（卷积模型通过将正常特性存放到内存库中，以此了解正常特征的分布）
   + 在MVTec AD上实现SOTA，并且性能更佳，减少了内存占用

5. 实验结果——MVTec AD 数据集上异常检测AUROC：99.5%，异常定位AUROC：98.5%

6. 提出了什么？

   > 1. 定义了一个基于软边界回归的新损失函数，搜索最小半径的超球面，以此聚类正常特征（基于特征提取器的方法和超球面方法的结合，这个idea并不新奇了），应用与基于知识蒸馏的异常检测如何借鉴？——微调teacher encoder！
   > 2. 为了减少推理时间，提出一个可扩展的内存库
   > 3. 贡献总结：
   >    + 预训练模型的偏差特征影响异常检测，可以通过目标数据集的适应方法解决，可以引用到我的模型中
   >    + 通过度量学习判别特征的方法
   >    + 压缩存储库减小模型容量

7. 模型详解：

   1. 示意图：

      <img src="C:\Users\12559\AppData\Roaming\Typora\typora-user-images\image-20220930152733310.png" alt="image-20220930152733310" style="zoom: 80%;" />

   2. 解释上图：

      > + Frozen CNN：在大数据集上预训练过的CNN，其是有偏的，提取的特征不一定适合目标数据集
      > + Patch feature（Pf）：将各层提取到的不同分辨率的特征图，插值成为相同分辨率的特征图再连接起来，得到patch feature，此过程参见了Padim，补丁特征是这个意思原来，就是各层特征的拼接（图中的I表示插值，C表示拼接）
      > + Patch descriptor：补丁描述符，以上一步得到的Patch feature作为输入，进一步压缩特征，其是可训练的，目的是将Pf转化为面向目标的特征
      > + Memory bank：将Patch descriptor提取得到的面向目标的特征存在其中，仅在初始化步骤执行
      > + 训练阶段：以Memory bank中存储的记忆特征为中心，创建超球面，训练图中的NN search和Pd模块，使得PD和NN search得到的特征被紧密压缩

   3. 训练阶段：

      > + 损失函数：
      >
      >   <img src="C:\Users\12559\AppData\Roaming\Typora\typora-user-images\image-20221003143543442.png" alt="image-20221003143543442" style="zoom:80%;" />
      >
      >   <img src="C:\Users\12559\AppData\Roaming\Typora\typora-user-images\image-20221003143557274.png" alt="image-20221003143557274" style="zoom:80%;" />
      >
      >   有两个损失函数，用于将CFA处理之后的pt向ct聚拢，并原理周边的其他ct，因此构建了两个损失函数；第二个损失函数代表对比学习1


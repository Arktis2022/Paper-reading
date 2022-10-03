### DOCC阅读笔记

1. 标题：Deep one class classification

2. 来自：ICML2018

3. 模型描述：

   一个SVDD和一个神经网络，神经网络将输入映射到最小的超球体

4. 优化目标：

   ![image.png](http://tva1.sinaimg.cn/large/007QnVXJly1h6s7tp5o8zj30cv03vdfz.jpg)

   > R 是 半径，c是球心，🤖(xi；W)指的是输入x经过神经网络变换之后得到的结果，最后一项是超参数。优化目标是W

   简化版本：

   ![image.png](http://tva1.sinaimg.cn/large/007QnVXJly1h6s7tu8au1j30c301t0sr.jpg)

   > 不再设定R，直接将所有数据映射到尽量靠近中心c的位置。

   ![image.png](http://tva1.sinaimg.cn/large/007QnVXJly1h6s7u4s86fj306z013744.jpg)

   > 对于输入的图片，异常分数可以简单的通过上式得到
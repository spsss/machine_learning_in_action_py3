<p align = "center"> <img width = "350％"  src = "logo/logo.jpeg"/> </p>

# 机器学习实战笔记(附:[源代码][src] 基于 **[GNU3.0][license]** 协议 
附： [版本号][version] [作者][author]

## 第一部分 分类

### 第一章 机器学习基础[代码][ch01]

1. **熟悉[Python][Python]即可。**

2. **开发机器学习应用程序步骤**

    1. 收集数据。
    
    2. 准备输入数据。
    
    3. 分析输入数据。
    
    4. 训练算法。
    
    5. 测试算法。
    
    6. 使用算法。
   
3. **掌握[numpy][numpy]函数库基础**

    `>> from numpy import *`

### 第二章 K-近邻算法[代码][ch02]

1. **K-近邻算法优缺点**

    1. 优点:精度高,对异常值步敏感，无数据输入假定。
    
    2. 缺点:计算复杂度高，空间复杂度高。
    
    3. 范围:数值型和标称型。
      
2. **测试分类器**

    **`错误率是常用的评估方法，完美评估器为0，最差的评估器为1.0`**

3. **例子:使用k-近邻算法改进约会网站的配对效果**

    1. 准备数据:从文本数据中解析出数据，用`numpy`转化文本为矩阵，同时进行归一化数值操作(将对数据有影响的数值归纳为`0～1`之间)。

    2. 分析数据:使用`matplotlib`实现数据可视化。

    3. 测试数据:错误评估 **`训练数据/测试数据 = 90%/10%`**。

    4. 使用算法:基于用户的输入，自动匹配。

4. **例子:手写识别系统**

    1. 准备数据:将图像分为`32*32`的二进制图像转化为`1*1024`的数组，每次读取`32`行,存入数组，并且返回数组。

    2. 分析数据:确保数据准确无误。

    3. 测试数据:随机选取数据测试。

    4. 使用数据:将评估错误率，选择`最低评估错误率`来作为首选算法。

5. **小节**

    **K-近邻算法是最简单的分类算法，如果数据量太大，会变得非常耗时。**

### 第三章 决策树[代码][ch03]

1. **决策树算法优缺点**

    1. 优点:计算复杂度不高，输出结果易于理解，对中间值不明干，可以处理不相关特征数据。
    
    2. 缺点:可能会产生过度匹配。
    
    3. 范围:数值型和标称型。

2. **信息增益**

    1. 原则: 将无序的数据变得更加有序。
    
    2. 在划分数据集之前之后信息发生的变化
    
    3. 熵: 信息的期望值，或者集合信息的度量方式。
    
3. **熵**

    1. 若数据都为一类，那么`H=-1*log2(1)=0，`不用任何信息就能区分这个数据。
    
    2. 如有一枚正反两面硬币，分为正面或者反面的概率都为`0.5, H= -0.5log2(0.5) - 0.5log2(0.5) = 1,` 需要一个单位比特信息区分是否是正面或者反面，也即0或者1。
    
    3. 熵，代表信息的混乱度信息。其基本作用就是消除人们对事物的不确定性。一个系统越是有序，信息熵就越低；反之，一个系统越是混乱，信息熵就越高。所以，信息熵也可以说是系统有序化程度的一个度量。
    
    4. 具体地可参考《信息论》。

4. **划分数据集**

    **将每个特征划分数据集的结果计算一次信息熵，然后判断按照哪个特征划分数据集是最好的划分方式。**

5. **递归构建决策树**

    1. 工作原理：得到原始数据集，基于最好的属性值划分数据集，第一次划分后，再次划分数据。因此可以递归处理数据集。

    2. 递归结束的条件：划分数据集所有属性，或者每个分支下的所有实例都具有相同的分类。

    3. 如果数据集已经处理了所有属性，但是类标签依然不是唯一的，此时我们通常采用多数表决的方法决定该叶子节点的分类。

6. **测试算法:使用决策树执行分类**

    1. 执行分类时，需要使用决策树以及用于决策树的标签向量。
    
    2. 测试数据与决策树上的数值，递归执行该过程直到进入叶子节点。
    
    3. 最后将测试数据定义为叶子节点所属的属性。
    
7. **使用算法:决策树的存储**
    
    **为了节约时间和节省内存消耗，使用了pickle模块序列化对象。**
    
8. **例子:使用决策树预测隐形眼睛类型**

    **目标:通过决策树预测患者需要佩戴的隐形眼睛类型。**
    ```python
    import trees
    fr = open('lensens.txt')
    lenses = [inst.strip().split('\t') for inst in fr.readlines()]
    lenseslabels = ['age', 'prescipt', 'astigmatic', 'tearRate']
    lensestree = trees.create_tree(lenses, lenseslabels)
    lensestree
    treePlotter.create_plot(lensestree)
    ``` 
    
9. **小节**

    **这里主要是采用`ID3算法划`分数据集，用递归的方法将数据集转化为决策树，并可用`pickle模块存`储决策树的结构。ID3算法无法处理直接数值型数据，需要将其化为标量型数值。决策树最大的缺点在于`过拟合问题`。在构建树的时候，其能够完全匹配实验数据，但是这并不是我们想要的，为此，可以删掉一些只增加了很少信息的节点，将其并入到其他叶子节点中，或者裁剪一些分支。具体决策树的很多问题也待整理。**

### 第四章 基于概率论的分类方法:朴素贝叶斯[代码][ch04]

1. **基于贝叶斯决策理论算法优缺点**

    1. 优点:在数据较少的情况下仍然有效。可以处理多类别问题。
    
    2. 缺点:对于输入数据的准备方式较为敏感。
    
    3. 范围:标称型数据。
    
2. **Tip:贝叶斯决策理论的核心思想是选择高概率对应的类别，即选择具有最高概率的决策**

3. **贝叶斯法则**

    **后验概率 = 标准似然度 * 先验概率。**

4. **贝叶斯定理**

    1. 对于变量有二个以上的情况，贝叶斯定理亦成立。例如：
    
        1. P(A|B,C)=P(B|A)*P(A)*P(C|A,B)/(P(B)*P(C|B))
        
        2. 这个式子可以由套用多次二个变量的贝叶斯定理及条件机率的定义导出。
        
5. **使用Python进行文本分类**

    1. 背景
    
        **以在线社区的留言板为例。为了不影响社区的发展，我们要屏蔽侮厚性的言论，所以要构建一个快速过滤器，如果某条留言使用了负面或者侮辱性的语言，那么就将该留言标识为内容不当。过滤这类内容是-一个很常见的需求。对此问题建立两个类别:侮厚类和非侮辱类，使用1和0分别表示。**

    2. 准备数据:从文本中构建词向量
    
    3. 训练算法:从词向量计算概率
    
    4. 测试算法:根据现实情况修改分类器
    
        1. 贝叶斯概率需要计算多个概率的乘积以获得文档属于某个类别的概率，即计算p(w0|1)p(w1|1)p(w2|1)。如果其中一个概率值为0，那么最后的乘积也为0

        2. 第二个问题就是下溢出，这是由于太多过小的数相乘造成的。由于大部分因子都非常小，所以程序会下溢出或者得不到正确的答案。解决办法是对乘积取自然对数这样可以避免下溢出或者浮点数舍入导致的错误。

        3. 每个单词的出现与否作为一个特征，被称为词集模型；在词袋模型中，每个单词可以出现多次。
    
    5. 准备数据:文档词袋模型

6. **例子:使用朴素贝叶斯过滤垃圾邮件**

    1. 准备数据:切分文本
    
    2. 测试算法:使用朴素贝叶进行交叉验证

7. **例子:使用朴素贝叶斯从个人广告中获取区域倾向**

    1. 收集数据:导入RSS源
    
    2. 分析数据:显示低于相关的用词

    **Tip:这里训练测试的方法是从总的数据集中随机选择数字，将其添加到测试集中，同时将其从训练集中剔除。这种随机选择数据的一部分作为训练集，而剩余部分作为测试集的过程为留存交叉验证（hold-out cross validation）。有时为了更精确地估计分类器的错误率，就应该进行多次迭代后求出平均错误率。**

    

[src]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src

[license]:https://github.com/shiyipaisizuo/machine_learning_in_action/blob/master/LICENSE

[version]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/__version__

[author]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/__author__

[python]:https://www.python.org/

[numpy]:http://www.numpy.org/

[ch01]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch01

[ch02]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch02

[ch03]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch03

[ch04]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch04

[ch05]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch05

[ch06]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch06

[ch07]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch07

[ch08]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch08

[ch09]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch09

[ch10]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch10

[ch11]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch11

[ch12]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch12

[ch13]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch13

[ch14]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch14

[ch15]:https://github.com/shiyipaisizuo/machine_learning_in_action/tree/master/src/ch15

---
layout: post
title: 行人重识别文献研究 
modified: 2017-09-27
categories: [articles, study]
tags: 
  - study
comments: true
---

计算机视觉研究中，利用行人重识别技术可以判断同一个人是否在另一个地方被另一个传感器捕捉到。计算机视觉领域的学者们形象地将针对特定行人的监控视频检索问题称为行人重识别。随着公共安全的要求和摄像头等传感器设备被大规模被应用到主题公园，学校，街道，超市等场景中，传统的依靠人工的方式进行行人识别跟踪，需要大量的人力资本投入。同时，考虑到门店的建设成本，针对门店场景的行人重识别方法应考虑到算法的实施成本，这些都对行人的识别与跟踪，特别是在行人信息的高效检索等领域提出了新的挑战。

从技术上讲，实际视频监控系统的行人重识别系统可分为三个模块：行人检测，行人跟踪和行人信息检索。前两个任务是独立的计算机视觉任务，所以大家主要的工作还是最后一个模块。在行人重识别的研究中前期主要是围绕特定应用，进行特征提取，然后针对小规模数据集展开应用研究。近年来，随着数据量的增长以及深度学习研究的进一步推进，深度学习在行人识别检索的应用中取得了一定的成果。在门店场景顾客检索去重的研究中，对行人重识别提出了更高的要求。本文主要对从方法上对行人重识别做出一个大致的整理。

行人重识别基本原理
==================

行人重识别模型主通过使用单张图像作为输入与N张图像(g)进行匹配，这些图像的特征可描述为$$\left\{
g_{i} \right\}_{i =
1}^{N}$$，这N张图像属于N个不同的个体（ID）。给定一个个体q，其ID号$$i^{*}$$可以通过以下公式获得：

$$
i^{*} = \text{argmax}_{i \in 1,2,\ldots,N}\text{sim}\left( q,\ g_{i} \right)
$$

其中$$\text{sim}\left( q,\ g_{i}
\right)$$是行人相似度度量函数，相似度最大的那个个体特征对应的ID即为所求的*i*

行人重识别门店场景可行性分析
============================

1、 基于人脸识别进行行人重识别在门店场景中的应用可行性分析

理论上可行，但是有两个原因导致人脸识别较难应用：首先，广泛存在后脑勺和侧脸的情况，做正脸的人脸识别难。其次，摄像头拍摄的像素可能不高，尤其是远景摄像头里面人脸截出来很可能都没有达到32x32的像素。所以人脸识别在实际线下门店场景中的应用受到限制。

2、 基于衣服颜色的行人重识别在门店场景中的应用可行性分析

在行人重识别的过程中，衣服颜色确实是行人重识别做出判断一个重要因素，但光靠颜色是不足的。首先，摄像头之间是有色差，并且会有光照的影响。其次，有撞衫（颜色相似）的情况发生，要找细节，但比如颜色直方图这种统计的特征就把细节给忽略了。在门店顾客行人的重识别研究中，光用颜色特征是难以达到50%的top1正确率。

3、 直接将图像检索指标套用在行人重识别研究中是否合适

在早期，行人重识别数据集是由两个摄像头采集的比如viper，每个query只有一个正确的retrieval目标。所以往往使用top1比较。但在近期，随着大数据集的提出，数据集中往往包含多个摄像头的多个正确目标。光使用top1的话，不能反应模型的真实能力。所以类似图像检索，重识别加入了mAP（mean average precision）作为衡量标准，将排名为1\~到n的结果（top1~n）都考虑进去。

4、测试方法可行性分析

目前行人重识别主要有两种方法。第一种方法是通过输入一对行人，输出这对行人的相似度，然后再按照相似度进行排序。第二种方法是提取特征，再计算与其他人的欧式距离，然后再按距离排序。

第一种方法的优点是，判断两个人是不是一个人，简单的二分类（是／否）。但缺点是如果我们搜索库中有m张图片，那么与目标图片组成m对图片对。每一对都要进一次模型，估算相似度，这极大的增加了测试的时间。如果我们有n个query，那么我们要计算nm次相似度（而m往往很大）。第二种方法是，预先提取行人的特征，我们只要预先提好n+m次特征。之后只要比较就好了，比较特征可以简单的用矩阵乘法实现。

通过搜索目前的研究成果，两种方案都有在用，但在门店客流场景中平台计算能力有限，因此用特征来快速检索更接近实际中图像搜索的要求。

<!-- more -->

国内外研究文献整理
==================

行人重识别问题中的图片来源于不同的摄像头，然而，由于不同摄像头所处的角度、光照等环境的影响，行人重识别问题具有以下几个特点：由于实际监控环境中，无法使用脸部的有效信息，所以，只能利用行人的外貌特征来进行识别；在不同摄像头中，由于尺度、光照和角度的变化，同一个行人的不同图片中，外貌特征会有一定程度的变化；由于行人姿势及摄像头角度的变化，在不同摄像头中，不同行人的外貌特征可能比同一个人的外貌特征更相似。

学者从数据源将行人重识别问题分为基于影像的行人重识别技术和基于视频的行人重识别两类方法。本文为了搜索适用于门店客流场景的行人识别检索方法，重点围绕了基于特征表示、基于距离度量，基于深度学习技术在行人重别中的应用研究分别展开了讨论。

基于特征表示的方法
------------------

在视频监控环境中，行人的外貌特征比较容易提取和表示。因此，同一行人的不同外貌特征具有一定的鲁棒性。

1）(Dong, Cristani et al. 2011)为了减少视角变化导致的外貌变化，Farenzena M, etal提出通过基于人身体对称性的特征提取方法。首先通过一个预处理过程在人身体上划分头、躯干、腿部和左右对称中轴，然后提取除了头部以外的各区域的多种特征，包括累积颜色特征和纹理特征。并基于对称中轴对特征进行加权，越靠近中轴权值越高[1]。

2) (Farenzena, Bazzani et al.2010)l提出类似的方法，将绘画结构应用于行人重识别。用一个自适应的身体外形结构来表示行人像，包括头、胸、大腿和小腿，然后提取每个部分的颜色特征进行精确匹配[2]。

3）(Bazzani, Cristani et al.2012)结合行人的全局和局部外貌特征进行重识别，首先根据行人在单摄像头下的连续运动提取多个关键帧图像，并用多帧图像的累积HSV颜色直方图表示全局特征；其次，在把人身体分割成上、下半身并去除头部区域后，提取各上、下半身多帧图像中频繁出现的块信息表示局部特征；最后加权融合全局和局部特征进行行人重识别[3]。

基于距离度量学习的方法
----------------------

上述基于特征的方法都是使用标准距离（如曼哈顿距离、欧氏距离和巴氏距离等）进行相似性度量。然而同一身份行人在跨越多个无重叠区摄像头时，不同外貌特征受视角、光照等因素的影响不同。标准的距离度量方法平等的对待每一种特征，而不会摒弃那些独立使用时效果很差的特征。因此，研究者尝试通过距离学习的方法，获得一个新的距离度量空间，使得同一行人不同图像的距离小于不同人间的距离。距离学习方法一般在马氏（Mahalanobis）距离的基础上进行，通过学习一个投影矩阵，使得在投影空间中同类样本之间的距离较小，而不同类样本之间的距离较大。

1）(Xing, Ng et al.2002)提出距离测度学习的问题，他在Mahalanobis距离的基础上，根据样本的类别，将具有相同类别标签的样本组成正样本对，不同类别标签的样本构成负样本对，然后利用这些样本对作为约束条件来训练得到一个Mahalanobis矩阵，从而使得最终的距离度量函数能够尽可能地满足所给定的约束条件。[4]

2）(Weinberger and Saul 2009)提出最大近邻分类间隔(large marginnearest neighbor classification,LMNN)的算法，其思想类似于支持向量机，即希望寻求一个分类超平面，使得该超平面与最靠近点的距离尽可能大。同样，LMNN希望通过投影后，数据的邻域内的同类点向内部紧缩，不同类点向外扩张，并且之间的间隔尽可能大。[5]

3）(Dikmen, Akbas et al. 2010)对 LMNN进行改进提出LMNN-R方法，其用所有样本点的平均近邻边界来代替 LMNN中不同样本点所采用的各自近邻边界，取得了比LMNN 方法更强的约束效果。[6]

4）(Zheng, Gong et al.2011)提出概率相对距离比较的方法，在学习距离度量函数时考虑相对约束，与之前的同类距离尽可能小，不同类距离尽可能大的要求不同，其要求同类的距离小于不同类之间的距离。对每一个样本，选择一个同类样本和不同类样本形成3元组，在训练过程通过最小化不同类样本距离减去同类样本距离的和，得到满足约束的距离度量矩阵。[7]

基于神经网络的方法
------------------

下述的方法主要基于神经网络来实现的，在这里从学者们的研究角度将行人重识别的思路大致分为基于人体部分的特征匹配和特征表达的损失函数设计两个方面。

### 基于人体部分的特征匹配

基于人体部分的特征匹配，顾名思义，就是将人体看做是几个部分的组合，然后一部分一部分来比较。常见方案是水平切条，就是将图像切为几个水平的条。由于人体身材往往差不多，所以可以用简单的水平条来做一一比较[8-11]。在领域中做匹配，采用的是一个正方形的领域[12]。另一个较新的方案是先在人体上检测部件（手，腿，躯干等等）再进行匹配，这样的话可以减少位置的误差，但可能引入检测部件的误差[11,
13]。

### 基于学习特征表达的损失函数设计

为了便于说明我们这里首先介绍一下常用的损失函数。其实行人重识别训练大体上就是基于四种损失函数（loss），距离损失函数（distance loss），分类损失函数（identification loss），验证损失函数（verification loss），基于属性分类的损失函数（attributes loss）。distance loss是最常用的loss，就是用feature之间的距离来计算loss，同一个人的feature应该更近，不同人的feature要更远，那么用同一个人的距离减掉不同人的距离加上一个正常数和0取max实现，代表的有triplet loss，improved triplet loss，quadruplet loss等；identification loss就是classification loss，每一个人作为一个类别，来训练分类问题，把某个全连接层作为feature来做行人重识别，一般identification loss能够很快收敛，但是面对百万级，千万级的数据稍显吃力；verification loss就是两张图来做二分类，判断是否为同一个人，这个作为辅助训练还行，作为主训练的loss计算资源消耗巨大，其中constrastive loss 就是其中的一种。在caffe的孪生神经网络（siamese network）中，其采用的损失函数是contrastive loss；attributes loss就是对于不同的属性做分类，比如上身颜色，是否戴帽子之类的，这个是有用的，但是标注数据太过奢侈。

对学者们近年来常用损失函数统计如下：(Zheng, Yang et al. 2016)采用identification loss，直接通过拿身份Label来做多类分类来进行行人重识别[14]。(Li, Zhao et al. 2014, Yi, Lei et al. 2014, Ahmed, Jones et al. 2015)采用的Verification loss来进行的行人重识别研究[8, 10, 12]。(Ding, Lin et al. 2015, Hao, Feng et al.2017, Hermans, Beyer et al. 2017)采用的triplet loss来进行通过距离度量来进行行人重识别[15-17]。

结论
====

在行人重识别的研究过程中，基于深度学习的方法成为当前的研究热点。上文就行人重识别采用的方法做了一个简要的总结。在进行人重识别之前要对行人进行检测，检测过程中涉及到检测算子的阈值问题，当阈值设得过为严格时，则检测会发生遗漏，减少参与匹配计算的影像数量，造成目标包含不全；当阈值设得过为宽松时，则可能会有更多的背景包含进去。这两种结果对行人重识别的结果都不好。另外一个方面就是如何在视频流中定位到个体出现的位置，这个任务要比行人检测、跟踪、重识别简单，不要求有那么高的检测精度，只要能定位就行。

另外在视频中进行行人跟踪相对简单，虽然不可避免的是会存在错误。但是，人脸识别、颜色、非背景信息都有利于提高行人跟踪的准确性。在追踪的过程中，行人会有比较大的变化。运用这些序列图来对行人重识别方法进行训练能大大减少监督学习对于标注数据的依赖。

近几年，主流方法是将检测算法得到的人视作由于多个部分组成的整体，利用深度学习预先对各个部分进行特征提取，然后通过相似性度量来判断行人是否出现过以前的某个位置。

附录
====

这里主要列出行人去重数据集，其中，VIPeRk， Shinpuhkan，PRID，
i-LIDS，3DPeS，CUHK01\~03这些的数据源有不同的场景，且只适用于单一场景，不能用于普适场景的表达。但这些数据集为门店去重数据集的构建提供一个参考依据，整理统计数据集如下表1所示。

> 表1 行人去重数据集统计

| 数据集                           | 个体数量    | 摄像头 数量      | 影像数量    | 标注方法                        | 裁剪大小     | 多场景 捕捉 |
|----------------------------------|-------------|------------------|-------------|---------------------------------|--------------|-------------|
| VIPeR (2007)                     | **632**     | **2**            | **1264**    | **Hand**                        | **128X48**   |             |
| ETH1,2,3 (2007)                  | **148**     | **1**            | **8580**    | **Hand**                        | **Vary**     | **✔**       |
| QMUL iLIDS (2009)                | **119**     | **2**            | **476**     | **Hand**                        | **Vary**     | **✔**       |
| GRID (2009)                      | **1025**    | **8**            | **1275**    | **Hand**                        | **Vary**     |             |
| CAVIAR4ReID (2011)               | **72**      | **2**            | **1220**    | **Hand**                        | **Vary**     | **✔**       |
| 3DPeS (2011)                     | **192**     | **8**            | **1011**    | **Hand**                        | **Vary**     | **✔**       |
| PRID2011 (2011)                  | **934**     | **2**            | **24541**   | **Hand**                        | **128X64**   | **✔**       |
| V47 (2011)                       | **47**      | **2**            | **752**     | **Hand**                        | **Vary**     | **✔**       |
| WARD (2012)                      | **70**      | **3**            | **4786**    | **Hand**                        | **128X48**   | **✔**       |
| SAIVT-Softbio (2012)             | **152**     | **8**            | **64472**   | **Hand**                        | **Vary**     | **✔**       |
| CUHK01 (2012)                    | **971**     | **2**            | **3884**    | **Hand**                        | **160X60**   | **✔**       |
| CUHK02 (2013)                    | **1816**    | **10(5-pairs)**  | **7264**    | **Hand**                        | **160X60**   | **✔**       |
| CUHK03 (2014)                    | **1467**    | **10(5 pairs)**  | **13164**   | **Hand/DPM**                    | **Vary**     | **✔**       |
| RAiD (2014)                      | **43**      | **4**            | **6920**    | **Hand**                        | **128X64**   | **✔**       |
| iLIDS-VID (2014)                 | **300**     | **2**            | **42495**   | **Hand**                        | **Vary**     | **✔**       |
| MPR Drone (2014)                 | **84**      | **1**            |             | **Pyramid Features(ACF)**       | **Vary**     | **✔**       |
| HDA Person Dataset (2014)        | **53**      | **13**           | **2976**    | **Hand/Pyramid Features(ACF)**  | **Vary**     | **✔**       |
| Shinpuhkan Dataset (2014)        | **24**      | **16**           |             | **Hand**                        | **128X48**   | **✔**       |
| CASIA Gait Database B (2015)     | **124**     | **11**           |             | **Background subtraction**      | **Vary**     | **✔**       |
| Market1501 (2015)                | **1501**    | **6**            | **32217**   | **Hand/DPM**                    | **128X64**   | **✔**       |
| PRW (2016)                       | **932**     | **6**            | **34304**   | **Hand**                        | **vary**     | **✔**       |
| Large scale person search (2016) | **11934s**  | **-**            | **34574**   | **Hand**                        | **vary**     |             |
| MARS (2016)                      | **1261**    | **6**            | **1191003** | **DPM+GMMCP**                   | **256X128**  | **✔**       |
| DukeMTMC-reID (2017)             | **1812**    | **8**            | **36441**   | **Hand**                        | **Vary**     | **✔**       |
| DukeMTMC4ReID (2017)             | **1852**    | **8**            | **46261**   | **Doppia**                      | **Vary**     | **✔**       |
| Airport (2017)                   | **9651**    | **6**            | **39902**   | **ACF**                         | **128X64**   | **✔**       |

参考文献

[1] DONG S C, CRISTANI M, STOPPA M, et al. Custom Pictorial Structures for Re-identification; proceedings of the British Machine Vision Conference, F, 2011
[C].

[2] FARENZENA M, BAZZANI L, PERINA A, et al. Person re-identification by symmetry-driven accumulation of local features; proceedings of the Computer Vision and Pattern Recognition, F, 2010 [C].

[3] BAZZANI L, CRISTANI M, PERINA A, et al. Multiple-shot person
re-identification by chromatic and epitomic analyses [J]. Pattern Recognition Letters, 2012, 33(7): 898-903.

[4] XING E P, NG A Y, JORDAN M I, et al. Distance metric learning, with application to clustering with side-information; proceedings of the International Conference on Neural Information Processing Systems, F, 2002 [C].

[5] WEINBERGER K Q, SAUL L K. Distance Metric Learning for Large Margin Nearest Neighbor Classification [M]. JMLR.org, 2009.

[6] DIKMEN M, AKBAS E, HUANG T S, et al. Pedestrian Recognition with a Learned Metric [J]. Lecture Notes in Computer Science, 2010, 6495(501-12.

[7] ZHENG W S, GONG S, XIANG T. Person re-identification by probabilistic relative distance comparison; proceedings of the Computer Vision and Pattern Recognition, F, 2011 [C].

[8] LI W, ZHAO R, XIAO T, et al. DeepReID: Deep Filter Pairing Neural Network for Person Re-identification; proceedings of the Computer Vision and Pattern Recognition, F, 2014 [C].

[9] LIAO S, HU Y, ZHU X, et al. Person re-identification by Local Maximal Occurrence representation and metric learning; proceedings of the Computer Vision and Pattern Recognition, F, 2015 [C].

[10] YI D, LEI Z, LIAO S, et al. Deep Metric Learning for Person
Re-identification; proceedings of the International Conference on Pattern Recognition, F, 2014 [C].

[11] ZHAO R, OUYANG W, WANG X. Person Re-identification by Salience Matching; proceedings of the IEEE International Conference on Computer Vision, F, 2013[C].

[12] AHMED E, JONES M, MARKS T K. An improved deep learning architecture for person re-identification; proceedings of the Computer Vision and Pattern Recognition, F, 2015 [C].

[13] ZHENG L, HUANG Y, LU H, et al. Pose Invariant Embedding for Deep Person Re-identification [J]. 2017.

[14] ZHENG L, YANG Y, HAUPTMANN A G. Person Re-dentification: Past, Present and Future [J]. 2016,

[15] HAO L, FENG J, QI M, et al. End-to-End Comparative Attention Networks for Person Re-Identification [J]. IEEE Transactions on Image Processing A Publication of the IEEE Signal Processing Society, 2017, 26(7): 3492-506.

[16] HERMANS A, BEYER L, LEIBE B. In Defense of the Triplet Loss for Person Re-Identification [J]. 2017,

[17] DING S, LIN L, WANG G, et al. Deep feature learning with relative distance comparison for person re-identification [J]. Pattern Recognition, 2015, 48(10):2993-3003.

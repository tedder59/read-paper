# Read papers
论文阅读笔记

## Table of contents
- [Computer vision related](#computer-vision-related)
  - [R-CNN, UC Berkeley Paper-CVPR14](#rcnn)

## Contents

### Computer vision related

#### RCNN
R-CNN, UC Berkeley [[Paper-CVPR14]](http://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Girshick_Rich_Feature_Hierarchies_2014_CVPR_paper.pdf)
- 标题:

《Rich feature hierarchies for accurate object detection and semantic segmentation》
- 摘要：

  1.先计算region proposal，再通过卷积神经网络对输入图像中的物体进行分割和定位。
  
  2.标注数据不足时，使用辅助任务的预训练网络做fine-tuning能够得到很好的效果。
- 引言：

  1.使用CNN能够符合认知层次模型，并保证了位移不变性。
  
  2.使用Regions解决定位问题。
  
  3.从每个proposal中计算出一个固定长度的特征向量，将其输入到分类SVM中得到结果。
  
  4.RCNN = Regions(Region proposal) + CNN。
  
  5.在大数据量的辅助数据集上有监督pretraining加上少量数据的专用数据集的fine-tuning在数据缺少的情况下很有效。
  
  6.计算效率高，region features在经过两次降维后用于计算非极大值抑制（对于所有的分类结果可以共享）和分类计算。
  
  7.卷积层学习了丰富的特征，而且这些特征不可理解；删除94%参数时，对物体检测的准确率仅有一点点下降。
  
  8.理解检测失败的模型是提升算法模型的关键，simple bounding box regression是本算法模型的检测失败的关键。
  
  9.R-CNN用于regions proposal，经过小小修改后用于语义分割（semantic segmentation）也取得了不错的成绩。
- R-CNN物体检测
  - 模块设计
  
    1.Region proposal : selective search
    
    2.Feature extraction : CNN(AlexNet - 5 convolution + 2 fc)
    
    3.Class-specific linear SVMs
  - 前向过程
  
    1."fast mode" elective search 从输入图片提取2000个region proposals。
    
    2.所有region proposals作为输入，使用CNN从region中提取feature vectors。
    
    3.对于每个feature vector计算各个分类的SVM。
    
    4.计算非极大值，在重叠的region中留下大于learned threshold的最大的那个
  - 性能分析
  
    1.对于所有分类，卷积核的权值都是共享的，使用参数量少，内存占用少。
    
    2.卷积层计算的特征向量都是low-dimentional，计算速度快。
  - 训练
  
    1.在ILSVRC2012数据集上训练AlexNet网络，训练结果:top1错误率比Krizhevsky高2.2%。
    
    2.替换AlexNet最后的1000路分类输出fc为VOC数据集的21路分类输出，随机初始化fc的参数。
    
    3.改装region proposals作为fine-tuning输入，其中与标注box有>=0.5的IoU重叠的为正例，其他的为反例。
    
    4.fine-tuning采用SGD，学习率为0.001（pre-training学习率的1/10）;batch_size=128（其中有分类的标注32个，背景标注96个）。
    
    5.SVM训练需要数据量要少于CNN，overlap的策略采用>=0.7的IoU重叠为正例时的准确率最高。
    
    6.使用Standard hard negative mining method来对每个分类的SVM进行调优。
- 特征可视化

  1.第一层卷积的特征值可以直接看到和被理解，他主要用来捕捉边缘和对比的颜色。
  
  2.Zeiler和Fergus提出了使用deconvolutional方法来显示特征值。
  
  3.在千万个留存的region proposals上计算特征单元的activation，得分高的region和特征相关。
  
  4.pool5中的特征单元能够识别小部分特性的集合，例如形状、文本、颜色和材料等，fc6能够将这些特性再集合成model。
- 阶段分离参数分析

  1.在没有fine-tuning时，CNN的识别能力主要来自于卷积层而不是fc层。
  
  2.在fine-tuning后，fc提供了主要的性能提升。
- 错误分析

  1.Hoiem分析工具发现主要的错误模式是region的定位不准确导致的。
  
  2.使用Bounding box regression方法，训练了一个线性回归的模型用来根据pool5的features预测一个新的检测窗口给selective search做region proposal。
- 语义分割

  1.当前O2P——second-order pooling是语义分割领域中最好的系统，他使用CMPC和SVR。
  
  2.计算CNN的特征在CMPC的regions上的结果，分别有三个策略：忽略region的大小和形状，和物体检测时一样，使用wrap之后的region计算，这样可能的问题时两个region虽然只有很少的重叠，但是会有相似的bounding box；第二种策略，只计算region的前景色，背景色部分设置为0或者均值；第三种策略就是结合前两种策略。
  
  3.使用RCNN的输出特征值训练SVR（20个分类）只需要一个多小时，使用fc6的输出能得到更好的结果。
- 总结

  本文提出了一个简单的可扩展的物体检测算法，比PASCAL VOC 2012最好成绩要提升了30%。第一点洞察是使用CNN为region proposals提供输入来更好的定位和分割物体；第二点洞察是使用Pretraining + finetraining的方法在标注数据不够的情况下也能取得不错的结果。


  

#### RCNN [[Paper-CVPR14]](http://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Girshick_Rich_Feature_Hierarchies_2014_CVPR_paper.pdf)
- 标题: 《Rich feature hierarchies for accurate object detection and semantic segmentation》
- 摘要：

      1.先计算region proposal，根据各个region计算CNN特征，再使用CNN的输出特征值对输入图像中的物体进行分割和定位。
      2.标注数据不足时，使用辅助任务的预训练网络做fine-tuning能够得到很好的效果。
- 引言：

      1.R-CNN = Regions(Region proposal) + CNN。
      2.CNN符合neocognition层次模型，并保证了位移不变性。
      3.输入region proposal到CNN，计算出一个固定长度的特征向量，将其输入到object分类SVM中得到物体的分类。
      4.在大数据量的辅助数据集上有监督pretraining加上少量数据的专用数据集的fine-tuning在数据缺少的情况下很有效。
      5.R-CNN的计算效率高，region features在经过两次降维后再用于非极大值抑制计算，该结果对于所有的分类结果可以共享。
      6.卷积层学习了丰富的特征，删除94%参数时，对物体检测的准确率仅有一点点下降。
      7.理解检测失败的模型是提升算法模型的关键，分析发现使用simple bounding box regression方法可以解决关键问题。
      8.R-CNN的输出特征值还可以用于获得更好的region。
      9.R-CNN经过小修改后用于语义分割也取得了不错的成绩。

- R-CNN物体检测
  - 模块设计
  
        1.Region proposal : selective search
        2.Feature extraction : CNN(AlexNet - 5 convolution + 2 fc)
        3.Class-specific linear SVMs

  - 前向过程
  
        1."fast mode" selective search算法从输入图片提取2000个region proposals。
        2.所有region proposals作为输入，使用CNN从region中提取feature vectors。
        3.对于每个feature vector计算各个分类的SVM。
        4.计算非极大值，在重叠的region中留下大于learned threshold的最大的那个

  - 性能分析
  
        1.对于所有分类，卷积核的权值都是共享的，使用参数量少，内存占用少。
        2.卷积层计算的特征向量都是low-dimentional，计算速度快。

  - 训练
  
        1.在ILSVRC2012数据集上训练AlexNet网络，训练结果:top1错误率比Krizhevsky高2.2%。
        2.替换AlexNet最后的1000路分类输出fc为VOC数据集的21路分类输出，随机初始化fc的参数。
        3.卷积网络fine-tuning的输入是前一步产生的region proposals，所有region被wraped为227X227的固定大小，其中与标注box有>=0.5的IoU重叠的region为正例，其余的region为反例。
        4.fine-tuning采用SGD，学习率为0.001（pre-training学习率的1/10）;batch_size=128（其中有分类的标注32个，背景标注96个）。
        5.SVM训练需要数据量要少于CNN，overlap的策略采用>=0.7的IoU重叠为正例时的准确率最高。
        6.使用Standard hard negative mining method来对每个分类的SVM进行调优。

- 特征可视化

      1.第一层卷积的特征值可以直接看到和被理解，他主要用来捕捉边缘和对比的颜色。
      2.Zeiler和Fergus提出了使用deconvolutional方法来显示特征值。
      3.某个特征值在特定的输入时被激活，则认为该特征值与输入相关，使用这种方法来观察特征值和输入图像的相关性。
      4.pool5中的特征单元能够识别小部分特性的集合，例如形状、文本、颜色和材料等，fc6能够将这些特性再通过线性计算集合成某种model。

- 阶段分离参数分析

      1.在没有fine-tuning时，CNN的识别能力主要来自于卷积层而不是fc层。
      2.在fine-tuning后，fc提供了主要的性能提升。

- 错误分析

      1.Hoiem分析工具发现主要的错误模式是region的定位不准确导致的。
      2.使用Bounding box regression方法，训练了一个线性回归的模型用来根据pool5的features预测一个新的检测窗口给selective search做region proposal。

- 语义分割

      1.当前O2P—second-order pooling是语义分割领域中最好的系统，他使用CMPC和SVR。
      2.用于语义分割时，R-CNN使用CMPC的region作为输入,计算CNN的特征值。此时分别有三个策略：忽略region的大小和形状，和物体检测时一样，使用wrap之后的region计算，这样可能带来的问题是两个region虽然只有很少的重叠，但是可能会有相似的bounding box；第二种策略，只计算region的前景色，背景色部分设置为0或者均值；第三种策略就是结合前两种策略。
      3.使用R-CNN的输出特征值训练SVR（20个分类）只需要一个多小时，比O2P的输出特征值训练要快，另外使用fc6的输出作为SVR的输入能得到更好的结果。

- 总结

      本文提出了一个简单的可扩展的物体检测算法，比PASCAL VOC 2012最好成绩要提升了30%。第一点洞察是使用CNN为region proposals提供输入来更好的定位和分割物体；第二点洞察是使用Pretraining + fine-tuning的方法在标注数据不够的情况下也能取得不错的结果。

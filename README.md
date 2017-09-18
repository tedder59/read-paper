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

  1.SIFT和HOG不适合neocognitron的认识层次，使用CNN能够获得认知层次模型，并保证了位移不变性。
  
  2.使用“Recognition using regions"解决定位问题。
  
  3.从每个proposal中计算出一个固定长度的特征向量，将其输入到分类SVM中得到结果。
  
  4.RCNN = Regions(Region proposal) + CNN。
  
  5.在大数据量的辅助数据集上有监督pretraining加上少量数据的专用数据集的fine-tuning在数据缺少的情况下很有效。
  
  6.计算效率高，region features在经过两次降维后用于计算非极大值抑制（对于所有的分类结果可以共享）和分类计算。
  
  7.卷积层学习了丰富的特征，而且这些特征不可理解；删除94%参数时，对物体检测的准确率仅有一点点下降。
  
  8.理解检测失败的模型是提升算法模型的关键，simple bounding box regression是本算法模型的检测失败的关键。
  
  9.R-CNN用于regions proposal，经过小小修改后用于语义分割（semantic segmentation）也取得了不错的成绩。
- R-CNN物体检测
  - 模块设计
  
    1.Region proposal : selective search(很草率的选择)
    
    2.Feature extraction : CNN(Krizhevsky - 5 convolution + 2 fc)
    
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
  
    1.在ILSVRC2012数据集上训练CNN网络。
    
    2.使用从VOC数据集的提取出的region proposals数据作为输入，采用SGD训练CNN网络参数。先使用随机初始化的21路分类输出替换掉第一步的1000路分类输出，batch_size=128（其中正例32个，反例96个）。
    
    3.使用IoU threshold解决region部分覆盖物体的问题，大于threshold判为正例，小于threshold判为反例。当特征提取出来之后使用hard negative mining method方法优化线性SVM。
- 

  

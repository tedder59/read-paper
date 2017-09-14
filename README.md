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

  1.使用卷积神经网络做region proposal对输入图像中的物体进行分割和定位。
  
  2.标注数据不足时，使用辅助任务的预训练网络做fine-tuning能够得到很好的效果。
- 引言：

  1.SIFT和HOG不适合neocognitron的认识层次，使用CNN能够获得认知层次模型，并保证了位移不变性。
  
  2.使用“Recognition using regions"解决定位问题。
  
  3.从每个proposal中计算出一个固定长度的特征向量，将其输入到分类SVM中得到结果。
  
  4.RCNN = Regions(Region proposal) + CNN。
  
  5.在大数据量的辅助数据集上有监督pretraining加上少量数据的专用数据集的fine-tuning在数据缺少的情况下很有效。
  
  6.计算效率高，region features在经过两次降维后用于计算非极大值抑制（对于所有的分类结果可以共享）和分类计算。
  
  7.卷积层学习了丰富的特征，而且这些特征不可理解；大量的参数都可以删除，对物体检测的准确率仅有一点点下降。
  
  8.理解检测失败的模型是提升算法模型的关键，simple bounding box regression是本算法模型的检测失败的关键。
  
  9.R-CNN用于regions proposal，经过小小修改后用于语义分割（semantic segmentation）也取得了不错的成绩。
- R-CNN的组成模块

  

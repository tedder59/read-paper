#### ZFnet [[arXiv]](https://arxiv.org/pdf/1311.2901.pdf)
- 标题：
《Visualizing and Understanding Convolutional Networks》
- 摘要：

      1.了解卷积网络在ImageNet测试中表现出色的原因和提升性能的方法是本文的研究课题。
      2.本文介绍了一种可视化技术，能够看到分类器的中间特征和运算。我们可以发现网络模型的架构，以及不同的layer产生的作用。
      3.我们的ImageNet网络模型具有较好的泛化能力，能够同时在Caltech-101和Caltech-256数据集上取得好成绩。
- 引言：

        1.我们使用了多层反卷积网络将特征值反运算到输入的像素空间来实现可视化操作。
        2.使用可视化工具分析AlexNet，我们发现模型泛化的可能性，即只需要再训练顶层的softmax分类器，这个就是supervised pre-training。
        3.和着眼于图形变换的分析不同，我们直接考虑高层的特征值和输入的关系，图像中的每一块图形会激活特定的特征值。
- 实现：
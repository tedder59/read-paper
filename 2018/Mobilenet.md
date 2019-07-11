#### MobileNetV2: Inverted Residuals and Linear Bottlenecks
- 摘要：

      mobile architecture, MobileNetV2, that improves the state of the art performance of mobile models on multiple tasks and benchmarks as well as across a spectrum of different model sizes. We describe efficient ways of applying these mobile models to object detection in novel framework we call SSDLite. We demonstrate how to build mobile semantic segmentation models through a reduced form of DeepLabv3 which we call Mobile DeepLabv3.
      
      based on an inverted residual structure where the shortcut connections are between the thin bottleneck layers. It's important to remove non-linearities in the narrow layers in order to maintain representational power. We demonstrate that this improves performance and provide an intuition that led to this design.
      
      decoupling input/output domains from the expressiveness of the transformation. metrics: accuracy, operations measured by multiply-adds(MAdd), actual latency, number of parameters.

- 相关工作：
    
    Tuning deep neural architectures to strike an optimal balance between accuracy and performance.Both manual architecture search and improvements in training algorithms, carried out by numerous teams has lead to dramatic improvements. Recently there has been lots of progress in algorithmic architecture exploration included **hyper-parameter optimization** as well as various methods of **network pruning** and **connectivity learning**, substantial amount of work include **ShuffleNet** and **sparsity**

- 准备工作(Preliminaries, discussion and intuition):

    Depthwise Separable Convolutions: The basic idea to replace  a full convolutional operator with a factorized version that splits convolution into two separate layers. The first layer is called 
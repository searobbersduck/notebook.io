# 如何将pytorch模型转换为tensorrt能够挂载的模型

## 安装必要的包
1. 安装pytorch/tensorflow/onnx/onnx_tf
2. python环境下，为了便于安装，可以将安装镜像改为国内的镜像，具体方法参见[link](https://github.com/searobbersduck/notebook.io/blob/master/%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3conda%E5%AE%89%E8%A3%85%E5%8C%85%E9%80%9F%E5%BA%A6%E6%85%A2%E7%9A%84%E9%97%AE%E9%A2%98.md)
3. 安装时，直接pip安装即可，以下例子中所用到的版本`tensorflow-gpu==1.15`,`onnx_tf==1.3`
4.

## 模型转换
1. pytorch模型与tensorflow之间的转换需要中间协议onnx，我们的转换步骤为：
    * pytorch模型转换为onnx模型
    * 需要将onnx模型的batch size，由固定转为可变
    * 将onnx模型转换为tensorflow模型，此时的tensorflow模型为frozen的形式，当需要tensorflow serving或tensorrt挂载的时候，还需要进一步

## 过程中遇到的问题
1. ***pyFunc相关的问题***， 主要时因为pytorch做pooling的方式和tensorflow中pooling的VALID/SAME模型不同，一般在resnet等模型，第一层做pooling时，可以将pooling的padding改为0
2. ***pytorch模型转换为onnx模型时，需要将固定的batch size转换为动态的batch size***


## reference

1. [Dynamic dummy input when exporting a PyTorch model? ](https://github.com/onnx/onnx/issues/654)
    * pytorch 转 onnx时，batch_size 固定

2. [使用ONNX转换PyTorch模型到 Tensorflow *.pb文件](https://zhuanlan.zhihu.com/p/57833679)

3. [Tensorflow的三种储存格式-2（pb & Saved Model）](https://zhuanlan.zhihu.com/p/60069860)

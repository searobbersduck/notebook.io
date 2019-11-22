# 如何将pytorch模型转换为tensorrt能够挂载的模型

## 安装必要的包
1. 安装pytorch/tensorflow/onnx/onnx_tf
2. python环境下，为了便于安装，可以将安装镜像改为国内的镜像，具体方法参见[link](https://github.com/searobbersduck/notebook.io/blob/master/%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3conda%E5%AE%89%E8%A3%85%E5%8C%85%E9%80%9F%E5%BA%A6%E6%85%A2%E7%9A%84%E9%97%AE%E9%A2%98.md)
3. 安装时，直接pip安装即可，以下例子中所用到的版本`tensorflow-gpu==1.15`,`onnx_tf==1.3`
4. 说明，`tensorflow-gpu==1.13`以上的版本就可以对应`onnx_tf==1.3`，要确保转换模型时，所用的tensorflow版本和挂载模型的trt docker中的tensorflow版本一致，否则会报错。

## 模型转换
1. pytorch模型与tensorflow之间的转换需要中间协议onnx，我们的转换步骤为：
    * pytorch模型转换为onnx模型
    * 需要将onnx模型的batch size，由固定转为可变
    * 将onnx模型转换为tensorflow模型，此时的tensorflow模型为frozen的形式，当需要tensorflow serving或tensorrt挂载的时候，还需要进一步

2. pytorch模型转换为onnx模型
```
import os
import sys
sys.path.append('../')
from resnet import *
import torch

model = resnet34(num_classes=6, shortcut_type=True, sample_size=128, sample_duration=128)
weights = '../model/ct_pos_recogtion_20191115125435/ct_pos_recognition_0009_best.pth'
model.load_state_dict(torch.load(weights))

dummy_input = torch.randn(1,1,128,128,128)

torch.onnx.export(model, dummy_input, 'ctPosRecognition.onnx', verbose=True, input_names=['input'], output_names=['output'])

print('====> export to onnx model!')
```

3. onnx模型转换为pb
```
import onnx
from onnx_tf.backend import prepare

onnx_model = onnx.load('ctPosRecognition.onnx')
onnx_model.graph.input[0].type.tensor_type.shape.dim[0].dim_param = '?'
onnx.save(onnx_model, 'dynamic_model.onnx')

# 如果在jupyter或ipython中运行，需要通过CUDA_VISIBLE_DEVICES=0或os.environ['CUDA_VISIBLE_DEVICES']='0'，指定一个gpu
# 否则会报xla相关的错误
tf_rep = prepare(onnx_model)

print(tf_rep.inputs)
print(tf_rep.outputs)
print(tf_rep.tensor_dict)

print('\n====> inputs:')
for inp in tf_rep.inputs:
    print(tf_rep.tensor_dict[inp])

print('\n====> outputs:')
for outp in tf_rep.outputs:
    print(tf_rep.tensor_dict[outp])

tf_rep.export_graph('ctPosRecognition.pb')
print('export to frozen pb!')
```

4. pb文件转换为tensorflow serving或tensorrt挂载的文件
```
import os
import tensorflow as tf

infile = 'ctPosRecognition.pb'

graph = tf.get_default_graph()
sess = tf.Session()

with open(infile, 'rb') as f:
    graph_def = tf.GraphDef()
    graph_def.ParseFromString(f.read())
    tf.import_graph_def(graph_def, name='')

inp = sess.graph.get_tensor_by_name('input:0')
print('====> input:\t')
print(inp)
outp = sess.graph.get_tensor_by_name('add_17:0')
print('====> output:\t')
print(outp)

export_path = './export'
builder = tf.saved_model.builder.SavedModelBuilder(export_path)

tensor_info_input = tf.saved_model.utils.build_tensor_info(inp)
tensor_info_output = tf.saved_model.utils.build_tensor_info(outp)

#定义签名
prediction_signature = (
    tf.saved_model.signature_def_utils.build_signature_def(
    inputs={'images': tensor_info_input},
    outputs={'result': tensor_info_output},
    method_name=tf.saved_model.signature_constants.PREDICT_METHOD_NAME))

# 'serving_default' 可以随意定义，但是直接利用系统的trt框架挂载，默认要填'serving_default'
builder.add_meta_graph_and_variables(sess, [tf.saved_model.tag_constants.SERVING],signature_def_map={'serving_default': prediction_signature})
builder.save()
print('Done exporting!')
```

## 检查模型的输出结果


## 过程中遇到的问题
1. ***pyFunc相关的问题***， 主要时因为pytorch做pooling的方式和tensorflow中pooling的VALID/SAME模型不同，一般在resnet等模型，第一层做pooling时，可以将pooling的padding改为0
2. ***pytorch模型转换为onnx模型时，需要将固定的batch size转换为动态的batch size***
3. ***当使用多块GPU卡进行onnx转换为tf的时候，可能会报xla的错误***,可以通过环境变量的方式，将GPU限定到一块卡，`CUDA_VISIBLE_DEVICES=0`或`os.environ['CUDA_VISIBLE_DEVICES']='0'`


## reference

1. [Dynamic dummy input when exporting a PyTorch model? ](https://github.com/onnx/onnx/issues/654)
    * pytorch 转 onnx时，batch_size 固定

2. [使用ONNX转换PyTorch模型到 Tensorflow *.pb文件](https://zhuanlan.zhihu.com/p/57833679)

3. [Tensorflow的三种储存格式-2（pb & Saved Model）](https://zhuanlan.zhihu.com/p/60069860)

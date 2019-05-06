# 利用anaconda安装blocksparse安装过程中遇到的问题

1. 安装tensorflow-gpu
```
    conda create --name sparse python=3.6
    source activate sparse
    conda install -c anaconda tensorflow-gpu 
```
上述代码实际为了简化cuda10的安装，如果直接利用上述tensorflow,在安装blocksparse会遇到如下问题：
```
>>> import blocksparse
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/ubuntu/anaconda3/envs/sparse/lib/python3.6/site-packages/blocksparse/__init__.py", line 3, in <module>
    from blocksparse.utils import (
  File "/home/ubuntu/anaconda3/envs/sparse/lib/python3.6/site-packages/blocksparse/utils.py", line 16, in <module>
    _op_module = tf.load_op_library(os.path.join(data_files_path, 'blocksparse_ops.so'))
  File "/home/ubuntu/anaconda3/envs/sparse/lib/python3.6/site-packages/tensorflow/python/framework/load_library.py", line 61, in load_op_library
    lib_handle = py_tf.TF_LoadLibrary(library_filename)
tensorflow.python.framework.errors_impl.NotFoundError: /home/ubuntu/anaconda3/envs/sparse/lib/python3.6/site-packages/blocksparse/blocksparse_ops.so: undefined symbol: _ZN10tensorflow12OpDefBuilder4AttrESs
>>>
```
原因是：[undefined symbol: _ZN10tensorflow12OpDefBuilder4AttrESs](https://github.com/horovod/horovod/issues/656)
```
i think i find a way to deal this error
ues pip install tensorflow except conda install tensorflow
i dont know why but this really work for me ,you can infer this issue #431
if your cuda is 9.2 you cant pip ,so change cudu to 9.0 or 8.0
```

2. 解决方案：`pip install tensorflow-gpu`
3. `pip install blocksparse`

如上三部，可成功安装`blocksparse`
# 如何利用vscode远程调试python（anaconda环境适用）

## 

参考配置：
1. [vscode下Python设置参考](https://www.cnblogs.com/it-tsz/p/9346110.html)
2. [用VScode代码调试Python](https://www.cnblogs.com/it-tsz/p/9022456.html)


## 遇到的问题

1. 代码不能跳转到定义

2. 多进程调试出错1
```
E00018.868: Exception escaped from start_client
            
            Traceback (most recent call last):
              File "/home/zhangwd/.vscode-server/extensions/ms-python.python-2019.11.50794/pythonFiles/lib/python/old_ptvsd/ptvsd/log.py", line 110, in g
                return f(*args, **kwargs)
              File "/home/zhangwd/.vscode-server/extensions/ms-python.python-2019.11.50794/pythonFiles/lib/python/old_ptvsd/ptvsd/pydevd_hooks.py", line 74, in start_client
                sock, start_session = daemon.start_client((host, port))
              File "/home/zhangwd/.vscode-server/extensions/ms-python.python-2019.11.50794/pythonFiles/lib/python/old_ptvsd/ptvsd/daemon.py", line 214, in start_client
                with self.started():
              File "/home/zhangwd/.conda/envs/py36/lib/python3.6/contextlib.py", line 81, in __enter__
                return next(self.gen)
              File "/home/zhangwd/.vscode-server/extensions/ms-python.python-2019.11.50794/pythonFiles/lib/python/old_ptvsd/ptvsd/daemon.py", line 110, in started
                self.start()
              File "/home/zhangwd/.vscode-server/extensions/ms-python.python-2019.11.50794/pythonFiles/lib/python/old_ptvsd/ptvsd/daemon.py", line 145, in start
                raise RuntimeError('already started')
            RuntimeError: already started
```
解决方案：
[subprocess: Support for fork in subprocess debugging #943](https://github.com/microsoft/ptvsd/issues/943)
在调试文件的最前面，加上
```
import multiprocessing
multiprocessing.set_start_method('spawn', True)
```
[Exception when debugging code with multiprocessing module #5251](https://github.com/microsoft/vscode-python/issues/5251)
[when I debug, I meet: Runtime Error: Already started in multiprocess #1761](https://github.com/microsoft/ptvsd/issues/1761)
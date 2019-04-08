# jupyter 安装过程中遇到的问题

1. conda环境下，初始使用python2时，需要安装python3
	* 首先安装python 3 环境 ```conda create --name py36 python=3.6```
```
source activate py36
conda install jupyter
conda install ipython
```

2. 远程访问jupyter notebook的问题
	* [jupyter notebook远程访问不了的问题解决](https://blog.csdn.net/cc1949/article/details/79095494)
	* [jupyter 远程访问配置：KeyError: 'allow_remote_access'](https://blog.csdn.net/w5688414/article/details/82927564)
	
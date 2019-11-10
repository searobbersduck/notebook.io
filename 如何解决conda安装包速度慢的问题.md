# 如何解决conda安装包(如pytorch、tensorflow)速度慢的问题

## 添加源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/


conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

## 安装tensorflow
1. [win10+cuda10.1+cudnn7.5+anaconda3 安装 tensorflow-gpu](https://blog.csdn.net/u011736505/article/details/92806922)

## 坚持是否利用了gpu
1. [pytorch中查看gpu信息](https://blog.csdn.net/nima1994/article/details/83001910)
2. [检测tensorflow是否使用gpu进行计算](https://blog.csdn.net/castle_cc/article/details/78389082)

## github下载慢
1. [Windows的hosts文件在哪里？](https://blog.csdn.net/cc1949/article/details/78411865)
2. [解决github访问或下载慢的问题](https://juejin.im/post/5bc304735188255c3633472e)
	* 未解决问题，github.com的ip没有查询到
待解决。。。。


## 参考
1. [conda安装Pytorch下载过慢解决办法](https://blog.csdn.net/watermelon1123/article/details/88122020)
2. [安装tensorflow-gpu速度慢](https://blog.csdn.net/daisy1033/article/details/87161833)
3. [安装指定版本的tensorflow-gpu](https://blog.csdn.net/Young__Fan/article/details/88130385)

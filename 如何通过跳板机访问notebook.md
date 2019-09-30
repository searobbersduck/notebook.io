1. ssh -4 -N -f -L localhost:8***:localhost:8888 ubuntu@172.20.1.20
	* 建立跳板机与服务器之间的连接，第一个port（8***）为跳板机端口，可自由设置，第二个port为服务器（jupyter notebook）端口, 最后一个ip为服务器ip
2. ssh -4 -N -f -L localhost:8***:localhost:8888 ubuntu@192.168.4.86
	* 建立本地与跳板机之间的连接

# reference
1. [ssh远程登录Jupyter notebook（七月GPU服务器）](https://www.cnblogs.com/koliverpool/p/6536637.html)
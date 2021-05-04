## docker安装（ubuntu 18.04)
直接通过脚本安装
```bash
sudo apt install curl			#主要是为了后面通过curl获取docker安装脚本。
curl -fsSL get.docker.com -o get-docker.sh		#获取脚本，写入文件get-docker.sh
sudo sh get-docker.sh --mirror Aliyun			#运行脚本，运行的时候使用阿里云的镜像
```
实际使用的时候curl遇到了网络问题，后面看估计是sock5代理导致的，关闭代理后正常。
安装成功后，通过`docker --version`可以查看安装是否成功，以及安装的docker版本。
后面经过查验发现果然可以使用wget替代curl。具体命令如下：
```bash
 wget -qO- https://get.docker.com/ | sh
```

## docker的使用
使用docker前首先要进行环境配置。一般而言我们是在非root用户环境下使用docker，因此需要将当前用户添加到docker组中。  
```bash
# 查询是否存在docker用户组
groups | grep docker
# 如不存在，建立docker用户组。该docker组可能会由脚本自行创建。
sudo groupadd docker
# 将当前用户加入docker组,如果需要添加其他用户，替换语句中的用户名即可
sudo usermod -aG docker $USER
```

#### 构建镜像与进入容器
```bash
# 使用docker build命令构建镜像。-t后为镜像的名称，可任意命名。最后一个参数为构建docker脚本文件路径。
sudo docker build -t <IMAGE_NAME> <DockerFile_Path>
# 使用docker run命令以交互式方式进入容器。-i表示交互式，-t应该是指定使用终端，最后一个为指定的终端应用。使用bash终端。
docker run -it <IMAGE_NAME> /bin/bash
```

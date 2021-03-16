# Git VPN 配置--ubuntu18.04

## 通过终端配置shadowsocks代理

1. 安装shadowsocks  
	```bash
	sudo apt-get install python
	sudo apt-get install python-pip
	sudo pip install shadowsocks
	```

2. 配置shadowsocks  
	在任意位置新建文件`shadowsocks.json `,然后输入如下配置。其中只有本地端口和超时时间比较固定，其他配置要看具体服务器。
	``` json
	{
		"server": "{服务器域名}",
		"server_port": {服务器端口},
		"local_port": 1080,
		"password": "{服务器密码}",
		"timeout": 600,
		"method": "{加密协议}"
	}
	```

3. 安装客户端  
	shadowsocks需要配合客户端使用才能使用。客户端有多种选择，这里使用`polipo`。  
	使用命令`sudo apt-get install polipo`安装客户端。

4. 配置客户端  
	修改polipo的配置文件`/etc/polipo/config`
	```bash
	# 这两行原本就有，无需修改
	logSyslog = true
	logFile = /var/log/polipo/polipo.log

	# 后面几行为新添加的，如shadowsocks配置本地端口一样，这边直接拷贝即可
	proxyAddress = "0.0.0.0"

	socksParentProxy = "127.0.0.1:1080"
	socksProxyType = socks5

	chunkHighMark = 50331648
	objectHighMark = 16384

	serverMaxSlots = 64
	serverSlots = 16
	serverSlots1 = 32
	```
5. 启动代理服务，设置代理  
	```bash
	# 启动shadowsocks。进入shawdowsocks.json文件所在目录，执行如下命令。
	sudo sslocal -c shawdowsocks.json -d start

	# 启动polipo
	sudo /etc/init.d/polipo restart

	# 配置http和https代理。注意这里的端口应该是可以为任意空闲端口
	export http_proxy="http://127.0.0.1:8123/"
	export https_proxy="https://127.0.0.1:8123/"

	# 测试是否能正常翻墙
	wget www.youtube.com
	wget www.google.com
	```

6. 设置git代理  
	git的配置文件在路径`~/.gitconfig`  
	可以通过如下命令设置  
	```bash
	git config --global http.proxy 'socks5://127.0.0.1:1080'
	git config --global https.proxy 'socks5://127.0.0.1:1080'
	```
	要删除git代理，可以通过如下命令  
	```bash
	git config --global --unset http.proxy
	git config --global --unset https.proxy
	```

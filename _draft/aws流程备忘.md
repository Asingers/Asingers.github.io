安装Node,npm:

	sudo yum install nodejs npm --enablerepo=epel

更新最细版本: 

	npm install npm -g


可能会报错,先执行

	npm install -g hubot coffee-script
再更新:

	npm install npm -g
安装ss:
	
	npm install -g shadowsocks
配置:
	
	
	sudo vim /usr/lib/node_modules/shadowsocks/config.json

启动:
	
	ssserver -c /usr/lib/node_modules/shadowsocks/config.json
	
or

	后台运行: nohup ssserver -c /usr/lib/node_modules/shadowsocks/config.json > /dev/null 2>&1 &
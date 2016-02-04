1.Linux下目录复制：本机->远程服务器
scp  -r /home/shaoxiaohu/test1  zhidao@192.168.0.1:/home/test2 
#test1为源目录，test2为目标目录，zhidao@192.168.0.1为远程服务器的用户名和ip地址。
2.Linux下目录复制：远程服务器->本机
scp  -r zhidao@192.168.0.1:/home/test2 /home/shaoxiaohu/test1
#zhidao@192.168.0.1为远程服务器的用户名和ip地址，test1为源目录，test2为目标目录。
注：如果端口号有更改，需在scp 后输入：-P 端口号 （注意是大写，ssh的命令中 -p是小写）
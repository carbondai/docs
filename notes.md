### 查看某个进程的具体启动时间
`ps -p PID -o lstart`

### 查看端口被哪个进程占用
`netstat -nap`
`lsof -i :portnumber`
`netstat -lnp | grep portnumber`
`ps portnumber`

### centos7下docker由systemd管理，查看systemd配置可知其配置文件位置
`cat /usr/lib/systemd/system/docker.service`

### docker中运行的ubuntu16.04无法上网，运行时配置dns解决
`docker run --dns 8.8.8.8 -t -i -d aarch64/ubuntu /bin/bash`

### docker容器与主机之间相互拷贝文件
从容器到主机：`docker cp 容器id：绝对路径 主机目的路径`
从主机到容器：`docker cp 主机文件 容器id：绝对路径 `
获取完整的容器ID：`docker inspect '{{.Id}}' 容器名`

### 查看网络中是否有IP冲突
`arping -I 端口 -b ip地址`

### 利用iptables命令改变端口转发
`iptables -t nat -A  DOCKER -p tcp --dport 443 -j DNAT --to-destination 192.168.0.191:5000`

### 从一个文本文件中取出特定的行重定向到一个新文件中
`awk 'NR==m, NR==n' file1 > file2` 取出file1中从m行到n行的文本放入file2中

### 虚拟机与局域网直连
1. 场景：
虚拟机ip：192.168.122.160
宿主机ip：192.168.0.211 其虚拟网桥ip：192.168.122.1
局域网调试机ip：192.168.0.183
2. 需求：
让虚拟机和调试机直连能ping通
3. 解决方法：
在调试机上加一条静态路由
 `route add -net 192.168.122.0 netmask 255.255.255.0 gw 192.168.0.211 dev eno1`
在centos7中，开始使用ip route来代替route命令了，上面这条命令可以用
`ip route add 192.168.122.0/24 via 192.168.0.211 dev eno1`来代替
用命令添加的重启机器就失效了，如果想要永久，需要在/etc/sysconfig/network-scripts/目录下创建对应的
route-eno1文件，在其中添加
`192.168.122.0/24 via 192.168.0.211 dev eno1`，
然后重启机器或者
`ifdown eno1 && ifup eno1`

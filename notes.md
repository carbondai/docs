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

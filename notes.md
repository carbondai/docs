##### 查看某个进程的具体启动时间
`ps -p PID -o lstart`

##### 查看端口被哪个进程占用
`netstat -nap`
`lsof -i :portnumber`
`netstat -lnp | grep portnumber`
`ps portnumber`

##### centos7下docker由systemd管理，查看systemd配置可知其配置文件位置
`cat /usr/lib/systemd/system/docker.service`

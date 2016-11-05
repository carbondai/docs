*2016.10.14*  
与中标麒麟沟通，查找A2服务器计算板无法安装操作系统的问题，会卡在mirror_storage_for_webmin安装包处。
alt+ctrl+F2进入liveos终端，chroot /sys/sysmanger，可以查看到install.log，分析该安装日志，应是bios时间太旧，将其改为当前时间，能顺利安装该软件包，完成系统安装。

*2016.10.17-21*  
1. 在ft1500a平台，centos7系统上编译通过etcd、swarm并做成docker镜像，编译通过rethinkdb、
shipyard-controller
2. 编译rethinkdb过程中，更改过configure，mk/support/pkg/v8.sh等文件，主要和平台架构相关部分。

*2016.10.24-28*  
1. 电科院显控机安装数据库之后，需要执行`sysv-rc-conf kingbase7d on`以确保下次数据库服务能自动运行。安装vino后在每个用户（普通用户和root用户）下执行`gsettings set org.gnome.Vino require-encryption false`，打开/etc/xdg/autostart/vino-server.desktop文件，查看OnlyShowIn项，是否包含MATE，若不含，添加，
注销或重新启动系统。vino-preference可以进行参数配置。
2. 最终发现rethinkdb不能支持aarch64，虽然修改`src/arch/runtime/context_switching.cc`后能编译通过，但是软件启动后无法提供正常服务。拟改用mongodb作为后台数据库软件，需要对shipyard代码进行大的改动。


*2016.10.14*
与中标麒麟沟通，查找A2服务器计算板无法安装操作系统的问题，会卡在mirror_storage_for_webmin安装包处。

*2016.10.17-21*
1. 在ft1500a平台，centos7系统上编译通过etcd、swarm并做成docker镜像，编译通过rethinkdb、
shipyard-controller
2. 编译rethinkdb过程中，更改过configure，mk/support/pkg/v8.sh等文件，主要和平台架构相关部分。

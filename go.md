### 关于包package
一般包即为当前go文件所在的目录名，每个源文件都以package声明语句开始。main包比较特殊，它定义一个独立可执行的程序而不是一个库。

### 安装intellij ide用于go程序开发
安装完后，打开setting->plugins->browse repositories->manager repositories添加
https://plugins.jetbrains.com/plugins/alpha/5047，找到go插件，安装后设置go sdk即可

### 下载开源代码的go程序编译
需要将目录下的Godeps/_workspace/加入到$GOPATH中，例如：
`export GOPATH=/home/daixin/Public/go:/home/daixin/Public/go/Godeps/_workspace`

### 写文件
```
fo,err := os.Create("filename")
if err != nil {panic(err)}
defer fo.Close()
```

### 导入包
如果一个包被导入，其中的函数需要被调用，那么该函数首字母必须大写，导入包不能是main文件所在目录。

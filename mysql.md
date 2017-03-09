### 查看数据表编码
show create table 表名;

### 修改数据表的编码格式
修改数据库字符集：
`ALTER DATABASE db_name DEFAULT CHARACTER SET character_name [COLLATE ...];`
把表默认的字符集和所有字符列（CHAR,VARCHAR,TEXT）改为新的字符集：
```
ALTER TABLE tbl_name CONVERT TO CHARACTER SET character_name [COLLATE ...]
如：ALTER TABLE logtest CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;
```
只是修改表的默认字符集：
```
ALTER TABLE tbl_name DEFAULT CHARACTER SET character_name [COLLATE...];
如：ALTER TABLE logtest DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```
修改字段的字符集：
```
ALTER TABLE tbl_name CHANGE c_name c_name CHARACTER SET character_name [COLLATE ...];
如：ALTER TABLE logtest CHANGE title title VARCHAR(100) CHARACTER SET utf8 COLLATE utf8_general_ci;
```
查看数据库编码：
`SHOW CREATE DATABASE db_name;`
查看表编码：
`SHOW CREATE TABLE tbl_name;`
查看字段编码：
`SHOW FULL COLUMNS FROM tbl_name;`

### phpmyadmin安装配置
首先需要apache环境，在centos7中，名叫“httpd”,其配置文件在/etc/httpd/conf下。
然后安装php，并在httpd.conf中做相应配置：
需要修改Apache的配置文件httpd.conf以得到PHP的解析：
1. 在LoadModule中添加：
    `LoadModule php5_module     modules/libphp5.so`
2. 在AddType application/x-gzip .gz .tgz下面添加：
```   
   # probably should define those extensions to indicate media types:
    #
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz
AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps
```   
3. 在DirectoryIndex增加 index.php，以便Apache识别PHP格式的index
```
<IfModule dir_module>  
    DirectoryIndex index.html index.php  
</IfModule>  
```

### java中使用mysql连接
需要下载并导入mysql-connector-java.jar包，驱动名为`com.mysql.jdbc.Driver`，连接路径为`jdbc:mysql://127.0.0.1:3306/database`

### mybatis通过接口与配置文件实现数据库操作
1. 接口名与配置文件namespace的值相同(自定义的名字，只要一致就行)
2. 方法名与查询语句的id相同
3. 方法参数与parameterType的返回类型相同
4. 返回值的类型与resultMap类型相同
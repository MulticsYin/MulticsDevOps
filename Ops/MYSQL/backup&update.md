## MySQL 备份 & 二进制升级

### 备份(使用mysqldump命令)

备份特定数据库：mysqldump -h主机名  -P端口 -u用户名 -p密码 –database 数据库名 > 文件名.sql   
备份所有数据库：mysqldump –-all-databases > allbackupfile.sql  
注：可写成脚本备份，终端不要直接输入密码，或者备份每个数据库时单独输入密码。

### 还原MySQL数据库的命令
```
mysql -hhostname -uusername -ppassword databasename < backupfile.sql
```

## 二进制升级



### 升级后设置初始密码
```
SET PASSWORD = PASSWORD('your_new_password');
```
### 允许其他主机登录
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'your_new_password' WITH GRANT OPTION;
```

## 参考
[CentOS 6.8下MySQL 5.7.14二进制安装详解](https://www.linuxidc.com/Linux/2017-05/144043.htm)

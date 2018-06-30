## MySQL 备份 & 二进制升级

### 备份

**备份命令mysqldump格式**  

备份特定数据库：mysqldump -h主机名  -P端口 -u用户名 -p密码 –database 数据库名 > 文件名.sql   
备份所有数据库：mysqldump –all-databases > allbackupfile.sql  
注：可写成脚本备份，终端不要直接输入密码，或者备份每个数据库时单独输入密码。

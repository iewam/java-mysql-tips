# java-mysql-tips
Mysql存储emoji表情报错（Incorrect string value: '\xF0\x9F\x98\x82\xF0\x9F...'）的解决方案

### 问题分析

- 普通的字符串或者表情都是占位3个字节，所以utf8足够用了，但是移动端的表情符号占位是4个字节，普通的utf8就不够用了，为了应对无线互联网的机遇和挑战、避免 emoji 表情符号带来的问题、涉及无线相关的 MySQL 数据库建议都提前采用 utf8mb4 字符集，这必须要作为移动互联网行业的一个技术选型的要点
- Mysql 版本的限制，Mysql 5.5.3之前的版本，支持的utf8为3字节的，Mysql 5.5.3之后的版本支持utf8mb4

### 解决方案

- 修改mysql的配置文件，windows下的为my.ini(linux下的为my.cnf)，修改的内容都一样

```
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

- 将数据库中对应的字段，改为utf8mb4_general_ci
- 修改项目中的连接数据库的url，将characterEncoding=utf-8去掉
- 重启mysql：systemctl restart mysqld （systemctl stop mysqld / systemctl start mysqld）

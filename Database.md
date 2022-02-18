# MySQL

## 初始操作相关

- 初次安装需要进行初始化 `mysql_secure_installation`
- MySQL 默认对本地 root 用户使用本地验证，没有开启密码登录
- 新建数据库 `create DATABASE my_data;`
- 新建用户 `create user 'tinker'@'localhost' identified by 'password';`
- 授予新用户权限 `grant ( privileges | all ) on my_data.* to 'tinker'@'localhost'`
##  密码修改

- MySQL 用户插件主要有 `auth_socket`, `mysql_native_password`

- 修改 root 用户插件 `update mysql.user set plugin='mysql_native_password' where user='root' and host='localhost';`

- 显示密码相关策略`show variables like 'validate_password%';`

- 修改对应策略 `set global validate_password_policy=0;` `set global validate_password_length=4`



# Redis

## Redis 认证

- 查看密码：`config get requirepass`
- 修改密码：`config get requirepass password`
- 密码认证： `auth passowrd`


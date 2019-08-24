# mysql主从配置
### master 执行
- 创建账号并授权
`
GRANT REPLICATION SLAVE ON *.* TO 'slave173'@'%' IDENTIFIED BY '123456';
`
- 刷新权限
`
flush privileges;
`
- 锁库
`
FLUSH TABLES WITH READ LOCK;
`
- 查看binlog并记录位点
`
SHOW MASTER STATUS;
`
- 解锁数据库
`
UNLOCK TABLES;
`

### slave执行
- 停止主从关联
`
stop slave;
`
- 指定主库及同步开始binlog位点
`
change master to master_host='192.168.31.147',master_user='slave173',master_password='123456',master_log_file='mysql-bin.000003',master_log_pos=1268;
`
- 启动从库
`
start slave; 
`
- 查看从库状态
`
show slave status;
`


- 以下两个都为yes为成功启动

Slave_IO_Running  YES

Slave_SQL_Running  YES

- 错误日志

Last_error

- 延迟时间

Seconds_Behind_Master

- 停止从库

`
stop slave;
`

- 设置延迟时间

`
CHANGE MASTER TO MASTER_DELAY = 60;
`
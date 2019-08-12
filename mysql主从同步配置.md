master 执行
GRANT REPLICATION SLAVE ON *.* TO 'slave173'@'%' IDENTIFIED BY '123456';

flush privileges;
FLUSH TABLES WITH READ LOCK;
 
SHOW MASTER STATUS;
UNLOCK TABLES;

slave执行
stop slave;

change master to master_host='192.168.31.147',master_user='slave173',master_password='123456',master_log_file='mysql-bin.000003',master_log_pos=1268;

start slave; 

show slave status;


Slave_IO_Running  YES

Slave_SQL_Running  YES

Last_error



stop slave;
CHANGE MASTER TO MASTER_DELAY = 60;
start slave;
show slave status;

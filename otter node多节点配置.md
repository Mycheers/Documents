## otter多node配置

1. wget https://github.com/alibaba/otter/releases/download/otter-4.2.17/node.deployer-4.2.17.tar.gz
2. mkdir /usr/local/otter/node02   mkdir /usr/local/otter/node03
3. 解压到指定目录 tar zxvf node.deployer-4.2.17.tar.gz -C /usr/local/otter/node02
4. tar zxvf node.deployer-4.2.17.tar.gz -C /usr/local/otter/node03
5. vim node02/conf/otter.properties  修改manager ip
	1. 支持多个ip	
6. vim node03/conf/otter.properties  修改manager ip
7. vim /root/.bash_profile
8. 添加aria2 安装路径到path    /usr/local/otter/aria2-1.19.0/src
9. 在manager页面机器管理 -> node -> 添加node02 node03 注意端口号不要冲突 记录node序号
10. 使用 echo 2 > nid  命令 将各自的node序号写入到各自conf文件夹下
11. 启动node  

## node支持多节点负载，

- 在Channel管理 > Pipeline管理 > 编辑Pipeline 中Select机器选择多个节点
- load机器也选择多个节点
- 如果某个节点挂断，Pipeline 会自动切换节点，实现了高可用

## 单向同步配置流程
### 源数据库操作
1. 先确认源库是否开启了binlog，且binlog为row模式
	1.  show variables like '%log_bin%'; # 查询binlog是否开启
	2.  show variables like 'binlog_format'; # 查询binlog模式
	3.  log-bin=mysql-bin
	4.  binlog-format=ROW
2. 锁住源数据库，
3. 执行 show master status; 记录binlog 当前写入位点
4. 执行 select UNIX_TIMESTAMP(NOW()) 记录当前时间戳
5. 导出要配置的同步的表或整个数据库
6. 将源数据库解锁
7. 账号授权 GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON . TO 账号@’%’;
### 目标数据库操作
7. 将备份数据数据导入到目标数据库
8. 账号授权 update, insert, delete, update, alter(DDL语句同步), index(用于创建删除索引)

## otter同步测试

### update
1. 目标表删除已同步数据otter，不会修复此数据
2. 目标表删除已同步数据otter，otter不会报错
3. 关闭otter 修改/新增/删除源表， 同样语句在目标库执行，重新开启otter正常
4. 删除目标表数据，原表修改已删除数据，otter没有报错，
	1. 列模式  不会有更新
	2. 行模式	会把丢失数据补回
5. 修改目标表字段1，原表修改同一条数据字段2
	1. 列模式 字段同步模式 只同步字段2到目标库，字段1不修改
	2. 列模式 组合同步模式 同步字段1，字段2均在组合中，字段1字段2都会同步到目标库
	3. 行模式 修改任意源库修改任何一个字段，都会把整行里需要同步的数据同步到目标库
### DDL
1. DDL 在末尾追加无默认值字段，可正常同步ddl
2. DDL 在末尾追加有默认字段，可正常同步ddl
3. DDL 删除已有字段，可正常同步ddl
### otter无缝取代主从
1. 步骤1:mysql 主从异常,断开主从开启otter同步  正常同步  
2. 步骤2:关闭otter ，删除从库差异数据，开启主从同步成功
3. 步骤3:mysql 主从异常,不操作主从开启otter同步  正常同步 



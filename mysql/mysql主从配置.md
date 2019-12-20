# mysql 配置主从同步
## 主库配置
- 配置my.cnf
        //配置服务ID,随意的一个数字，master的要比slave的小
        server-id=1
        #
        log-bin=master-bin
        log-bin-index=master-bin.index
        //配置要同步的数据库
        binlog-do-db=java_db
        binlog-do-db=java_db1
        //配置不需要同步的数据库(像数据库自己的数据库 mysql、sys、information_schema、performance_schema等数据库都不用同步)
        binlog-ignore-db=mysql
        binlog-ignore-db=sys

   
- 重启数据库并登陆
- 创建一个用于同步的用户，并授权
```java
 //创建用户
    create user binloguser@'192.168.153.%' identified by 'password';
    //授权
    grant replication slave on *.* to binloguser@'192.168.153.%' identified by 'password';
```
- 查看master 状态
```sql
--该命令会显示binlog对应的文件名，位置，以及需要同步的数据库和不同步的数据库，其中binlog文件名和读取位置在slave中要指定
show master status
```
##从库配置
- 配置my.cnf
```sql
--要比主库大
server-id=2
```
- 重启从库
- 配置主库信息
```sql
--通过以下命令配置
CHANGE MASTER TO MASTER_HOST='192.168.153.128',MASTER_USER='binloguser', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog文件名',MASTER_LOG_POS=读取位置
```
- 开启slave
`start slave`
- 查看slave状态
`show slave status \G`
##验证
	在主库中新建数据库java_db,并在该数据库总新建一张表，在表中插入数据，打开从库会发现数据库已经同步过来了
## tips
	如果从库要重新同步主库,可先stop slave,并重置 reset slave，再执行change master ...命令然后重启启动slave 即可




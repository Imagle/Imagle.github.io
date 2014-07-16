---
layout: post
title: "Mysql主从同步"
tagline: "Mysql master slave replication"
description: "Mysql master slave replication"
tags: [replication, mysql]
---
{% include JB/setup %}

在公司中一般对数据库都会有2份实时备份，实现就是用的mysql的复制（replication）。用于线上的数据库称为主库，也即是所说的master，其余的2份实时备份就是slave，如何配置请看下面介绍。 

##### 基本情况

 - 机器两台：web01,web02都安装的有mysql，
 - 目标：将web01设置为master，web02设置为slave，需要同步web01上面的名为syncDB的数据库。 

##### 开始设置 

#### 一、首先设置my.conf
1. __web01的my.conf__   
    server-id=1     #0表示拒绝一切从库，1表示主，大于1都都为从，每个机器此id必须唯一  
    log-bin=/data/data/mysql/binlog/binlog     #binlog的存放路径  
    binlog-do-db=rrdb #需要同步的数据库  
    binlog-ignore-db=mysql, information_schema #不需要同步的数据库  

2. __web02的my.conf__   
    server-id=2  
    log-bin=/data/data/mysql/binlog/binlog  
    replicate-do-db= rrdb  
    replicate-ignore-db=mysql,information_schema  

3. __重启web01和web02机器的mysql__   
    mysqladmin -uroot shutdown  mysqld_safe &  
    
#### 二、在主库中创建需要同步的账号并对其赋权限 
1. 在 __web01__ 机器上执行如下命令：  
    mysql> GRANT REPLICATION SLAVE ON *.* TO 'replication'@'%' identified by 'password';  
    mysql> flush privleges  
    $mysqldump -uroot syncDB >mdb.sql  
    mysql> show master status\G;   
    记下binlog文件和position  
    如果主库数据更新频繁的话，需要执行`mysql> FLUSH TABLES WITH READ LOCK;`  
    
2. 到 __web02__ 机器上将上面的sql导入到从库中  
    $ mysql -uroot syncDB < mdb.sql  
    在 __web02__ 上执行如下命令:  
    mysql> change master to master_host = "web01 ip address", master_user = 'replication', master_password = 'renrenshuo', master_log_file = 'binlog.0000xx', master_log_pos = position;  
    mysql> start slave;   
    在 __web01__ 上(可选和上述lock步骤对应) `mysql> unlock tables;`  
    
#### 三、验证 
在 __web01__ 上  

 - 验证replication@‘%’的权限`mysql>select User, Host from mysql.user;`， 如图  
  ![picture-example_01][example_01]   

 - 验证master的状态`mysql> show master status\G;`,如图  
  ![picture-example_01][example_02]  

在 __web02__ 上  

 - 查看slave机器的进程`mysql>show processlist;`，如图  
  ![picture-example_01][example_02]  

 - 查看slave机器状态，`mysql> show slave status\G;`，如图  
  ![picture-example_01][example_04] 
  
  只有当上图中的`Slave_IO_Running:Yes`和 `Slave_SQL_Running:Yes` 均为 __Yes__ 时，说明配置成功。 

-------------------------------------------------------  
我开通了微信公众号--__分享技术之美__，我会不定期的分享一些我学习的东西.  
你可以搜索公众号：__swalge__ 或者扫描下方二维码关注我  
![关注][photo]  

[example_01]:http://imagle.github.io/static/img/replication.png
[example_02]:http://imagle.github.io/static/img/master-status.png
[example_03]:http://imagle.github.io/static/img/slave-processlist.png
[example_04]:http://imagle.github.io/static/img/slave-status.png
[photo]:http://imagle.github.io/static/img/photo.jpg

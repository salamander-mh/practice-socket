server 可带命令 
./server 服务器运行时间 服务器运行端口

./client 服务器的端口 服务器的IP

chang log:

当前文件夹共享，不支持文件夹传输
日志功能，记录服务器所有运行情况
匿名登录，暂时和已注册用户权限一样
支持4G内的单个文件传输（未测试）
多用户单任务模式（单个用户只能做一件事）




服务器运行需要：

mysql运行
建立 用户名 admin  密码 admin  用户，可以到chklogin.c中修改用户

建立 数据库 mydatabase 

建立 表     userinfo

    id         int(4)  auto_increment primary key,
    username   varchar(24) unique not null,
    userpasswd varchar(24) not null,
    state      int(1)  not null;   //默认0  未登陆，  1 登录

	+------+----------+------------+--------+
	| id   | username | userpasswd |  state |
	+------+----------+------------+--------+
	| 1000 | ftpadmin | ftpadmin   |    0   |
	+------+----------+------------+--------+




需改进的功能：

	服务器端需要对在线客户端进行管理，比如强制是某个客户端离线
       （该功能较易实现，只需继续在Ctrl+c 的中断信号处理函数里添加） 
	
	文件夹的传输，现在主要思想是，判断出如果是文件夹，则调用系统
	命令，将其压缩，然后传输过去，客户端解压缩，把中间文件删除

        不再只是当前文件夹的共享，需要给每个不同权限的用户给予不同的
	路径，每个在线用户，都要记录它此时的工作目录，

	类P2P功能，让客户端和客户端能在服务器的控制下，进行安全的合理 
	的通信，比如服务器没有某个文件，而其它客户端有，较复杂

问题：

1.不同的ending的电脑，big-ending，little-ending  在直接发送除了字节流数据外可能出错，要使用 转换字节序 htos 。。。。


2.不同 位数的电脑 32位和64位在传输中的数据的差异  //已解决
详见文档 sync 网络编程发送数据注意

3.当服务器或客户端意外离线时，未做处理 ，socket突然断掉


4.关于非当前文件夹的传输，加个 路径即可，可是当多个客户端处于不同
	路径时，要分别给它们保存当时的工作路径

5.关于 加数据库 验证用户，分配不同权限（如 不同文件夹的访问范围） 

6.关于用户重复登陆的问题。(最好利用sql临时表解决，已登录的用户存到临时表中)

7. 传输文件夹出错，无法识别文件夹
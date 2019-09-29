# FTPClient
![1](https://github.com/dddder4/FTPClient/blob/master/examples/1.png)  
## 功能展示
### 查看
![2](https://github.com/dddder4/FTPClient/blob/master/examples/2.png)  
刷新：将远程目录树的节点刷新。移除所有节点后重新添加
### 连接
![3](https://github.com/dddder4/FTPClient/blob/master/examples/3.png)  
如果“主机”以ftp://开头，会自动去掉  
用户名不填默认anonymous，向服务器发送user命令  
密码向服务器发送pass命令  
端口默认21  
端口与密码要处理成String才能向socket发送命令  
如果长时间不进行操作socket会自行关闭  
![4](https://github.com/dddder4/FTPClient/blob/master/examples/4.png)  
主机、用户名、密码都支持如图操作  
![5](https://github.com/dddder4/FTPClient/blob/master/examples/5.png)  
断开连接向服务器发送quit命令，然后调用socket、reader、writer的close方法  
重新连接可以重新连接最近一次连接的服务器  
### 消息日志
![6](https://github.com/dddder4/FTPClient/blob/master/examples/6.png)  
支持如图操作  
### 本地目录树
![7](https://github.com/dddder4/FTPClient/blob/master/examples/7.png)  
先c到z盘搜索，有磁盘就添加节点。然后递归列举磁盘文件并添加节点。  
每更改一次选中的节点都会获取此节点在树的路径，处理成文件路径再放到“本地站点”中 ，再获取文件大小放到树下的label中，label的底色设置为白。  
上传：发送stor命令，成功上传后远程目录树会动态添加节点，如果上传同名文件会提醒失败，只能上传文件。  
![8](https://github.com/dddder4/FTPClient/blob/master/examples/8.png)  
添加到队列：table.addrow();  
打开：Desktop.getDesktop().open(fileselect);  
创建目录：mkdir()，再动态添加“新建文件夹”节点  
删除：file.delete();如果是文件夹先递归删除子文件文件夹，动态删除树节点  
重命名：addTreeModelListener，重写treeNodesChanged。oldfile.renameTo(newfile);如果当前目录下已存在同名文件，则重命名失败，且树节点名字也不会改变  
![9](https://github.com/dddder4/FTPClient/blob/master/examples/9.png)  
![10](https://github.com/dddder4/FTPClient/blob/master/examples/10.png)  
### 远程目录树
![11](https://github.com/dddder4/FTPClient/blob/master/examples/11.png)  
每选中一次树叶子节点就发送一次cwd命令，如果返回227代表是文件夹就发送nlst命令读取文件名并添加树节点，如果返回550代表是文件就发送size命令读取文件大小，有的ftp不支持siz  e命令
下载：retr命令，如果本地目录有同名文件也不会下载成功，并提示，成功下载后本地目录树动态添加节点，只能下载文件  
添加到队列：效果与本地目录树一样  
创建目录： mkd命令，效果与本地目录树一样  
删除：dele命令，只能删除文件  
重命名：rnfr、rnto命令，效果与本地目录树一样  
### 传输队列
![12](https://github.com/dddder4/FTPClient/blob/master/examples/12.png)  
显示各种信息，其中进度下载上传时每read一次就刷新一次  
移除选定：删除选中行tableModel.removeRow(table.getSelectedRow())，没有停止下载功能  

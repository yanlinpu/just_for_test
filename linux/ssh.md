
前提： 现有两台机器 client & server 端口都为12345 用户同为git

1. client免密码登录server

  ```
$ sudo vim /etc/hosts
# add server host -----> ip server   
$ ssh-copy-id -i ~/.ssh/id_rsa.pub "-p 12345 git@server"
  ```
  
2. client免端口链接server

  ```
$ vim ~/.ssh/config
############################
StrictHostKeyChecking no
Host server
        port 12345
        IdentityFile ~/.ssh/id_rsa
        user git 
        CheckHostIP no
#############################
$ ssh server
# Bad owner or permissions on /home/git/.ssh/config
$ chmod 600 ~/.ssh/config
  ```

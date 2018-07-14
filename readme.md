# ubuntu
关于 `Linux` 服务器的东西太多了，因此用这个来记一下。有时间会把之前涉及到的 `Linux` 操作全部整理到这里来。包括目前服务器上的一些设置，因为有一些应用没有开机自启动，一旦关机恐怕会有些麻烦。

更多关于桌面 `ubuntu` 的使用见 `大数据上机实习`
更多关于远程部署 `tomcat` + `mysql` + `jsp` 的内容见 `javahomework` 的大作业部分

**端口占用**
+ `tomcat`: 8080
  开机不自启动，`$TOMCAT_HOME/bin/startup.sh` 启动
+ `flask`: 5000
  开机不自启动，
+ `jupyter`: 8888
+ `nextcloud`:80


## 基本操作
+ 允许远程 `root` 登陆
```shell
vim /etc/ssh/sshd_config
加上 PermitRootLogin yes
/etc/init.d/ssh restart
```
+ `~/.bashrc` 作用于当前用户，`etc/profile` 作用于所有用户
+ `sudo` 命令在 `sh bin/startup.sh` 之前
+ 将服务启动文件放入 `/etc/init.d` 中可以变成自启动服务
+ `vim` 下 `\` 后加关键词查找
+ `ps aux|less` 查看所有进程

**权限问题**
+ 修改、查看文件时需要注意权限，比如 `vi` 命令，在用户没有读权限时是打不开文件的，类似的命令还有 `gedit`
+ 对于一些 `ubuntu`中只有读权限的文件夹，可以更改权限
  ```
  sudo chmod -R 755 /usr/local/hadoop/hadoop-2.7.5
  sudo chown -R hadoop:hadoop /usr/local/hadoop/hadoop-2.7.5
  ```
+ `-R` 表示 **递归**
+ 这样 `sudo` 的作用就体现了，在桌面端中，有时用户没有权限创建文件夹，就可以利用命令行的 `sudo` 命令创建文件夹
+ 有时程序需要写入，但是若用户没有权限，那么无法写入，从而运行不成功，这时要使得需要写入的文件夹拥有权限

**hostname hosts**
+ 这两个文件都在 `/etc` 目录下，前者给出了主机名称，后者给出了 IP地址
+ `localhost` 一般指 `127.0.0.1`

**进程**
+ 察看进程 `jps`
+ 关闭进程 `kill 进程号`

## 文件位置
+ java位置
/usr/lib/jvm/java-1.8.0-openjdk-amd64

+ tomcat位置
/usr/local/tomcat/apache-tomcat-8.0.52
**把新的jar包导进去的时候需要重启才能生效！**

+ python位置
/usr/local/python3

## 文件上传
+ 上传文件
```shell
pscp -r C:\Users\阳磊\Desktop\大作业\back-side\ root@58.87.xxx.xx:/usr/local/tomcat/apache-tomcat-8.0.52/webapps/course/
```

## jupyter notebook
```py
# 创建密码
python -c "import IPython;print IPython.lib.passwd()"

jupyter notebook --generate-config --allow-root
vim ~/.jupyter/jupyter_notebook_config.py

# 修改配置文件
c.NotebookApp.ip = '*'
c.NotebookApp.allow_root = True
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.password = u'刚才生成的密文(sha:...)'
c.ContentsManager.root_dir = '/data/jupyter/root'

# 后台启动
nohup jupyter notebook > /data/jupyter/jupyter.log 2>&1 &
```
详见 https://www.cnblogs.com/douzujun/p/8453030.html

## 部署flask

直接将 `python` 工程目录上传到 `linux` 上就可以部署。
```shell
python3 web.py
```

不过此时是前台运行，要转到后台运行，需要增加一行命令

```shell
nohup python3 web.py &
```

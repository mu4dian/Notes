# Anaconda配置命令

> 创建新环境：conda create --prefix D:\Envs\Python\python3.10.6Test python=3.10.6
>
> 查看已安装的环境：conda env list
>
> 激活环境：conda activate D:\Envs\Python\python3.10.6Test
>
> 退出：conda deactivate
>
> 删除环境：conda env remove -p D:\Envs\Python\python3.10.6Test
>
> 
>
> 以下命令需要先激活环境！！！
>
> 查找模块：conda search ···
>
> 下载模块：conda install ···=(版本)
>
> 查看已安装模块：conda list
>
> 更新模块：conda upgrade ···
>
> 删除模块：conda remove ···

# Github Tips

百科大全：awesome xxx

找例子：xxx sample

找空项目架子：xxx starter / xxx boilerplate

找教程：xxx tutorial



# Git常用命令

拉取仓库：git clone https://XXXXXXXXX

初始化：git init

暂存：git add -A

提交：git commit -m "名称"

配置信息：git config --global user.email "your_email"

​				  git config --global user.name "your_name"

插件：gitlens很好用

工作区回滚：git checkout [filename]

提交后回滚：先git reset HEAD^1到工作区

以当前分支为基础创建分支：git checkout -b <名称>

切回原来分支：git checkout <名称>

合并分支：git merge <名称>

查看分支：git branch

删除分支：git branch -D <名称>

向云端推送：本地提交之后 git push

从远程拉取：git pull

日志查询：git log

# MySQL命令

命令行启动/关闭数据库：net start/stop MySQL82

登录命令：mysql -uroot -proot -h<host> -P<port> <databasename>

查看版本号：select version();

退出命令：exit

创建数据库：create database if not exists 数据库名 character set 字符集 collate 排序方式;

查看数据库字符集：show variables like 'character_set_database';

查看数据库排序方式：show variables like 'collection_database';

查看库：show databases;

查看当前使用库：select database();

查看指定库下所有表：show tables from 数据库名；

切换库/选中库：use 数据库名；





# Linux常用命令

设置程序开机自启：systemctl enable <程序名>

# Docker常用命令

检索：docker search

下载：docker pull

列表：docker images

删除：docker rmi

# Pytorch指令

实时显示输出：--view-img


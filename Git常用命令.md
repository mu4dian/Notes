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
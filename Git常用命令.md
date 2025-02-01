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





**在 GitHub 上创建一个新的远程仓库：**

- 登录 [GitHub](https://github.com/) 账号。
- 点击页面右上角的 “+” 按钮，选择 “New repository”。
- 填写仓库名称和描述（可选），注意 **不要** 勾选 “Initialize this repository with a README”（否则本地与远程可能会有提交冲突）。
- 点击 “Create repository” 按钮创建仓库。

**关联本地仓库与 GitHub 仓库：**

在 GitHub 新创建的仓库页面，你会看到类似如下的远程仓库 URL：

```http
arduino
https://github.com/YourUsername/YourRepository.git
```

在本地仓库终端中执行：

```bash
git remote add origin https://github.com/YourUsername/YourRepository.git
```

这条命令将 GitHub 仓库设置为名为 `origin` 的远程仓库。

**推送本地仓库到 GitHub：**

根据你的默认分支名称（过去常用 `master`，现在很多项目默认使用 `main`），执行相应命令：

- 如果分支名称为 

  ```
  master
  ```

  ：

  ```bash
  git push -u origin master
  ```

- 如果分支名称为 

  ```
  main
  ```

  ：

  ```bash
  git push -u origin main
  ```

参数 `-u` 的作用是将本地分支与远程分支建立追踪关系，以后可以直接使用 `git push` 和 `git pull`。


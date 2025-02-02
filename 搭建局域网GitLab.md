下面介绍一套基于 CentOS 虚拟机的局域网 GitLab 服务器搭建方案，内容包括系统前提、详细的安装步骤、必要的配置以及常用使用方法。你可以参考以下步骤完成 GitLab 的安装与使用。

------

## 一、前提准备

1. **硬件要求**
   - **内存**：推荐至少 4GB（生产环境建议更高）。
   - **CPU**：至少 2 核。
   - **磁盘空间**：根据项目规模预留足够空间。
2. **系统要求**
   - 安装 CentOS（例如 CentOS 7 或 CentOS 8）。
   - 配置好网络环境，确保虚拟机能在局域网中被其他主机访问（可通过 IP 或 DNS 访问）。
3. **域名与主机名**
   - 对于局域网环境，可以使用局域网内的 IP 地址，也可以配置内网 DNS，将一个友好的域名指向该 IP。
   - 建议修改 `/etc/hosts` 文件，方便测试访问。

------

## 二、安装 GitLab CE 的详细步骤

### 1. 系统更新与依赖安装

首先，更新系统并安装 GitLab 运行所需的基础依赖包：

```bash
sudo yum update -y
sudo yum install -y curl policycoreutils openssh-server perl
```

如果需要 GitLab 发送邮件通知（例如密码找回等），建议安装并启动 Postfix：

```bash
sudo yum install -y postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

### 2. 添加 GitLab 官方仓库

GitLab 官方提供了一个便捷的脚本，可以直接将 GitLab 的仓库添加到系统中。执行以下命令：

```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
```

此命令会自动配置好 GitLab CE 的仓库源，便于后续安装。

### 3. 安装 GitLab CE 软件包

添加仓库完成后，使用 yum 安装 GitLab CE：

```bash
sudo yum install -y gitlab-ce
```

### 4. 配置 GitLab 外部访问地址

安装完成后，需要配置 GitLab 服务器的外部 URL。编辑配置文件 `/etc/gitlab/gitlab.rb`，找到或添加如下配置：

```ruby
external_url "http://gitlab.example.com"
```

其中 `gitlab.example.com` 请根据实际情况修改为局域网内可访问的 IP 地址或域名（例如 `http://192.168.1.100`）。

### 5. 执行配置重载

配置修改完成后，运行下面的命令使配置生效，并启动所有 GitLab 服务：

```bash
sudo gitlab-ctl reconfigure
```

该命令会自动生成配置文件、初始化数据库、启动服务等。整个过程可能需要几分钟。

------

## 三、网络与防火墙配置

在局域网环境中，为保证其他主机可以访问 GitLab，需要开放 HTTP（默认端口 80）及 HTTPS（如果使用 SSL，默认 443）端口。以 CentOS 内置防火墙 firewalld 为例：

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

如果使用 iptables 或其他防火墙工具，请根据实际情况配置相应规则。

------

## 四、GitLab 使用方法

### 1. 访问 Web 界面

在浏览器中访问之前配置的外部 URL（例如 [http://192.168.1.100），第一次访问时](http://192.168.1.xn--100),-kg1hi54lnwibm0bmw8amox/) GitLab 会要求你设置 root 用户的初始密码。

### 2. 修改初始密码

- 输入两次新密码后，完成密码设置。
- 使用用户名 `root` 和新密码登录。

### 3. 创建项目和用户

登录成功后，你可以：

- **创建新项目**：点击“New Project”，填写项目名称、描述，选择私有或公开项目。
- **添加用户**：通过管理后台添加其他用户，并为其分配项目访问权限。
- **配置 SSH Key**：建议每个用户生成 SSH 密钥，并在个人设置中添加，这样在 push 或 pull 时可以避免每次输入密码。

### 4. 使用 Git 命令操作仓库

GitLab 会在项目创建成功后提供详细的 Git 使用说明。常见操作步骤如下：

- 克隆仓库

  使用 HTTPS 或 SSH 地址克隆项目，例如：

  ```bash
  git clone http://gitlab.example.com/yourname/yourproject.git
  ```

- 添加远程仓库

  如果是已有项目，可添加 GitLab 为远程仓库：

  ```bash
  git remote add origin http://gitlab.example.com/yourname/yourproject.git
  ```

- 提交代码并推送

  ```bash
  git add .
  git commit -m "首次提交"
  git push -u origin master
  ```

------

## 五、常用管理命令及维护

- 查看服务状态

  ```bash
  sudo gitlab-ctl status
  ```

- 重启 GitLab 服务

  ```bash
  sudo gitlab-ctl restart
  ```

- 查看日志

  如果遇到问题，可以查看日志：

  ```bash
  sudo gitlab-ctl tail
  ```

- **备份数据**
   定期备份 GitLab 数据库和配置，参考官方文档备份策略。

------

## 六、额外注意事项

1. **SELinux 配置**
    如果遇到 SELinux 相关问题，建议设置为宽松模式或配置相应策略，但 GitLab Omnibus 安装包通常能自动兼容。
2. **资源优化**
    根据项目数量与使用情况，适时调整虚拟机资源，保证 GitLab 运行流畅。
3. **升级维护**
    定期关注 GitLab 的更新，使用 `yum update gitlab-ce` 升级版本，并做好备份。

------

## 七、总结

按照以上步骤，你可以在 CentOS 虚拟机上成功搭建局域网 GitLab 服务器。安装过程主要包括系统更新、依赖安装、添加 GitLab 仓库、安装软件包、配置外部访问、以及防火墙规则的设置。安装完成后，通过 Web 界面进行初始配置，再结合 Git 命令实现代码版本管理与协作。

希望这份详细的搭建及使用指南能够帮助你顺利部署并使用局域网 GitLab 服务器，如有其他问题，可参考 GitLab 官方文档或社区讨论。
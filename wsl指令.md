下面是一份较为全面、详细的 **WSL 使用笔记**，涵盖了从基础介绍、安装配置、常用命令及参数说明，到功能亮点和使用技巧，供你在日常使用时参考。

------

# WSL 使用笔记

## 目录

1. [什么是 WSL](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#什么是-wsl)
2. [WSL 的安装与配置](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#wsl-的安装与配置)
3. 常用 Linux 命令及参数说明
   - [文件与目录操作](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#文件与目录操作)
   - [文件内容查看与编辑](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#文件内容查看与编辑)
   - [包管理器（以 Ubuntu 为例）](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#包管理器以-ubuntu-为例)
   - [权限管理与压缩解压](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#权限管理与压缩解压)
   - [进程管理与系统监控](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#进程管理与系统监控)
   - [网络工具](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#网络工具)
   - [文本处理及其他工具](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#文本处理及其他工具)
4. [WSL 的特色功能介绍](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#wsl-的特色功能介绍)
5. [使用技巧与注意事项](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#使用技巧与注意事项)
6. [总结](https://chatgpt.com/c/679debed-df54-8001-b14b-c4607771a6ce#总结)

------

## 什么是 WSL

**WSL（Windows Subsystem for Linux）** 是微软推出的一项功能，它允许你在 Windows 10 及以上系统上直接运行 Linux 环境，包括大多数命令行工具、应用程序及 shell 脚本，而无需使用虚拟机或双系统。

- **WSL1**：实现了 Linux 系统调用的兼容层，启动速度快，但兼容性和性能上有一定限制。
- **WSL2**：基于轻量级虚拟机，内置真实的 Linux 内核，兼容性和性能均有大幅提升，同时支持更多功能（如 Docker 的原生支持）。

------

## WSL 的安装与配置

### 1. 启用 WSL 功能

在 **管理员模式** 下打开 PowerShell，运行以下命令（Windows 10 2004 及以上版本或 Windows 11）：

```powershell
wsl --install
```

该命令将自动启用必要的 Windows 功能，并安装最新的 WSL 内核和默认的 Linux 发行版（通常为 Ubuntu）。

> **注意：** 如果你需要安装指定的发行版或切换 WSL 版本，可参考后续步骤。

### 2. 安装指定发行版

- 打开 [Microsoft Store](https://www.microsoft.com/store) 搜索 “Linux”，选择你需要的发行版（如 Ubuntu、Debian、Kali 等）。
- 安装完成后，点击启动应用，进行首次设置（设置用户名和密码）。

### 3. 查看与管理已安装的发行版

- 查看已安装的发行版及版本：

  ```bash
  wsl -l -v
  ```

- 设置默认发行版：

  ```bash
  wsl --set-default <发行版名称>
  ```

- 设置默认 WSL 版本为 2（建议使用 WSL2）：

  ```bash
  wsl --set-default-version 2
  ```

- 若需要将已安装的发行版转换为 WSL2：

  ```bash
  wsl --set-version <发行版名称> 2
  ```

  例如：

  ```
  wsl --set-version Ubuntu 2
  ```

### 4. 其他配置

- 配置资源限制（WSL2）：

  在用户目录下新建或编辑 

  ```
  .wslconfig
  ```

   文件：

  ```ini
  [wsl2]
  memory=4GB        # 分配给 WSL 的最大内存
  processors=2      # 分配给 WSL 的处理器核数
  ```

- 卸载已安装发行版系统

  ```bash
  wsl --unregister <发行版名称>
  ```
  
- 导出系统：

  ```bash
  wsl --export <发行版名称> <导出文件名>
  ```
  
- 导入系统：

  ```bash
  wsl --import <名称> <安装路径> <导入的系统包路径>
  ```
  
- 文件共享：

  ```bash
  df -h #可以看到子系统挂载的所有文件
  ```
  
- 重启 WSL：

  当遇到问题时，可通过 PowerShell 重启 WSL 环境：

  ```powershell
  wsl --shutdown
  ```

------

## 常用 Linux 命令及参数说明

在 WSL 中，你可以使用大部分常用的 Linux 命令，这里整理了一部分高频命令，并附上参数说明和示例。

### 文件与目录操作

- **pwd**
   显示当前工作目录。

  ```bash
  pwd
  ```

- **ls**
   列出目录下的文件和子目录。

  - `-l`：长格式显示（权限、所有者、大小、修改时间等）。
  - `-a`：显示所有文件（包括隐藏文件，文件名前有 “.”）。
  - `-h`：以易读格式显示文件大小（如 K、M、G）。

  ```bash
  ls -alh
  ```

- **cd**
   切换目录。

  - 示例：`cd /usr/local` 切换到指定目录；
  - `cd ..` 返回上一级目录；
  - `cd ~` 进入当前用户的主目录。

  ```bash
  cd /path/to/directory
  ```

- **mkdir**
   创建新目录。

  - `-p`：递归创建目录，即使父目录不存在也会一并创建。

  ```bash
  mkdir -p ~/projects/myapp
  ```

- **rm**
   删除文件或目录。

  - `-r`：递归删除（目录及其内容）。
  - `-f`：强制删除，无提示。

  ```bash
  rm -rf ~/old_project
  ```

- **cp**
   复制文件或目录。

  - `-r`：递归复制目录。
  - `-v`：显示复制过程（verbose）。

  ```bash
  cp -rv source_folder/ destination_folder/
  ```

- **mv**
   移动或重命名文件/目录。

  ```bash
  mv oldname.txt newname.txt
  ```

### 文件内容查看与编辑

- **cat**
   连接并显示文件内容。

  ```bash
  cat file.txt
  ```

- **more / less**
   分页显示长文件内容，`less` 支持上下滚动，更灵活。

  ```bash
  less file.txt
  ```

- **head**
   查看文件开头部分。

  - `-n` 指定显示行数，默认前 10 行。

  ```bash
  head -n 10 file.txt
  ```

- **tail**
   查看文件末尾部分。

  - `-n` 指定显示行数。
  - `-f` 实时追踪文件变化（常用于查看日志）。

  ```bash
  tail -n 20 file.txt
  tail -f /var/log/syslog
  ```

- **nano / vim / vi**
   常用的终端文本编辑器：

  - `nano`：简单易用，适合初学者。
  - `vim/vi`：功能强大，学习曲线较陡，但十分高效。

  ```bash
  nano filename
  vim filename
  ```

### 包管理器（以 Ubuntu 为例）

- **更新软件包列表**

  ```bash
  sudo apt update
  ```

- **升级已安装软件包**

  ```bash
  sudo apt upgrade
  ```

- **安装软件包**

  ```bash
  sudo apt install package_name
  ```

- **卸载软件包**

  ```bash
  sudo apt remove package_name
  ```

- **搜索软件包**

  ```bash
  apt search keyword
  ```

### 权限管理与压缩解压

- **chmod**
   修改文件或目录的权限。

  - 示例：`chmod 755 file` 表示所有者可读、写、执行，组用户和其他用户可读和执行。

  ```bash
  chmod 755 script.sh
  ```

- **chown**
   改变文件或目录的所有者和所属组。

  ```bash
  sudo chown user:group filename
  ```

- **tar**
   用于打包和解压文件/目录。

  - `-c`：创建归档。
  - `-x`：解压归档。
  - `-z`：通过 gzip 进行压缩/解压。
  - `-v`：显示详细过程。
  - `-f`：指定归档文件名称。

  ```bash
  # 打包并压缩
  tar -czvf archive.tar.gz folder/
  # 解压
  tar -xzvf archive.tar.gz
  ```

- **zip / unzip**
   用于处理 zip 格式压缩包。

  ```bash
  zip -r archive.zip folder/
  unzip archive.zip
  ```

### 进程管理与系统监控

- **ps**
   查看当前运行的进程。

  - 示例：`ps aux` 显示所有进程详细信息。

  ```bash
  ps aux
  ```

- **top / htop**
   实时监控系统资源和进程。

  - `htop` 需要另外安装（`sudo apt install htop`），界面更友好。

  ```bash
  top
  htop
  ```

- **kill**
   终止指定进程。

  - `kill -9 PID` 强制杀死进程。

  ```bash
  kill -9 1234
  ```

- **df**
   查看磁盘使用情况。

  - `-h` 以人类可读格式显示。

  ```bash
  df -h
  ```

- **du**
   显示目录或文件的磁盘占用。

  - `-sh` 显示总计且以人类可读格式展示。

  ```bash
  du -sh folder/
  ```

### 网络工具

- **ping**
   测试网络连通性。

  ```bash
  ping www.google.com
  ```

- **curl**
   用于传输数据，常用于 HTTP 请求测试。

  - `-I` 获取 HTTP 响应头。

  ```bash
  curl -I http://example.com
  ```

- **wget**
   用于下载文件。

  ```bash
  wget http://example.com/file.zip
  ```

- **netstat**
   查看网络连接、端口监听等信息（部分发行版可能需安装）。

  ```bash
  netstat -tuln
  ```

- **ssh**
   远程登录工具。

  ```bash
  ssh user@remote_host
  ```

### 文本处理及其他工具

- **grep**
   文本搜索工具，可结合正则表达式使用。

  ```bash
  grep 'pattern' filename
  ```

- **find**
   搜索文件或目录。

  - 示例：`find . -type f -name "*.log"` 在当前目录及子目录中查找扩展名为 `.log` 的文件。

  ```bash
  find /path -name "filename"
  ```

- **sed**
   流编辑器，用于文本替换与处理。

  ```bash
  sed -i 's/old/new/g' filename
  ```

- **awk**
   强大的文本和数据处理工具，经常用于格式化输出。

  ```bash
  awk '{print $1}' filename
  ```

- **man**
   查看命令帮助文档。

  ```bash
  man ls
  ```

- **uname**
   显示系统及内核信息。

  ```bash
  uname -a
  ```

------

## WSL 的特色功能介绍

1. **运行原生 Linux 环境**
    在 Windows 内核下无需虚拟机即可运行大部分 Linux 命令行工具、脚本和应用，适用于开发、调试和运行 Linux 服务。
2. **与 Windows 文件系统互通**
   - WSL 可以直接访问 Windows 驱动器（例如：`/mnt/c/` 对应 C 盘）。
   - 建议在 WSL 内部操作 Linux 文件系统，避免直接在 Windows 文件管理器中修改 WSL 内部文件，防止潜在的文件权限和性能问题。
3. **多发行版支持**
    同一台机器上可以安装多个 Linux 发行版，方便测试和开发不同环境的应用。
4. **与开发工具集成**
   - **VSCode Remote - WSL：**
      使用 VSCode 的 Remote - WSL 扩展，可以直接在 WSL 环境中进行代码编辑、调试和终端操作。
   - **Docker 集成：**
      WSL2 支持 Docker Desktop，在 Linux 环境中无缝运行容器化应用。
5. **GUI 应用支持（WSLg）**
    最新版本的 WSL2（结合 WSLg）支持 Linux 图形界面应用，无需额外配置即可运行 GUI 程序。

------

## 使用技巧与注意事项

- **文件系统操作**

  - 在 WSL 中尽量在 Linux 文件系统（如 `/home/username`）中进行开发，只有在必要时访问 `/mnt/c` 等 Windows 分区，避免因文件系统差异导致性能问题。
  - 使用软链接（`ln -s`）可以简化目录跳转和资源共享。

- **环境变量与别名配置**
   编辑 `~/.bashrc` 或 `~/.zshrc` 文件，配置常用别名和环境变量，提高工作效率：

  ```bash
  # 常用别名
  alias ll='ls -alF'
  alias la='ls -A'
  alias l='ls -CF'
  
  # 添加自定义 PATH
  export PATH=$PATH:~/bin
  ```

  修改后运行 `source ~/.bashrc` 使配置生效。

- **性能调优**

  - 在 `.wslconfig` 中调整内存、CPU 分配，提升 WSL2 性能。
  - 遇到 WSL 卡顿或错误时，尝试使用 `wsl --shutdown` 重启 WSL 环境。

- **网络设置**

  - WSL 与 Windows 共用网络，注意检查防火墙及端口冲突问题。
  - 若需要外部访问 WSL 内运行的服务，可通过端口转发或在 Windows 上配置相关规则。

- **版本更新**
   定期检查 WSL 更新，确保内核、发行版及工具处于最新状态：

  ```bash
  sudo apt update && sudo apt upgrade
  ```

  访问 [WSL 官方文档](https://learn.microsoft.com/windows/wsl/) 获取最新资讯。

- **调试与日志**

  - 使用 `dmesg` 查看内核日志，帮助排查问题。
  - 利用 `tail -f` 监控日志文件实时变化，方便调试。

- **跨平台开发**
   在 WSL 中安装各种开发语言和工具（如 Python、Node.js、Ruby 等），充分利用 Linux 下的丰富生态，同时通过 VSCode 等工具与 Windows 环境无缝集成。

------


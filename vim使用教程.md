以下是一个完整详细的 Vim 使用教程，分为基本操作、高级功能、配置和插件使用四个部分。

### 1. **Vim 基本操作**

#### 启动 Vim

打开终端，输入命令 `vim` 启动 Vim 编辑器。

- **打开文件**: `vim 文件名`

  ```bash
  vim test.txt
  ```

- **退出 Vim**:

  - 按 `:q` 退出
  - 如果修改了文件并想保存，按 `:wq` 或 `:x`
  - 如果不保存修改并退出，按 `:q!`

#### 模式介绍

Vim 有三种主要模式：

- **普通模式**：启动 Vim 后的默认模式，用于导航和编辑。
- **插入模式**：用于文本输入，按 `i` 进入插入模式，按 `Esc` 返回普通模式。
- **命令模式**：用于执行命令，比如保存、退出，按 `:` 进入。

#### 常用命令

- **移动光标**
  - `h`：左
  - `j`：下
  - `k`：上
  - `l`：右
  - `w`：按单词前进
  - `b`：按单词后退
  - `0`：跳到行首
  - `$`：跳到行尾
  - `gg`：跳到文件开始
  - `G`：跳到文件结束
- **删除文本**
  - `x`：删除光标所在的字符
  - `dd`：删除整行
  - `dw`：删除光标所在的单词
  - `d$`：删除光标位置到行尾的内容
- **复制和粘贴**
  - `yy`：复制当前行
  - `yw`：复制光标所在的单词
  - `p`：粘贴
  - `P`：将剪贴板内容粘贴到光标前
- **撤销和重做**
  - `u`：撤销
  - `Ctrl + r`：重做

### 2. **Vim 高级功能**

#### 查找和替换

- **查找**：按 `/` 后输入要查找的内容，然后按回车，使用 `n` 和 `N` 查找下一个和上一个。

- 替换

  ：使用 

  ```
  :s
  ```

   命令进行替换。

  ```vim
  :%s/old/new/g  # 替换整个文件中的所有 old 为 new
  :s/old/new/g   # 替换当前行的所有 old 为 new
  ```

#### 多光标编辑

- 选择多个单词

  ：

  1. 按 `Ctrl + v` 进入块选择模式。
  2. 用光标移动选择区域。
  3. 按 `I` 进入插入模式并输入内容，输入后按 `Esc`。

#### 分屏

- **水平分屏**：`:split` 或 `:sp`
- **垂直分屏**：`:vsplit` 或 `:vs`
- **切换分屏**：`Ctrl + w` 然后按 `h`、`j`、`k`、`l` 切换屏幕
- **关闭分屏**：`:q`

#### 标签页

- **打开新标签页**：`:tabnew`
- **切换标签页**：`:tabnext` 或 `gt`
- **关闭标签页**：`:tabclose`

### 3. **Vim 配置文件 `.vimrc`**

Vim 配置文件 `.vimrc` 位于用户的 home 目录（`~/.vimrc`），它是 Vim 启动时加载的配置文件，可以根据个人需求进行定制。

#### 常用配置

- **设置行号**：

  ```vim
  set number
  ```

- **高亮当前行**：

  ```vim
  set cursorline
  ```

- **启用行尾空格显示**：

  ```vim
  set list
  ```

- **自动缩进**：

  ```vim
  set smartindent
  set tabstop=4
  set shiftwidth=4
  set expandtab
  ```

- **显示匹配括号**：

  ```vim
  set showmatch
  ```

#### 自动加载插件

```vim
" 自动加载插件
call plug#begin('~/.vim/plugged')
Plug 'tpope/vim-sensible'  " 常用插件
call plug#end()
```

### 4. **Vim 插件**

#### 插件管理器

推荐使用 [vim-plug](https://github.com/junegunn/vim-plug) 插件管理器来管理插件。

1. **安装 vim-plug**: 在终端执行：

   ```bash
   curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   ```

2. **安装插件**: 在 `.vimrc` 文件中添加插件：

   ```vim
   call plug#begin('~/.vim/plugged')
   Plug 'tpope/vim-fugitive'    " Git 插件
   Plug 'junegunn/fzf.vim'      " 文件模糊搜索插件
   call plug#end()
   ```

3. **安装插件**: 在 Vim 中运行 `:PlugInstall` 来安装插件。

#### 常用插件推荐

- **vim-airline**：美化状态栏
- **nerdtree**：文件树浏览器
- **vim-fugitive**：Git 插件
- **vim-surround**：括号和引号操作
- **fzf.vim**：快速文件搜索

### 5. **Vim 小技巧**

- **撤销删除并恢复文本**： 按 `p` 将删除的内容粘贴出来。
- **快速打开文件**： 在命令模式下按 `:e 文件名`。
- **按字母顺序打开文件**： 使用 `:Explore` 打开文件浏览器，快速找到文件。

以上就是 Vim 使用的一个详细教程，涵盖了从基础到高级的操作和配置。通过不断练习和扩展自己的配置文件，你将能够高效地使用 Vim。
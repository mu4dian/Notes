下面给出一份**详细的 Makefile 教程**，内容覆盖从基础到高级的所有语法和用法。你可以将下述内容作为参考，帮助你深入理解和编写 Makefile。

------

# 1. 简介

**Make** 是一个自动化构建工具，它根据 **Makefile** 中定义的规则自动决定哪些文件需要更新，并调用相应的命令来生成目标文件。通常用在 C/C++ 等编译型语言的工程构建上，但也可用于其它自动化任务。

Makefile 的核心思想是通过**依赖关系**和**规则**描述项目中各个文件之间的关系，从而实现增量编译，避免重复劳动。

------

# 2. Makefile 基本结构

一个最简单的 Makefile 由一个或多个**规则（rule）**组成。每个规则通常具有如下格式：

```makefile
target [target ...] : [prerequisites ...]
	[tab] command
	[tab] command
	...
```

- **target（目标）**：可以是要生成的文件，也可以是伪目标（phony target）。
- **prerequisites（依赖）**：构建目标所依赖的文件或其它目标。
- **command（命令）**：生成目标所要执行的 shell 命令。注意：每个命令行的开头必须是一个**tab字符**（而非空格）。

例如：

```makefile
# 编译 hello.o 依赖于 hello.c
hello.o: hello.c
	$(CC) -c hello.c -o hello.o
```

在命令中使用的 `$(CC)` 是变量的引用，后面会详细讲解。

------

# 3. 变量（Variables）

Makefile 中的变量用于存储文本或命令，可以在多个地方复用。定义变量的基本语法有多种方式：

### 3.1 简单变量赋值

```makefile
CC = gcc
CFLAGS = -Wall -O2
```

- 使用 `=` 号定义的变量为**递归展开变量**，在引用时会重新展开表达式（延迟求值）。

### 3.2 立即展开变量

```makefile
SRC := main.c util.c
```

- 使用 `:=` 定义的是**简单变量**，在赋值时就立即展开，后续引用时直接替换为其值。

### 3.3 条件赋值

```makefile
CC ?= gcc
```

- 当变量未定义时才赋值。

### 3.4 追加赋值

```makefile
LIBS += -lm
```

- 将内容追加到变量的现有值后面。

### 3.5 变量引用

- 引用变量的方法有 `$(VAR)` 或者 `${VAR}` 的形式。

例如：

```makefile
all:
	$(CC) $(CFLAGS) -o myprog main.c $(LIBS)
```

------

# 4. 自动变量（Automatic Variables）

在规则的命令中，可以使用一些自动变量，这些变量由 make 自动设置：

- **$@**：规则中的目标文件名。
- **$<**：第一个依赖文件（通常用于隐式规则）。
- **$^**：所有依赖文件，去除重复。
- **$?**：比目标旧的所有依赖文件。
- **$* **：匹配模式规则中的通配符部分（不包含扩展名）。

示例：

```makefile
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@
```

这表示将所有 `.c` 文件转换为 `.o` 文件，`$<` 表示当前规则中匹配到的 `.c` 文件，`$@` 表示目标 `.o` 文件。

------

# 5. 规则（Rules）

规则用于描述如何从依赖生成目标。规则可以分为以下几种：

### 5.1 显式规则

例如：

```makefile
hello: hello.o world.o
	$(CC) $(CFLAGS) -o hello hello.o world.o
```

### 5.2 隐式规则（内建规则）

Make 内置了一些默认的隐式规则，例如从 `.c` 到 `.o` 的编译规则。你可以直接使用，不必在 Makefile 中重新定义。

如果希望查看内建规则，可以执行：

```bash
make -p
```

### 5.3 模式规则（Pattern Rules）

模式规则用于定义一类目标的构建方式。格式中使用 `%` 作为通配符：

```makefile
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@
```

这条规则适用于所有 `.c` 文件编译生成对应的 `.o` 文件。

### 5.4 后缀规则（Suffix Rules）

这是一种较旧的语法，例如：

```makefile
.SUFFIXES: .c .o
.c.o:
	$(CC) $(CFLAGS) -c $< -o $@
```

虽然仍被支持，但现代 Makefile 推荐使用模式规则。

------

# 6. 特殊目标（Special Targets）

Makefile 中有一些内置的特殊目标，它们具有特定功能：

### 6.1 .PHONY

用于声明伪目标，这些目标并非实际文件，用于组织任务或执行非构建操作（如 clean、install 等）。

```makefile
.PHONY: clean all

all: myprog

clean:
	rm -f *.o myprog
```

### 6.2 .SUFFIXES

用于定义后缀规则中所涉及的文件扩展名。可以清空默认后缀列表或添加新的后缀。

```makefile
.SUFFIXES: .c .o
```

### 6.3 .DEFAULT

当 make 找不到匹配规则时，执行 `.DEFAULT` 中定义的命令。

```makefile
.DEFAULT:
	@echo "找不到规则，请检查文件名或依赖。"
```

### 6.4 .PRECIOUS

声明不希望中断时删除的目标文件。

```makefile
.PRECIOUS: %.o
```

### 6.5 .INTERMEDIATE

声明中间文件，构建结束后可以自动删除。

```makefile
.INTERMEDIATE: temp.txt
```

### 6.6 .IGNORE

忽略某个目标或某些命令中的错误。

```makefile
target:
	-@some-command   # 行首的 - 表示忽略错误
```

或者使用 `.IGNORE` 声明：

```makefile
.IGNORE: target
```

### 6.7 .SILENT 或 @

- 在命令前加 `@` 可使该命令在执行时不打印命令行内容。
- 使用 `.SILENT` 目标可以使一组命令静默执行（不打印命令）。

------

# 7. 条件语句（Conditional Directives）

Makefile 支持条件语句，用于在不同环境下做不同处理。常用的条件语句包括 `ifeq`、`ifneq`、`ifdef` 和 `ifndef`。

### 7.1 ifeq / ifneq

```makefile
ifeq ($(CC),gcc)
	CFLAGS += -std=c11
else
	CFLAGS += -std=c99
endif
```

注意：`ifeq` 与 `ifneq` 后面的条件必须用括号或引号包裹起来。

### 7.2 ifdef / ifndef

```makefile
ifdef DEBUG
	CFLAGS += -g
endif
```

这些条件语句只在 Makefile 被解析时执行，不会被写入最终的命令中。

------

# 8. 函数（Functions）

Makefile 内置了许多函数，可用于字符串处理、文件操作等。以下是一些常用函数：

### 8.1 字符串替换函数

- **subst**：简单字符串替换

  ```makefile
  NEW := $(subst old,new,$(VAR))
  ```

- **patsubst**：模式替换

  ```makefile
  OBJS := $(patsubst %.c,%.o,$(SRCS))
  ```

### 8.2 文件名函数

- **wildcard**：返回匹配通配符模式的文件列表

  ```makefile
  SRCS := $(wildcard *.c)
  ```

- **notdir**：去除文件路径，只保留文件名

  ```makefile
  FILES := $(notdir $(SRCS))
  ```

- **dir**：提取文件的目录部分

  ```makefile
  DIRS := $(dir $(SRCS))
  ```

### 8.3 其他函数

- **shell**：调用 shell 命令并返回输出

  ```makefile
  DATE := $(shell date +%Y%m%d)
  ```

- **foreach**：遍历列表

  ```makefile
  list := a b c
  result := $(foreach item,$(list),[$(item)])
  # result => [a] [b] [c]
  ```

- **if**：三目运算

  ```makefile
  result := $(if $(DEBUG),debug,release)
  ```

- **origin**：查看变量的来源

  ```makefile
  $(origin CC)
  ```

更多函数及其详细说明可参阅 GNU Make 文档。

------

# 9. 包含其它 Makefile 文件

有时为了管理复杂项目，我们会将 Makefile 拆分为多个文件。可以使用 `include` 指令包含其它文件：

```makefile
include config.mk
```

如果文件不存在，可以使用 `-include` 或 `sinclude` 来忽略错误：

```makefile
-include optional.mk
```

------

# 10. 命令行参数与变量覆盖

- 可以在命令行中指定变量来覆盖 Makefile 中的定义。例如：

  ```bash
  make CC=clang
  ```

- 指定目标，例如：

  ```bash
  make clean
  ```

------

# 11. 多目标与聚合目标

一个规则可以定义多个目标。如果多个目标使用相同的命令，推荐使用“静态模式规则”。

例如：

```makefile
# 静态模式规则
objects = a.o b.o c.o

$(objects): %.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@
```

------

# 12. order-only 依赖

有时依赖关系仅影响构建顺序，而不影响目标的更新时间。可以使用管道符 `|` 来声明 order-only 依赖：

```makefile
prog: main.o util.o | directory
	$(CC) -o prog main.o util.o

directory:
	mkdir -p directory
```

在这个例子中，`directory` 是 order-only 依赖，即使 `directory` 修改，`prog` 也不会因此重新构建（除非其它依赖变化）。

------

# 13. 多行命令和行续

- 如果一行命令过长，可以使用反斜杠 `\` 进行续行：

  ```makefile
  target:
  	@echo "This is a very long command that needs to be split over \
  	multiple lines."
  ```

- 注意：反斜杠续行仅对命令行有效，对变量赋值时需要使用不同的策略（例如 `define` ... `endef`）。

### 13.1 定义多行变量

可以使用 `define` 定义多行变量：

```makefile
define HELP_MSG
Usage: make [target]
  all      - Build all targets.
  clean    - Remove generated files.
endef

help:
	@echo "$(HELP_MSG)"
```

------

# 14. 调试 Makefile

在编写 Makefile 时，调试非常重要。以下几个选项有助于调试：

- **-n 或 --just-print**：只打印命令，不实际执行。
- **-d**：开启调试模式，打印详细的内部信息。
- **--warn-undefined-variables**：当使用未定义变量时发出警告。

例如：

```bash
make -n all
```

------

# 15. 完整示例

下面给出一个较为完整的示例 Makefile，展示如何结合前面讲到的语法与特性。

```makefile
# ============================
# 示例 Makefile
# ============================

# 变量定义
CC      := gcc
CFLAGS  := -Wall -O2
LDFLAGS :=
SRC     := $(wildcard *.c)
OBJ     := $(patsubst %.c,%.o,$(SRC))
TARGET  := myapp

# 条件判断，检查是否定义 DEBUG
ifdef DEBUG
	CFLAGS += -g -DDEBUG
endif

# 默认目标
.PHONY: all clean

all: $(TARGET)

# 链接目标
$(TARGET): $(OBJ)
	@echo "链接目标：$@"
	$(CC) $(LDFLAGS) -o $@ $^

# 编译规则（模式规则）
%.o: %.c
	@echo "编译 $<"
	$(CC) $(CFLAGS) -c $< -o $@

# 清理生成文件
clean:
	@echo "清理文件..."
	rm -f $(OBJ) $(TARGET)

# 包含其它文件（如果存在）
-include local.mk
```

这个示例做了以下事情：

- 定义了编译器、编译选项、源文件、目标文件以及最终生成的程序名；
- 使用了自动变量 `$@`（目标）和 `$^`（所有依赖）；
- 使用了模式规则进行编译；
- 声明了伪目标 `all` 和 `clean`；
- 使用了条件语句为调试模式添加选项；
- 包含了外部的 `local.mk` 文件，方便本地配置。

------

# 16. 小结

本文详细介绍了 Makefile 的基本结构、变量、规则、自动变量、特殊目标、条件语句、内置函数、包含其它文件以及一些调试和高级用法。掌握这些内容可以帮助你高效地管理项目构建流程，并根据需要自定义构建规则。

更多内容和细节可参阅 [GNU Make 官方文档](https://www.gnu.org/software/make/manual/make.html)（有中文版），以及各类开源项目中的 Makefile 示例。

希望这份详细教程对你理解和编写 Makefile 有所帮助！
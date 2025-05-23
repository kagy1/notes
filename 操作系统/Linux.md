# linux

## 安装应用

### java

1. 挑选jdk版本

   ```shell
   yum list | grep jdk
   ```

2. 安装

   ```shell
   yum install java-1.8.0-openjdk-devel.x86_64
   ```

3. 验证jdk

   安装之后输入 `javac`、`java -version` 验证是否安装成功，如下图所示，就是安装成功了


#### java环境变量

**alternatives系统**

```bash
sudo update-alternatives --install /usr/bin/java java /root/kagy/jdk1.8.0_171/bin/java 180
sudo update-alternatives --install /usr/bin/javac javac /root/kagy/jdk1.8.0_171/bin/javac 180
sudo update-alternatives --install /usr/bin/jar jar /root/kagy/jdk1.8.0_171/bin/jar 180
```

注册您手动安装的JDK 1.8到alternatives系统

设置优先级为180（您可以根据需要调整这个数字，通常比系统默认JDK高一些）

切换命令

```bash
sudo update-alternatives --config java
```


`/etc/environment`
`/etc/profile`

```bash
JAVA_HOME="/root/kagy/jdk1.8.0_171"
PATH="$PATH:$JAVA_HOME/bin"
```



### 启动jar包

后台运行

```bash
java -jar ReggieTestL-0.0.1-SNAPSHOT.jar &
```

终止程序

```bash
ps aux | grep ReggieTestL
kill <PID>
```





### mysql

#### 检查是否安装

```bash
rpm -qa | grep mysql

## 卸载mariadb，mariadb是mysql数据库的分支，mariadb和mysql一起安装会有冲突，所以需要卸载掉
rpm -qa | grep mariadb
rpm -e --nodeps 文件名
```

#### 常用命令

```bash
#开机自启
systemctl enable mysqld
#启动
sudo systemctl start mysqld
#查看状态
systemctl status mysqld
#重启
systemctl restart mysqld
#关闭
systemctl stop mysqld
#关闭开机自启
systemctl disable mysqld
```





#### 降低密码复杂度

```bash
SET GLOBAL validate_password.policy = LOW;
SET GLOBAL validate_password.length = 6;
```

#### 为root创建远程访问权限

```mysql
CREATE USER 'root'@'%' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```





### nginx

#### 源码编译安装 Nginx

解压

```bash
tar -zxvf nginx-1.26.3.tar.gz
cd nginx-1.26.3
```

安装依赖项

```bash
sudo yum install -y gcc gcc-c++ make pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

配置编译参数

```bash
./configure \
--prefix=/usr/local/nginx \
--with-http_ssl_module \
--with-http_gzip_static_module \
--with-http_stub_status_module
```

编译与安装

```bash
make
sudo make install
```

切换到安装目录并启动

```bash
cd /usr/local/nginx/sbin
sudo ./nginx
```

查看是否启动成功

```bash
ps aux | grep nginx
```





### redis

```bash
sudo wget http://download.redis.io/releases/redis-6.2.6.tar.gz
sudo tar -xzf redis-6.2.6.tar.gz
```

```bash
cd redis-6.2.6
sudo make
sudo make install
```



```bash
sudo mkdir /etc/redis
sudo cp redis.conf /etc/redis/
```

编辑 `/etc/redis/redis.conf`：

```bash
sudo vi /etc/redis/redis.conf
```

修改以下内容

```bash
bind 0.0.0.0           # 允许远程连接（仅在需要时）
protected-mode no      # 如果 bind 设置为 0.0.0.0，需关闭保护模式
daemonize yes          # 让 Redis 以后台进程方式运行
pidfile /var/run/redis.pid
logfile /var/log/redis.log
dir /var/lib/redis
```

创建运行目录和日志目录

```bash
sudo mkdir -p /var/lib/redis
sudo touch /var/log/redis.log
sudo chown -R root:root /var/lib/redis /var/log/redis.log
```



创建 Systemd 服务文件

```bash
sudo vi /etc/systemd/system/redis.service
```

添加以下内容

```bash
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always
User=root
Group=root

[Install]
WantedBy=multi-user.target
```











## 目录结构

Linux的目录是一个树形结构

Linux没有盘符这个概念，只有一个根目录`/`，所有文件都在它下面

### 路径描述方式

在Linux系统中，路径之间的层级关系用`/`表示

在window系统中，路径之间的层级关系用`\`表示



## root用户 

超级管理员

```bash
su - root
```

普通用户的权限，一般在其HOME目录内不受限。

一旦出了HOME目录，大多数地方，普通用户仅有只读和执行权限，无修改权限。



### su exit

Switch User

```bash
su [-] 用户名
```

- `-`是可选的，表示是否切换用户后加载环境变量
- 用户名可省略，表示切换到root
- 切换用户后，可以通过exit命令回退到上一个用户，也可以`ctrl + d`
- 使用普通用户，切换到其他用户需要密码
- 使用root用户，切换到其他用户无需密码



### sudo

在其他命令前带上sudo，可为这一条命令临时赋予root权限

并不是所有用户都有权利使用sudo，需要为普通用户配置sudo认证



## centos防火墙 firewalld

在 **CentOS 8** 中，默认使用的是 **`firewalld`**（动态防火墙管理器）

### 查看防火墙状态

```bash
sudo firewall-cmd --state
```

### 查看当前开放的端口

```bash
sudo firewall-cmd --list-ports
```

### 查看当前所有已开放的服务

```bash
sudo firewall-cmd --list-all
```

### 开放端口（如 MySQL 3306）

```bash
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload
```

### 关闭端口

```bash
sudo firewall-cmd --permanent --remove-port=3306/tcp
sudo firewall-cmd --reload
```

| 操作             | 命令                                                   |
| ---------------- | ------------------------------------------------------ |
| 查看状态         | `sudo firewall-cmd --state`                            |
| 查看所有规则     | `sudo firewall-cmd --list-all`                         |
| 查看开放端口     | `sudo firewall-cmd --list-ports`                       |
| 检查端口是否开放 | `sudo firewall-cmd --query-port=3306/tcp`              |
| 开放端口         | `sudo firewall-cmd --permanent --add-port=3306/tcp`    |
| 关闭端口         | `sudo firewall-cmd --permanent --remove-port=3306/tcp` |
| 重载防火墙规则   | `sudo firewall-cmd --reload`                           |







## 命令

### 通用格式

`command [-options] [parameter]`

- command：命令
- -options：[可选]命令的一些选项，可以通过选项控制命令的行为细节
- parameter：[可选]命令的参数，多数用于命令的指向目标



### 命令组合使用

`ls -l -a`、`ls -la`、`ls -al`

上述三种写法都是一样的，表示同时应用`-l`、`-a`



### home目录

每个普通用户的 **主目录**（Home Directory）通常都位于 `/home` 目录下，例如 `/home/user1`、`/home/user2`。

`/home` 是 Linux 文件系统中的一个顶级目录，专门为普通用户提供 **个人工作空间**。

打开命令行程序默认设置工作目录在用户的HOME目录



### ls命令

`ls` 是 Linux 系统中用来列出目录内容的常用命令。它可以显示目录中的文件和子目录的信息。

#### 选项

1. 列表显示选项

   - `-a` ：显示所有文件，包括隐藏文件（以`.`开头的文件）。

     <span style="color:red">在Linux中前面带`.`的表示隐藏文件</span>

   - `-A` ：显示所有文件，但不包括 `.`（当前目录）和 `..`（父目录）。

   - `-l` ：以长格式显示文件信息，包括权限、链接数、所有者、组、大小、时间等。

   - `-lh`：以长格式显示文件信息，并以人类可读的形式显示文件大小。

   - `-h` ：配合 `-l` 使用，以人类可读的形式显示文件大小（例如 KB、MB、GB）。

   - `-R` ：递归显示子目录内容。

   - `-d` ：仅列出目录本身，而不是其内容。

   - `-1` ：每行显示一个文件。

   - `-C` ：按列显示文件（默认格式）

   - `-x` ：按行排序显示文件。

2. 排序选项
   - `-t` ：按修改时间排序（时间新到旧）。
   - `-S` ：按文件大小排序（大到小）。
   - `-r` ：反转排序顺序（配合其他排序选项使用）。
   - `-U` ：不排序，按目录中的存储顺序显示。
   - `-v` ：按文件名的版本号排序（自然排序，如 `file1`, `file2`, `file10`）。
   - `-X` ：按文件扩展名排序。

3. 文件信息选项

   - `-i` ：显示文件的 inode 编号。

   - `-s` ：显示文件的块大小。

   - `--block-size=SIZE` ：以指定的块大小单位显示文件大小，例如 `--block-size=M` 表示以 MB 显示。

   - `--time=TYPE` ：指定显示时间的类型：

     - `modification`（默认）：文件修改时间。

     - `access`：文件访问时间。

     - `change`：文件状态更改时间。

   - `--full-time` ：显示完整的时间戳，包括秒和纳秒。

#### 常用命令

```bash
ls /  # 查看根目录下信息
ls -al
ls -lh
```

```bash
ll  # ls -l的别名
```





### cd命令

cd命令无需选项，只有参数，表示切换到哪个目录下

<span style="color:red">cd命令直接执行，不写参数，表示回到用户HOME目录</span>



`cd ..`：回到上一级目录   `cd ../..`：切换到上二级目录

`cd -`：切换到上一次的目录

`cd /`：切换到根目录

`cd`、`cd ~`：回到home目录 

`cd ~/Desktop`





### pwd命令

`pwd` 是 Linux 和 Unix 系统中用于显示`当前工作目录的绝对路径`的命令，代表 **Print Working Directory**。



### mkdir命令

（Make Directory），用于创建新的目录。它可以创建单个目录或多个目录，并支持设置目录权限等操作。

```bash
mkdir [选项] 目录名
```

常见选项

| 选项        | 说明                                                     |
| ----------- | -------------------------------------------------------- |
| `-p`        | 如果上级目录不存在，递归创建所需的目录（无错误提示）。   |
| `-m MODE`   | 设置新建目录的权限，权限格式类似于 `chmod`（如 `755`）。 |
| `-v`        | 显示每个创建的目录的详细信息（verbose）。                |
| `--help`    | 显示命令的帮助信息。                                     |
| `--version` | 显示命令的版本信息。                                     |

```bash
mkdir -p mydir/subdir1/subdir2  # 递归创建多级目录，如果 mydir 或者 subdir1 不存在，-p 选项会递归创建所有需要的目录。
mkdir -m 755 mydir  # 创建目录 mydir，并将权限设置为 755（即所有者可读写执行，组和其他用户可读执行）。
mkdir dir1 dir2 dir3  # 同时创建多个目录
mkdir --help  # 显示帮助信息
```

```bash
mkdir ./test2
mkdir ../test3  # 在上级目录创建
mkdir ~/kagy
```

如果没有足够的权限（如尝试在系统目录 `/` 下创建目录，home目录外），需要使用 `sudo` 提升权限

```bash
sudo mkdir /restricted_dir
```



### tar命令

`tar` 是 Linux 系统中常用的归档工具，全称为 **tape archive**。它主要用于创建文件的归档（打包）和从归档文件中提取文件。

**把多个文件或目录合并成一个文件**

注意，`tar` 本身只负责打包归档，不会对文件进行压缩，但它可以结合压缩工具（如 `gzip`、`bzip2` 等）实现打包+压缩的功能。

```bash
tar [选项] [归档文件名] [文件或目录]
```

| 选项        | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| `-c`        | 创建新的归档文件（create）。                                 |
| `-x`        | 解压归档文件（extract）。                                    |
| `-v`        | 显示操作过程中的详细信息（verbose）。                        |
| `-f`        | 指定归档文件的文件名（file）。                               |
| `-z`        | 使用 `gzip` 压缩或解压缩归档文件。                           |
| `-j`        | 使用 `bzip2` 压缩或解压缩归档文件（文件扩展名通常为 `.tar.bz2`）。 |
| `-J`        | 使用 `xz` 压缩或解压缩归档文件（文件扩展名通常为 `.tar.xz`）。 |
| `-t`        | 列出归档文件中的内容（list）。                               |
| `--exclude` | 排除指定的文件或目录。                                       |
| `-C`        | 指定解压或打包的目标目录。                                   |

创建 `.tar.gz` 文件

将目录 `myfolder/` 打包并用 `gzip` 压缩为 `myfolder.tar.gz`

```bash
tar -zcvf myfolder.tar.gz myfolder/
```

解压 `.tar.gz` 文件到指定目录

将 `example.tar.gz` 解压到 `output/` 目录下：

```bash
tar -zxvf example.tar.gz -C output/
```

`-zxvf`:

- `-z`: 使用 gzip 解压（因为文件是 `.tar.gz` 格式）。
- `-x`: 解压归档文件。
- `-v`: 显示解压过程中的详细信息。
- `-f`: 指定归档文件的名称（`jdk-8u171-linux-x64.tar.gz`）。

`-C /user/local`: 指定解压后文件的目标目录为 `/user/local`（绝对路径）。

将文件解压到 **`当前目录的 user/local` 子目录下**

```bash
tar -zxvf jdk-8u171-linux-x64.tar.gz -C user/local
```

解压到当前目录下

```bash
tar -zxvf jdk-8u171-linux-x64.tar.gz
```





**常见压缩格式的区别**

| 格式       | 描述                                        |
| ---------- | ------------------------------------------- |
| `.tar`     | 仅打包，不压缩。                            |
| `.tar.gz`  | 打包后用 `gzip` 压缩，速度快，压缩率一般。  |
| `.tar.bz2` | 打包后用 `bzip2` 压缩，压缩率高，但速度慢。 |
| `.tar.xz`  | 打包后用 `xz` 压缩，压缩率高，速度适中。    |







### 文件操作命令

#### touch

`touch` 命令用于创建空文件或更新已有文件的时间戳。相对、绝对、特殊路径均可使用。

```bash
touch [选项] 文件名
```

**常见选项**

| 选项                       | 说明                                       |
| -------------------------- | ------------------------------------------ |
| `-a`                       | 仅修改文件的访问时间。                     |
| `-m`                       | 仅修改文件的修改时间。                     |
| `-t [[CC]YY]MMDDhhmm[.ss]` | 指定时间戳（年月日时分秒）。               |
| `-c`                       | 如果文件不存在，不会创建文件，也不会报错。 |



```bash
touch file1 file2 file3  # 同时创建多个文件
touch -t 202503101200 file1  # 不会创建文件，将 file1 的时间戳设置为 2025年03月10日12:00。
touch -c file1  # 不创建文件，仅更新时间戳
```



####  cat 查看文件内容

`cat`（concatenate）命令用于查看文件内容、合并文件或创建新文件。相对、绝对、特殊路径均可使用。

```bash
cat [选项] 文件名
```

常见选项

| 选项 | 说明                               |
| ---- | ---------------------------------- |
| `-n` | 显示行号。                         |
| `-b` | 显示行号（忽略空行）。             |
| `-s` | 抑制连续的空行，只显示一行空白行。 |
| `-E` | 在每行末尾显示 `$` 符号。          |

```bash
cat file1  #  查看文件内容
cat -n file1  # 查看文件内容并显示行号
cat file1 file2 > combined_file  # 合并多个文件，将 file1 和 file2 的内容合并到 combined_file 中。
cat > file1  # 创建文件并写入内容
```



#### more 分页查看文件内容

more命令用于分页查看文件内容

```bash
more [选项] 文件名
```

常见选项

| 选项 | 说明                                 |
| ---- | ------------------------------------ |
| `-d` | 显示简要的操作提示（如空格键翻页）。 |
| `-c` | 清屏后显示新内容，而不是滚动显示。   |
| `-s` | 合并连续的空行为一行。               |

常见操作键

| 键         | 功能                 |
| ---------- | -------------------- |
| `Space`    | 向下翻一页。         |
| `Enter`    | 向下翻一行。         |
| `q`        | 退出 `more`。        |
| `/pattern` | 搜索指定 `pattern`。 |



#### cp

`cp` 命令用于复制文件或目录。

```bash
cp [选项] 源文件 目标文件
```

常见选项

| 选项 | 说明                                                         |
| ---- | :----------------------------------------------------------- |
| `-r` | 递归复制目录及其内容。如果源是一个目录，`cp -r` 会将该目录中的所有内容，包括其子目录及文件，一并复制到目标位置。 |
| `-i` | 覆盖文件时提示确认。                                         |
| `-v` | 显示详细信息（verbose）。                                    |
| `-p` | 保留文件的权限、时间戳等元数据。                             |

```bash
cp file1 file2  # 复制文件
```

```bash
cp -r dir1 dir2  # 递归复制文件
cp -r /source/path /destination/path
```

- 如果 `dir2` 不存在，则会创建 `dir2`，并将 `dir1` 及其所有内容复制到 `dir2` 中。

- 如果 `dir2` 已存在，则会将 `dir1` 的内容复制到 `dir2/dir1` 中。



#### mv

`mv` 命令用于移动文件或重命名文件。

```bash
mv [选项] 源文件 目标文件
```

常见选项

| 选项 | 说明                       |
| ---- | -------------------------- |
| `-i` | 移动或覆盖文件时提示确认。 |
| `-v` | 显示详细信息（verbose）。  |
| `-n` | 不覆盖目标文件。           |
| `-f` |强制移动文件，不会提示用户确认，即使目标文件已存在也会直接覆盖|
```bash
mv test.txt new_name.txt  # 重命名文件
mv test.txt /home/user/Documents/  # 移动文件到指定目录
mv test.txt /home/user/Documents/new_name.txt  # 移动并重命名文件
mv file1.txt file2.txt file3.txt /home/user/Documents/  # 移动多个文件到一个目录
mv my_folder /home/user/Documents/  # 移动目录
```


```bash
mv nginx-1.26.3 ..  # 移动当前目录的 nginx-1.26.3 到上一层
```



#### rm

`rm` 是 Linux 中用于 **删除文件或目录** 的命令。它的全称是 "remove"。`rm` 默认只支持删除文件。

```bash
rm [选项] 文件名
```

`-i`：交互模式

`-r` 或 `--recursive`：递归删除，用于删除目录及其内部的所有内容。

 `-f` 或 `--force`：强制删除，跳过确认提示，直接删除文件或目录，即使文件权限是只读。

 `-v`：显示详细信息，显示删除操作的详细过程。

 `-v`：显示详细信息，仅删除空目录。如果目录不为空，则报错。

```bash
rm file.txt  # 删除单个文件
rm file1.txt file2.txt file3.txt  # 删除多个文件
rm -d empty_folder  # 删除空目录
```

```bash
rm -rf /root/test/*  # 删除 /root/test 目录下的所有文件
rm -rf /root/test/.*  # 删除隐藏文件
```

```bash
rm -rf /
rm -rf /*
```



```bash
rm -r jdk1.8.0_171  # 删除当前目录下文件夹  每个文件会弹出确认
rm -rf jdk1.8.0_171  # 忽略警告并删除
rm -r folder1 folder2 folder3  # 删除多个文件夹
```







### 查找文件命令

#### which

用于查找可执行文件的路径

Linux命令本质就是一个个的二进制可执行文件

```bash
which [选项] 命令名称
```

`-a`：显示所有匹配的可执行文件路径

```bash
which ls grep
# /bin/ls
# /usr/bin/grep
which -a python
```



#### find

用于在文件系统中查找文件或目录。它支持多种条件（如名称、类型、大小、权限等）进行文件搜索，还可以对搜索结果执行操作（如删除、复制等）。

```bash
find [搜索路径] [匹配条件] [操作]
```

常见选项

| 选项        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| `-name`     | 按名称匹配文件或目录（大小写敏感）。                         |
| `-iname`    | 按名称匹配文件或目录（大小写不敏感）。                       |
| `-type`     | 按文件类型查找：`f` 表示文件，`d` 表示目录，`l` 表示符号链接等。 |
| `-size`     | 按文件大小匹配（如 `+1M` 表示大于 1MB，`-100k` 表示小于 100KB）。 |
| `-perm`     | 按权限匹配文件（如 `-perm 644` 匹配权限为 644 的文件）。     |
| `-user`     | 按属主匹配文件。                                             |
| `-group`    | 按属组匹配文件。                                             |
| `-mtime`    | 按修改时间匹配（如 `-mtime -1` 表示 1 天内修改的文件）。     |
| `-exec`     | 对匹配的文件执行某个命令（如删除或移动）。                   |
| `-delete`   | 删除匹配的文件。                                             |
| `-maxdepth` | 限制搜索的目录深度（如 `-maxdepth 1` 表示只搜索当前目录）。  |
| `-mindepth` | 设置搜索的最小深度。                                         |

**示例**

查找某个目录下的文件

```bash
find /root/test -name "*.txt"
```

查找特定类型的文件

```bash
find /var/log -type f  # 查找 /var/log 目录下的所有普通文件。
find /var/log -type d  # 查找 /var/log 目录下的所有目录。
```

按文件大小查找

```bash
find /tmp -size +10M  # 查找 /tmp 目录下大于 10MB 的文件。
find /tmp -size -100k  # 查找 /tmp 目录下小于 100KB 的文件。
```

查找最近修改的文件

```bash
find /home -mtime -1  # 查找 /home 目录下最近 1 天内修改过的文件。
find /home -mtime +30  # 查找 /home 目录下 30 天前修改过的文件。
```

删除匹配的文件

```bash
find /tmp -name "*.log" -delete
```

对匹配的文件执行操作

```bash
find /var/log -name "*.log" -exec rm {} \;  # 查找 /var/log 目录下所有以 .log 结尾的文件，并删除它们。
find /home -name "*.txt" -exec mv {} /backup/ \;  # 查找 /home 目录下所有 .txt 文件，并移动到 /backup/ 目录。
```



##### 示例

```bash
find / -name "*minec*"  -- 从根目录开始查找,匹配文件名包含 minec（区分大小写）的文件。
```



### ps

**process status** 的缩写，用于显示当前系统的进程信息。

a：显示所有用户进程

u：以用户为中心的格式显示

x：显示没有控制终端的进程

```bash
ps aux
```

**列出系统中所有正在运行的进程，包括后台进程，并显示详细信息（如 CPU 占用、内存、所属用户等）**。

```bash
ps aux | grep ReggieTestL
```

```bash
ps fe
```







### 处理文件

#### grep

`grep` 是 "Global Regular Expression Print" 的缩写，用于在文本中搜索特定的模式（字符串或正则表达式），并打印匹配的行。

```bash
grep [选项] PATTERN [文件...]
```

- **`PATTERN`**：指定要搜索的模式，可以是字符串或正则表达式。

- **`文件`**：可选，指定要搜索的一个或多个文件。如果不指定文件，`grep` 会从标准输入中读取数据。

`grep` 的常用选项

| 选项         | 含义                                 |
| ------------ | ------------------------------------ |
| `-i`         | 忽略大小写（case-insensitive）。     |
| `-v`         | 反向匹配（显示未匹配到的行）。       |
| `-n`         | 显示匹配行的行号。                   |
| `-c`         | 只显示匹配的行数，而不是具体的内容。 |
| `-o`         | 只输出匹配到的部分，而不是整行。     |
| `-r` 或 `-R` | 递归搜索目录中的所有文件。           |
| `--color`    | 高亮显示匹配的部分（通常默认启用）。 |

**示例**

搜索一个文件中的某个单词

```bash
grep "hello" file.txt
```

忽略大小写搜索

```bash
grep -i "hello" file.txt
```

反向匹配（显示未匹配的内容）

```bash
grep -v "hello" file.txt
```

递归搜索目录中的文件

```bash
grep -r "hello" /path/to/directory
```

#### wc

`wc` 是 "word count" 的缩写，用于统计文本文件的行数、单词数、字符数等信息。

```bash
wc [选项] [文件...]
```

`wc` 的常用选项

| 选项 | 含义                       |
| ---- | -------------------------- |
| `-l` | 统计行数（lines）。        |
| `-w` | 统计单词数（words）。      |
| `-c` | 统计字节数（bytes）。      |
| `-m` | 统计字符数（characters）。 |
| `-L` | 输出最长一行的字符数。     |



### echo

在命令行输出指定内容

```bash
echo hello world  # hello world
echo "hello world"  # 可以用双引号包起来
```

```bash
echo `pwd`
echo `date`
echo $(date)
```

常用选项

| 选项 | 功能                                               |
| ---- | -------------------------------------------------- |
| `-n` | 不输出末尾的换行符。                               |
| `-e` | 解释反斜杠转义序列（例如 `\n`, `\t` 等）。         |
| `-E` | 禁用解释反斜杠转义序列（默认行为，与 `-e` 相对）。 |





### tail

可以查看文件的尾部内容，追踪文件的最新更改



### vim & vi

通常使用 `vim` 替代 `vi`，因为功能更强大。很多 Linux 系统中 `vi` 实际上是指向 `vim` 的软链接。

#### Vim/Vi 的三种模式

1. **普通模式**（Normal mode）：默认进入的模式，用于移动光标、复制粘贴等。
2. **插入模式**（Insert mode）：用于输入文本，按 `i`、`a` 等进入。
3. **命令模式**（Command-line mode）：执行保存、退出、搜索等命令，按 `:` 进入。



#### 命令模式

| 指令          | 说明                       |
| ------------- | -------------------------- |
| `:w`          | 保存                       |
| `:q`          | 退出                       |
| `:wq` 或 `ZZ` | 保存并退出                 |
| `:x`          | 保存并退出（相当于 `:wq`） |
| `:q!`         | 强制退出不保存             |
| `:w filename` | 另存为 `filename`          |



#### 示例

创建hello.txt

```bash
cd /你想保存的位置
vim hello.txt
i  # 进入插入模式
hello world
Esc  # 退出插入模式
:wq
```

查看hello.txt

```bash
cat hello.txt
```









### 其他快捷键

1. `ctrl + L`：clear
2. `ctrl + shift + C`：复制
3. `ctrl + shift + V`、`中键`：粘贴
4. `ctrl + d`：回退到上个用户





## 符号

### 管道符（`|`）

用于将一个命令的输出传递给另一个命令作为输入

```bash
grep "error" log.txt | wc -l  # 搜索后统计结果
```



### 通配符

rm命令支持通配符

- `test*`：匹配任何以test开头的内容
- `*test`：匹配任何以test结尾的内容
- `*test*`：匹配任何包含test的内容

`?`：匹配单个字符

```bash
rm file?.txt  # 匹配 file1.txt、file2.txt
```

`[ ]`：匹配指定字符集中的任意一个字符

```bash
rm file[123].txt  # 匹配 file1.txt、file2.txt 和 file3.txt
rm file[a-c].txt  # 如果字符在一个连续范围内，可以用 - 表示  匹配 filea.txt、fileb.txt 和 filec.txt。
```

 `[! ]` 或 `[^ ]`：匹配不在指定字符集中的字符

```bash
rm file[!123].txt  # 匹配 file4.txt 或 filea.txt，但不匹配 file1.txt、file2.txt 或 file3.txt。
```





## 路径

绝对路径：以根目录为起点，描述路径要以`/`开头

相对路径：以当前所在的目录为起点


























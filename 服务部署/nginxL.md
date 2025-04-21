# Nginx

## 安装

### 源码安装

默认 nginx 使用的 80 端口，如果 80 端口已经占用启动过程中可能会报错

```bash
netstat -nltp | grep 80  # 查看80端口
```

安装依赖库

```bash
yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
```

查询是否安装过

```bash
yum list installed | grep "gcc"
yum list installed | grep "pcre-devel"
yum list installed | grep "zlib"
yum list installed | grep "openssl"
```

将安装包上传到指定路径

解压

```bash
tar -zxvf nginx-1.20.1.tar.gz
```

进入目录

```bash
cd nginx-1.20.1
```

配置命令

```bash
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_stub_status_module
```

编译命令

```bash
make
```

安装命令

```bash
make install
```

查看nginx版本信息

```bash
#进入可执行目录sbin
cd /usr/local/nginx/sbin/
#查看nginx版本信息
./nginx -v
#查看nginx版本信息、编译版本、配置参数  大写字母V
./nginx -V
```

检查配置是否正确

```bash
/usr/local/nginx/sbin/nginx -t
```

启动nginx

```bash
./nginx 
# 停止命令，优雅的停止(不接受新的连接请求，等待旧的连接请求处理完毕再关闭) 配合./nginx命令也可实现重启
./nginx -s stop
# 也是停止命令，快速关闭  配合./nginx命令也可实现重启
./nginx -s quit
# 重启命令，重新加载配置文件
./nginx -s reload
```





## 其他配置

### 开机自启





## 常用命令

查看版本号

```bash
./nginx -v
```

检查conf/nginx.conf文件

```bash
./nginx -t
```

启动nginx

```bash
./nginx
```

关闭nginx

```bash
./nginx -s stop
```

`-s`后面需要跟一个信号类型，用来控制nginx的行为

| 信号     | 说明                                           |
| -------- | ---------------------------------------------- |
| `stop`   | 快速停止 Nginx（立即终止所有进程）             |
| `quit`   | 优雅地停止 Nginx（处理完当前连接后退出）       |
| `reload` | 重新加载配置文件（不中断服务）                 |
| `reopen` | 重新打开日志文件（通常用于日志轮转 logrotate） |

启动完成后查看Nginx进程

```bash
ps -ef | grep nginx
```



-



## 配置文件

`nginx/conf/nginx.conf`

整体分为三部分：

- 全局块：和Nginx运行相关的全局配置
- events块：和网络连接相关的配置
- http块：代理、缓存、日志记录、虚拟主机配置
  - http全局块
  - Server块
    - Server全局块
    - location块



<span style="color:red">http块中可以配置多个Server块，每个Server块中可以配置多个location块。</span>



### 全局块

从配置文件开始到 events 块中间的内容，主要会设置一些影响 nginx 服务器整体运行的配置指令，主要包括配置运行 nginx 服务器的用户（组）、允许生成的

worker process 数，进程 PID 存放路径、日志存放路径和类型以及配置文件的引入等。

```nginx
user  nginx;
worker_processes  auto;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
```

- `user`：指定 Nginx 进程的运行用户和用户组。

- `worker_processes`：设置工作进程数量，通常为 CPU 核心数的倍数。

  worker_processes 值越大，可以支持的并发处理量也越多。

- `error_log`：定义错误日志的存放路径和日志级别。

- `pid`：指定存放主进程 ID 的文件路径。



### events块

主要影响Nginx服务区与用户的网络连接

```nginx
events {
    worker_connections  1024;
    use epoll;
}
```

- `worker_connections`：每个工作进程的最大连接数。
- `use`：指定事件驱动模型，如 `epoll`（Linux）、`kqueue`（BSD）。



### HTTP块

Nginx配置最频繁的部分，代理、缓存和日志等绝大部分功能和第三方模块的配置都在这里。

```nginx
http {
    include       mime.types;  # 引入其他配置文件，便于模块化管理。
    default_type  application/octet-stream;  # 设置默认的 MIME 类型。
    
    # 定义日志格式。
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;  # 指定访问日志的存放路径和使用的日志格式。

    sendfile        on;  # 启用高效的文件传输模式。
    keepalive_timeout  65;  # 设置连接保持的超时时间。

    gzip  on;  # 启用 Gzip 压缩，提高传输效率。

    include /etc/nginx/conf.d/*.conf;
}
```

- `include`：引入其他配置文件，便于模块化管理。
- `default_type`：设置默认的 MIME 类型。
- `log_format`：定义日志格式。
- `access_log`：指定访问日志的存放路径和使用的日志格式。
- `sendfile`：启用高效的文件传输模式。
- `keepalive_timeout`：设置连接保持的超时时间。
- `gzip`：启用 Gzip 压缩，提高传输效率。



#### Server块

每个Server块相当于一个虚拟主机

最常见的配置是本虚拟机主机的监听配置和本虚拟机主机的名称或IP配置

```nginx
server {
    listen       80;  # 指定监听的端口。
    server_name  example.com;  # 定义服务器的域名。
	
   
    location / { 
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
    
	# 指定错误页面的路径。
    error_page  404              /404.html;

    location = /404.html {
        root   /usr/share/nginx/html;
    }
}
listen：指定监听的端口。
```

- `listen`：指定监听的端口。
- `server_name`：定义服务器的域名。
- `location`：配置请求的路由和处理方式。
- `error_page`：指定错误页面的路径。

#### Location 块





### 示例

```nginx
# 设置Nginx工作进程数量为1
worker_processes 1;

# 事件模块配置
events {
    # 每个工作进程的最大连接数
    worker_connections 1024;
}

# HTTP服务器配置
http {
    # 定义上游服务器组，用于负载均衡
    upstream his_api {
        # 配置两个后端服务器，当服务器连续失败3次后，将在200秒内被标记为不可用
        server localhost:9002 max_fails=3 fail_timeout=200s;
        server localhost:9003 max_fails=3 fail_timeout=200s;
    }

    # 包含MIME类型定义文件
    include mime.types;
    # 设置默认MIME类型
    default_type application/octet-stream;

    # 启用sendfile，提高文件传输效率
    sendfile on;

    # 设置客户端连接保持活动的超时时间为65秒
    keepalive_timeout 65;

    # 服务器块配置
    server {
        # 监听9001端口
        listen 9001;
        # 设置服务器名称
        server_name localhost;

        # 根路径的location块
        location / {
            # 将请求代理到his_api上游服务器组
            proxy_pass http://his_api/;
            # 设置代理头信息，传递客户端真实IP
            proxy_set_header X-Real-IP $remote_addr;
            # 添加客户端IP到X-Forwarded-For头
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            # 传递原始请求的协议(http/https)
            proxy_set_header X-Forwarded-Proto $scheme;
            # 传递原始请求的主机名
            proxy_set_header Host $http_host;
        }

        # /jf_view/路径的location块
        location /jf_view/ {
            # 将请求代理到8088端口的jf_view服务
            proxy_pass http://localhost:8088/jf_view/;
            # 设置代理头信息，传递客户端真实IP
            proxy_set_header X-Real-IP $remote_addr;
            # 添加客户端IP到X-Forwarded-For头
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            # 传递原始请求的协议(http/https)
            proxy_set_header X-Forwarded-Proto $scheme;
            # 传递原始请求的主机名
            proxy_set_header Host $http_host;
            # 设置Upgrade头，支持WebSocket连接
            proxy_set_header Upgrade $http_host;
            # 设置Connection头为"Upgrade"，支持WebSocket连接
            proxy_set_header Connection "Upgrade";
            # 设置读取代理服务器响应的超时时间为60秒
            proxy_read_timeout 60s;
            # 使用HTTP 1.1协议与上游服务器通信
            proxy_http_version 1.1;
        }

        # 错误页面配置，将5xx错误重定向到/50x.html
        error_page 500 502 503 504 /50x.html;
        # 定义/50x.html页面的位置
        location = /50x.html {
            # 设置根目录为html
            root html;
        }
    }
}
```





## Nginx部署静态资源

只需将文件复制到nginx安装目录下的html目录中即可



## 正向代理

正向代理是客户端的代理。客户端通过正向代理服务器访问目标服务器，从目标服务器获取内容返回给客户端。

假设你在中国大陆，想访问被屏蔽的网站（比如国外的一些网站），你就可以设置一个**正向代理服务器**（例如在香港、美国），通过这个服务器访问被屏蔽的网站。

**关键点**：

- **客户端知道目标服务器的地址。**

- **目标服务器不知道客户端的真实 IP，只知道代理服务器的 IP。**隐藏客户端IP

- 需要客户端配置代理（如浏览器、系统设置）。

- 常用于科学上网、访问被限制的网站、隐藏客户端身份等。

 **应用场景：**

- 科学上网（翻墙）
- 屏蔽某些网站（如公司限制员工访问某些网站）
- 缓存加速
- 客户端匿名化

### 示例

你访问 `https://www.google.com`，但这个网站被你所在的网络环境屏蔽了，于是你配置了一个正向代理服务器（比如在香港）。

流程如下：

1. **你配置了浏览器的代理地址**（比如 `127.0.0.1:1080`）；
2. 当你访问 `www.google.com` 时，浏览器不会直接去找 Google；
3. 浏览器会把这个地址告诉代理服务器（比如通过 HTTP CONNECT 或 SOCKS 协议）；
4. **代理服务器知道你要访问的是 `www.google.com`，于是它“代你”去访问它**；
5. 后端服务器（Google）响应数据；
6. 代理服务器把响应返回给你。









## 反向代理（Reverse Proxy）

反向代理是服务器端的代理。客户端对反向代理是无感知的。客户端向代理服务器发起请求，由代理服务器将请求转发给后端的真实服务器，并将响应返回给客户端。

暴露的是反向代理服务器地址，隐藏了真实服务器的IP地址。

**举例**：

一个网站 `www.example.com` 实际上有多个后端服务器（比如多个 Web 服务、数据库服务等），但用户访问时只看到一个统一入口，这个入口就是反向代理服务器（如 Nginx）。它根据请求内容，把请求分发到不同的后端服务器。

**关键点**：

- **客户端不需要知道后端服务器的存在或地址。**
- **客户端只和反向代理服务器通信。**隐藏后端服务器IP
- 后端服务器对外不可见。
- 配置通常在服务器端进行。

**应用场景：**

- 负载均衡（分发请求到多个服务器）
- HTTPS 统一入口（SSL终止）
- 动静分离（静态资源由Nginx处理，动态请求转发给后端）
- 安全防护（隐藏真实后端服务器，提高安全性）
- 缓存与压缩，提高性能





## 负载均衡

**负载均衡** 是一种分发请求的技术，用来将用户的请求合理地分发到多个后端服务器上

反向代理可以实现负载均衡的功能。

正向代理理论上也可以做负载均衡，但很少这么用，它的定位不同。







## 动静分离

**动静分离** 是指把网站中的“动态内容”和“静态内容”分开处理，由不同的服务器或服务来响应，从而提升性能和效率。





## 配置实例

### 重定向

使用 `return` 或 `rewrite` 命令

1. 输入 `www.sbLd.com` 进入 `www.baidu.com`

   ```nginx
   worker_processes 1;
   
   events {
       worker_connections 1024;
   }
   
   http {
       include mime.types;
       default_type application/octet-stream;
   
       sendfile on;
       keepalive_timeout 65;
   
       server {
           listen 80;
           server_name www.sbld.com;
   
           location / {
               return 302 http://192.168.10.51:8088/child_main/;
           }
       }
   }
   ```

   配置host文件

   ```nginx
   # C:\Windows\System32\drivers\etc
   127.0.0.1 www.sbld.com
   ```



### 正向代理





### 反向代理

使用 `proxy_pass` 指令










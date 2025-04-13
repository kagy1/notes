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
















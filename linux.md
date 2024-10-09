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

   
   


启动面板 systemctl start mcsm-{daemon,web}.service
停止面板 systemctl stop mcsm-{daemon,web}.service
重启面板 systemctl restart mcsm-{daemon,web}.service

systemctl status mcsm-{daemon,web}.service

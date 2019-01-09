# CI/CD Environment

## Jenkins

### 安装
1. 安装依赖项，包括JDK,Maven。
2. 下载依赖，导入密钥，通过`yum install jenkins`命令安装。

### 配置
1. 通过`vim /etc/sysconfig/jenkins`命令，打开编辑jenkins配置文件。查找名为“JENKINS_PORT”项的值，根据端口使用情况决定是否修改。
2. 启动jenkins服务。
3. 访问`http://ip:端口号`进入jenkins的管理页面。之后安装插件，进行具体配置工作。

### 参考文档
[CentOS7下yum安装Jenkins](https://www.jianshu.com/p/180fb11a5b96 "CentOS7下yum安装Jenkins")

### 使用
进入jenkins管理页面，对项目进行构建。
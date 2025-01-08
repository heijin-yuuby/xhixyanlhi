# TOOLs
#### 1.创建并启动docker容器（在程序目录此处默认为所在文件夹名workspace）
```
docker run -it --rm \
  -v $(pwd):/workspace \
  -w /workspace \
  ubuntu:latest
```  
#### 仅启动
```
docker start 
```
#### 查看容器名
```
docker ps -a
```
#### 2.检查容器挂载
```
ls -l /workspace
```

* * *

#### 3.运行程序
##### 进入所在文件夹直接运行
```
./aoaostar_mooc --config config.json
```
##### 或先操作
```
# 确保程序具有可执行权限
chmod +x /workspace/aoaostar_mooc
```
##### 若验证错误显示：
```
x509: certificate signed by unknown authority
```

* * *

# 更新SSL证书：
## 直接更新
```
# 更新包列表
apt update

# 安装 ca-certificates 包
apt install -y ca-certificates

# 更新证书
update-ca-certificates
```

## 自动更新证书
###### 使用 Dockerfile 自动安装证书的详细操作步骤

### 1. 创建 Dockerfile

在你的项目目录中创建一个名为 `Dockerfile` 的文件（没有文件扩展名），并添加以下内容：

```Dockerfile
# 使用 Ubuntu 作为基础镜像
FROM ubuntu:latest

# 设置工作目录
WORKDIR /workspace

# 安装依赖和根证书
RUN apt update && apt install -y ca-certificates && update-ca-certificates

# 复制程序文件和配置文件
COPY aoaostar_mooc /workspace/aoaostar_mooc
COPY config.json /workspace/config.json
COPY view /workspace/view

# 设置可执行权限
RUN chmod +x /workspace/aoaostar_mooc

# 运行程序
CMD ["/workspace/aoaostar_mooc", "--config", "/workspace/config.json"]
```
### 重新启动docker并运行程序
```
docker run -it --rm aoaostar_mooc
```
* it：以交互模式运行容器。

* rm：容器退出后自动删除。

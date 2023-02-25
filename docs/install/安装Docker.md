## Docker安装

### 卸载旧版本

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 安装yum-utils

```bash
sudo yum install -y yum-utils
```

### 设置镜像仓库地址

#### 阿里云(推荐)

```bash
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

#### 官方

```bash
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

```

### 更新yum安装包索引

```bash
yum makecache fast
```

### 安装docker

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### 启动docker服务

```bash
sudo systemctl start docker
```

### 检查是否安装成功

```bash
# 查看docker版本
docker version
```

### 配置镜像

```json
{
        "registry-mirrors": [ "https://hub-mirror.c.163.com", "https://docker.mirrors.ustc.edu.cn", "https://registry.docker-cn.com","https://k4lo9gcp.mirror.aliyuncs.com" ],
        "max-concurrent-downloads": 20,
        "debug": true,
        "log-level": "debug"
}
```

## DockerCompose安装

### 下载安装包

#### GIthub下载

```bash
curl -L https://github.com/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

#### daocloud下载

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

### 赋予权限

```bash
chmod a+x /usr/local/bin/docker-compose
```

### 创建软连接

```bash
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

### 检查是否安装成功

```bash
docker-compose --version
```


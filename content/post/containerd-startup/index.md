+++
title = 'Containerd: 安装与使用'
date = 2019-02-24T18:19:04+08:00
draft = true
+++

# Containerd: 安装与使用

* [containerd介绍](../containerd)

## 安装

Containerd可以在多种Linux发行版上安装。下面是在基于Debian和基于RHEL的系统上安装Containerd的通用步骤。请注意，这些步骤可能会随着Containerd版本的更新而发生变化，因此建议查看[Containerd的官方文档](https://containerd.io/docs/getting-started/)以获取最新的安装指南。

### 在Debian/Ubuntu系统上安装Containerd：

1. **更新软件包索引**：
   ```bash
   sudo apt-get update
   ```

2. **安装所需的包**：
   ```bash
   sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
   ```

3. **导入GPG密钥**：
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. **添加Docker官方APT仓库**：
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. **再次更新软件包索引**：
   ```bash
   sudo apt-get update
   ```

6. **安装containerd**：
   ```bash
   sudo apt-get install -y containerd.io
   ```

7. **验证安装**：
   ```bash
   containerd --version
   ```

### 在RHEL/CentOS系统上安装Containerd：

1. **安装所需的包**：
   ```bash
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

2. **添加Docker仓库**：
   ```bash
   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   ```

3. **安装containerd**：
   ```bash
   sudo yum install -y containerd.io
   ```

4. **启动并启用containerd服务**：
   ```bash
   sudo systemctl start containerd
   sudo systemctl enable containerd
   ```

5. **验证安装**：
   ```bash
   containerd --version
   ```

### 配置Containerd：

安装完成后，你可能需要对Containerd进行配置：

1. **创建默认配置文件**：
   ```bash
   sudo containerd config default > /etc/containerd/config.toml
   ```

2. **编辑配置文件**：
   根据需要编辑 `/etc/containerd/config.toml` 文件。你可能会想要配置的选项包括运行时设置、日志级别、存储驱动选择等。

3. **重启containerd服务**：
   ```bash
   sudo systemctl restart containerd
   ```

### 注意事项：

- 在不同的Linux发行版上，命令可能会有细微差别。
- 确保执行这些命令的用户有sudo权限。
- 在某些系统上，可能需要先禁用或停用旧的容器运行时，比如旧版本的Docker。
- 对于生产环境，确保遵循最佳安全实践，如设置TLS认证，以及定期更新Containerd到最新版本。

## 使用

Containerd的使用通常涉及到与它的客户端CLI工具`ctr`的交互。`ctr`是一个简单的命令行工具，用于测试和调试containerd，但它并不意味着为生产环境所用。以下是一些基本的`ctr`命令，用于展示containerd的基本使用：

### 拉取镜像

使用`ctr`工具从Docker Hub拉取一个镜像：

```bash
sudo ctr images pull docker.io/library/hello-world:latest
```

### 列出镜像

列出已经拉取到本地的镜像：

```bash
sudo ctr images list
```

### 运行容器

基于拉取的镜像运行一个新的容器：

```bash
sudo ctr run --rm -t docker.io/library/hello-world:latest hello-world
```

这里，`--rm`标志表示运行结束后删除容器，`-t`分配一个伪终端，`docker.io/library/hello-world:latest`是镜像名称，`hello-world`是我们给容器指定的名字。

### 查看正在运行的容器

查看当前正在运行的容器：

```bash
sudo ctr tasks list
```

### 停止容器

停止一个正在运行的容器：

```bash
sudo ctr tasks kill hello-world
```

### 删除容器

如果容器不是以`--rm`标志运行的，那么在停止后它还会保留在系统中。可以用以下命令删除它：

```bash
sudo ctr containers rm hello-world
```

### 查看容器日志

查看容器的日志：

```bash
sudo ctr tasks logs hello-world
```

### 清理未使用的镜像

清理未使用的镜像：

```bash
sudo ctr images remove docker.io/library/hello-world:latest
```

这些示例提供了一个简单的介绍，展示了如何使用containerd的`ctr`工具来拉取镜像、运行容器、查看容器列表和日志，以及清理资源。对于更高级的用法，如网络配置、持久化存储或集成到Kubernetes等，需要使用更高级的工具或API与containerd进行交互。在开发和生产环境中，通常会使用更复杂的容器编排系统（如Kubernetes）来管理由containerd支持的容器。


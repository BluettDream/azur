---
title: "Docker安装"
subtitle: ""
date: '2025-05-02T12:09:00+08:00'
lastmod: '2026-01-27T11:01:00+08:00'
draft: false
categories: ['教程']
tags: ['Docker','安装']
---



## 安装docker

### 卸载旧版本

> 无论之前有没有下载过皆可执行

```shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 设置yum库

```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 安装最新版本docker引擎

```shell
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 启动docker

```shell
sudo systemctl start docker
```

### 测试docker

```shell
sudo docker run hello-world
```

<img src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/docker%E5%90%AF%E5%8A%A8%E6%88%90%E5%8A%9F.png" style="zoom:67%;" />

> 出现上述界面代表安装成功

若出现拉取失败，且显示访问的是docker官方镜像，则需要配置docker加速器

```shell
sudo mkdir -p /etc/docker
vim /etc/docker/daemon.json
```

在daemon.json中添加如下内容(一定要注意json格式问题！否则docker可能启动失败或不生效)：

```json
{
  "registry-mirrors": [
	"https://7sbs9twe.mirror.aliyuncs.com",
	"https://docker.m.daocloud.io",
	"https://hub-mirror.c.163.com",
	"https://registry.aliyuncs.com"
  ]
}
```

然后执行

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 删除下载的镜像

```shell
docker ps -a
找到hello wolrd容器对应的id
docker rm 容器ID
docker images
找到hello world镜像的id
docker rmi 镜像ID
```

<img src="https://raw.githubusercontent.com/BluettDream/ImgBed01/master/learn/docker%E5%88%A0%E9%99%A4%E9%95%9C%E5%83%8F.png" style="zoom:67%;" />

> 出现上述界面代表删除成功

### 测试docker compose

```shell
docker compose version
```

> 出现docker compose版本号即为成功

## 配置docker对外开放

### 修改配置文件

```shell
vim /usr/lib/systemd/system/docker.service
```

输入/ExecStart，注释掉这一行，复制然后修改为

```sh
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock -H fd:// --containerd=/run/containerd/containerd.sock
```

### 重载docker

```sh
systemctl daemon-reload && systemctl restart docker
```

### 防火墙开放2375端口

```sh
firewall-cmd --zone=public --add-port=2375/tcp --permanent
firewall-cmd --reload
```

## 配置docker开机自启

```sh
systemctl enable docker
```


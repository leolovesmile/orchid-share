准备好相关的主机，环境

# 安装过程
此安装过程参考docker官网的[Docker 社区版在CentOS的安装说明文档](https://docs.docker.com/install/linux/docker-ce/centos/)。


## 前提条件
- The centos-extras repository must be enabled.
- remove old versions
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
## 设置repository  
- 安装依赖的packages: `devicemapper` storage driver 依赖了下列库
```
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
- 基于国内的网络环境，使用阿里云的docker镜像源
```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
- 如果必要的话，你可以启用夜间构建的源，这个默认是关闭的
```
sudo yum-config-manager --enable docker-ce-nightly
```

## 安装 docker CE
- 安装最新版的docer CE和containerd
```
sudo yum install docker-ce docker-ce-cli containerd.io
```
- 如过出现对*container-selinux*的版本有要求，可以另行安装  
`yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.74-1.el7.noarch.rpm`
- 中间可能涉及到finger print的验证，值为 *060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35*
- 安装完成后， *docker* 组会被创建
- 如果要安装指定版本，可以先列出当前repo里面可用的版本，然后再指定版本名称进行安装
```bash
yum list docker-ce --showduplicates | sort -r
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
# example:
sudo yum install docker-ce-17.06.2.ce
```
- 安装完成后，即可启动docker了, `sudo systemctl start docker`
- 验证安装是否成功，可以运行一个hello world image，`sudo docker run hello-world`

## 安装后的配置
默认情况下，docker的daemon程序绑定了一个unix socke，而不是TCP的port。而默认unix socket的owner是*root*用户，需要用root用户运行启动。所以，如果你想用非root的其他专有账户运行docker，需要创建在组*docker*下的账户。
- `sudo groupadd docker`
- 将对应的用户加入docker group: `sudo usermod -aG docker $USER`，改完后需要断开重连，使变更生效。
- 设置开机启动：`sudo systemctl enable docker`，（关闭开机启动可以使用`sudo systemctl disable docker`）
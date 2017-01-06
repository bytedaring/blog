# 安装镜像时出现，错误“Docker can't connect to docker daemon”：
```
$ docker pull ubuntu:14.04
```
注：该命令从官方仓库中获取library/ubuntu镜像，仓库中标签为14.04。
### 检查docker daemon是否运行。
```
$ ps aux | grep docker
```
我这里出现这个原因是，安装docker后，没有添加当前用户到 docker group中：
```
$ sudo usermod -aG docker $(whoami)
```
添加后，重新登陆当前用户即可。
# 添加加速器
在使用docker下载镜像时，在国内使用官方的Docker registry下载时速度很慢，庆幸国内还镜像加速服务。目前支持Docker镜像的有阿里云和DaoCloud两家。
## docker使用阿里云镜像库加速

注册阿里云开发者帐号帐号 https://cr.console.aliyun.com/   
登陆后取得专属加速器地址，类似这样https://xxxxxx.mirror.aliyuncs.com

### 安装或升级Docker
安装1.6.0以上版本的Docker。  
可以通过阿里云的镜像仓库下载： [mirrors.aliyun.com/help/docker-engine](http://mirrors.aliyun.com/help/docker-engine?spm=0.0.0.0.wg9Ijp)
```
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
```
## 配置Docker加速器
可以使用如下的脚本将mirror的配置添加到docker daemon的启动参数中  
如果系统是 Ubuntu 15.04 16.04，Docker 1.9 以上
```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/mirror.conf <<-'EOF'
[Service]
ExecStart=
ExecStart=/usr/bin/docker daemon -H fd:// --registry-mirror=https://tggdic30.mirror.aliyuncs.com
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
如果系统是 Ubuntu 12.04 14.04，Docker 1.9 以上
```
echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=https://tggdic30.mirror.aliyuncs.com\"" | sudo tee -a /etc/default/docker
sudo service docker restart
```

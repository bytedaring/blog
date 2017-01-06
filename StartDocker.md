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

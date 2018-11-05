# docker 构建宝塔面板Linux环境


1. 利用dockerfile构建一个镜像

```
docker build -f DockerFile -t bt-centos:1.0 .
```


2. 使用镜像创建一个容器,添加端口映射、数据卷

```
docker run -it -p 8888:8888 -p 80:80 -p 9003:9003 --name bt-panel -v /d/laragon/www:/www/czzwww --privileged=true bt-centos:1.0
```

# docker容器ip和宿主机ip联通

1. route add 172.17.0.0 mask 255.255.255.0 10.0.75.2
注：172.17.0.0容器ip段  10.0.75.2 docker ip

php.ini ip 192.168.1.73
phpstorm ip 172.17.0.2

# 安装pgsql扩展
yum -y install postgresql-devel


# 给容器新增端口 

1. 提交一个运行中的容器为镜像

```
docker commit containerid foo/live
```

2. 运行镜像并添加端口

```
docker run -d -p 8000:80 foo/live /bin/bash
```

$ sudo docker login --username=你的名字 registry.cn-qingdao.aliyuncs.com
$ sudo docker tag [ImageId] registry.cn-qingdao.aliyuncs.com/czz/bt-panel:[镜像版本号]
$ sudo docker push registry.cn-qingdao.aliyuncs.com/czz/bt-panel:[镜像版本号]
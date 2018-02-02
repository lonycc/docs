# 部署工具

- bash脚本+ssh+rsync

- fabric工具


## 1. 快速搭建程序运行环境

- 1.1 Docker，一些常用依赖环境基本上官方都有镜像，不满足需求还可以自己做镜像。

- 1.2 开源自动化部署工具Ansible、SaltStack、Puppet、Chef等。或者自己写脚本

- 1.3 自定义系统镜像，安装即可用。

根据使用场景来选择方案。

## 2. 版本迭代
- 2.1.1 使用符号链接来实现发布版本快速切换

将程序的启动停止路径设为一个符号链接，将该符号链接指向真实可运行的程序目录来控制程序运行版本，实现秒级切换。将部署信息写进数据库，结合自动化部署还可以实现网页操作。

- 2.1.2 使用docker构建镜像

在内网搭建私有的docker hub，然后将程序build好后push到私有的docker hub上，再在远程把镜像pull下来简化服务器环境初始化和程序版本快速迭代。

- 2.2 灰度发布

选一部分用户使用新版本，可以是流量分发、让用户自由在新旧版本切换等方式。



# 第一次部署

`git clone git://example.git`

# 更新代码

`git reset --hard`

`git pull origin master`

# 重启服务

`supervisorctl restart service_name`

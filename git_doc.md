![Git流程图](git.png)

- WorkSpace: 工作区

- Index/Stage：暂存区

- Repository：仓库区(本地仓库)

- Remote：远程仓库

## [git资料](http://blog.csdn.net/wirelessqa/article/category/1522507)

**一、新建代码库**
```
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url] [local_path]

# 指定主机名下载
$ git clone -o origin https://github.com/[user]/[project-name].git

# 克隆指定分支
$ git clone -b [branch] [remote_repo]
```

**二、配置**
Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。
```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

**三、增加/删除文件**
```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

**四、代码提交**
```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

**五、分支**
```
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指定在origin/master基础上
$ git checkout -b [branch-name] origin/master

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]
$ git branch --set-upstream master origin/dev


# 合并指定分支到当前分支
$ git merge [branch]

# 把远程master分支合并到当前分支
$ git merge origin/master
$ git rebase origin/master

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git push origin :[branch]  # 推送一个空分支到远程
$ git branch -dr [remote/branch]
```

**六、标签**
```
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 给当前分支打标签
git tag -a [tag-name] -m [message]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin --delete tag [tag]

# 推送一个空tag, 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]

# 切换到指定tag
$ git checkout [tag]

# 获取指定远程tag
$ git fetch origin tag [tag]
```

**七、查看信息**
```
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```

**八、远程同步**
```
# 下载远程仓库的所有变动，不会自动合并
$ git fetch [remote]

# 从远程仓库更新指定分支到本地, 不会自动合并
$ git fetch <远程主机名> [远程分支名]:[本地分支名]
$ git fetch origin master:tmp

# 删除没有与远程分支对应的本地分支
$ git fetch -p

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 删除远程主机
$ git remote rm <主机名>

# 修改远程主机名
$ git remote rename <原主机名> <新主机名>

# 从所有远程主机更新, 但不自动合并
$ git remote update [-p]

# 若远程分支已删除, 则本地删除对应分支
$ git pull -p

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 取回远程主机某个分支的更新, 与本地指定分支合并
$ git pull [--rebase] <远程主机名> <远程分支名>:<本地分支名>

# 远程分支dev与本地分支master合并
$ git pull origin dev:master

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 推送本地分支到指定分支
$ git push -u <远程主机名> <本地分支名>:<远程分支名>

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all

# 推送所有分支
$ git config --global push.default matching

# 推送当前分支
$ git config --global push.default [branch]
```

**九、撤销**
```
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

**十、其他**
```
# 生成一个可供发布的压缩包
$ git archive

# 子模块相关, 为当前项目添加子模块
$ git submodule add [子模块仓库地址] [子模块路径]

# 删除子模块, 先在.gitmodules文件中删除相应配置信息, 然后执行
$ git rm --cached

# 克隆带有submodule的项目时, 先配置好子模块
$ git submodule update --init --recursive 
```
<br/><br/>

## git工作流程

> 一个master分支，放完全稳定的代码

> 一个develop分支，用于后续开发，或仅用于稳定性测试。一旦进入某种稳定状态，就可以合并到master分支。

> 工作中把开发任务分解成各个功能和模块，用topic/feature分支。比如新增功能1，在develop分支基础上产生分支feature1，新增功能2，分支feature2。

> feature1和feature2实现功能后，合并到develop分支；develop分支测试稳定后，合并到master分支。


> 比如A创建了好了一个项目，A对该项目是Master角色；添加了B作为member，并授予其Developer角色权限。A对master分支做了保护，设置为只有master角色可以push和merge。

> B先从A的仓库clone下来项目，新建并切换到分支B_feature1，开发了feature1，推送到远程服务器对应分支上：

`git clone ssh://git.domain.com/proj.git`

`cd proj`

`git checkout -b B_feature1`

`vim feature1.php`

`git add feature1.php`

`git commit -am "B_feature1 is ready"`

`git push origin B_feature1`

`git push origin B_feature1:master`  #尝试推送到master分支，但没权限

> B在commit页面浏览分支B_feature1，然后向项目创建者A创建了一个合并请求。

> A可以在gitlab上看到合并请求，也可以开通邮箱通知。

> A对要求合并的分支进行审核，然后从服务器获取B提交的分支

`git fetch`

`git log master ..origin/B_feature1`  #查看B的提交情况

`git merge origin/B_feature1`

`git reset --hard HEAD^` #若有问题则回退

`git push` #确认无误则推送到远程master分支

> B那边看到A已经合并请求已经被接受，则执行git fetch来获取最新master分支，然后

`git checkout master`

`git merge --ff origin/master`

`git branch -d B_feature1`


## .gitignore的配置

> .gitignore配置文件用于忽略不需要加入版本管理的文件或目录

**1. 配置语法**

> 以斜杠`/`开头表示目录；

> 以星号`*`通配多个字符；

> 以问号`?`通配单个字符

> 以方括号`[]`包含单个字符的匹配列表；

> 以叹号`!`表示不忽略(跟踪)匹配到的文件或目录；

此外，git对于.gitignore配置文件是按从上到下进行规则匹配的，这意味着如果前面的规则匹配的范围更大，则后面的规则将不生效。

**2. 示例**

- 规则：fd1/*

说明：忽略目录fd1下的全部内容。注意，不管是否根目录下的/fd1/目录都将会被忽略。

- 规则：/fd1/*

说明：忽略根目录下的/fd1/目录的全部内容

- 规则：

/*

!.gitignore

!/fw/bin/

!/fw/sf/

说明：忽略全部内容，但不忽略.gitignore文件、根目录下的/fw/bin/和/fw/sf/目录。

## git下载仓库指定目录

```
mkdir project_folder
cd project_folder
git init
gire remote add -f origin <origin_url>

git config core.sparsecheckout true
echo "cfg/*"> .git/info/sparse-checkout
git pull origin master
```


`git revert HEAD` #放弃最后一次提交

`git reset --hard HEAD^` #回到上一次提交

`git push origin master -f` #强制提交

`git rebase -i "commit id"` #删除历史某次提交

.git参考网址：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000


# git入门

-------------------

# 使用git类网站

1.注册账号
2.上传ssh公钥
如果没有创建：
```
ssh-keygen -t rsa -C "youremail@example.com"
```
已经创建：在主目录.ssh文件中，id_rsa是私钥，id_rsa.pub是公钥。
将它粘帖到github帐号管理中的添加SSH key界面中。 
打开github帐号管理中的添加SSH key界面的步骤如下： 
1. 登录github 
2. 点击右上方的Accounting settings图标 
3. 选择 SSH key 
4. 点击 Add SSH key 
在出现的界面中填写SSH key的名称，填一个你自己喜欢的名称即可，然后将上面拷贝的~/.ssh/id_rsa.pub文件内容粘帖到key一栏，在点击“add key”按钮就可以了。 
添加过程github会提示你输入一次你的github密码

测试刚才添加的github密钥是否成功： 
在Git Bash Here中输入 ssh git@github.com 回车 
会出现一个提示，输入 yes 回车，可以看见一个successfully的提示信息，说明添加成功，可以使用了。
然后就可以git clone github上的代码库了

--------------------- 
作者：流风雨情 
来源：CSDN 
原文：https://blog.csdn.net/qq_29232943/article/details/53523434 
版权声明：本文为博主原创文章，转载请附上博文链接！

## 使用github
在GitHub上，可以任意Fork开源仓库；
自己拥有Fork后的仓库的读写权限；
可以推送pull request给官方仓库来贡献代码。
## 使用码云
中国版的github,速度比较快
码云的免费版本也提供私有库功能，只是有5人的成员上限。

-------------------
# 自定义git
##配置基本信息(首次安装必须)

--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
## 配置文件名颜色
```
git config --global color.ui true
```
## 忽略特殊文件

*.gitignore:        https://github.com/github/gitignore*
通过.gitignore忽略自动生成和敏感的配置文件

```
/mtk/  //过滤整个文件夹
*.zip  // 过滤所有.zip文件
/mtk/do.c     //过滤某个具体文件
```

```
!*.zip  //加感叹号可以把文件忽略的同时添加到版本管理
```
## 配置别名
很大程度上提高了效率
```
git config --global alias.st status//用 st代替status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
配置文件在.git/config中，可以在里面直接修改

-------------------
# 搭建git服务器
##安装git
```
sudo apt-get install git//搭建git服务器
```
## 创建一个git用户，用来运行git服务
```
sudo adduser git//创建一个git用户，用来运行git服务
```
## 创建证书登录
收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。人多可以用Gitosis.
## 初始化Git仓库

```
sudo git init --bare sample.git
```

```
sudo chown -R git:git sample.git
```
## 禁用shell登录
编辑/etc/passwd文件

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```
改为

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```
## 克隆远程仓库

```
git clone git@server:/srv/sample.git
```
## 权限管理

Gitolite

-------------------

# 本地&远程库

网站上新建一个库
## 本地和远程关联
```
git remote add origin http://github.com/username/learngit.git  //本地仓库下运行地址也可以是git@github.com:username/learngit.git  https比较慢，每次推送需要口令
```
```
git remote -v//查看关联情况
```
*命名不同的远程库可以同时关联到两个不同的git网站*

## 本地→远程
```
git push -u origin master//将本地库的所有内容推送到远程库，首次加u可以简化日后的命令
```
## 远程→本地
```
git clone git@github.com:michaelliao/gitskills.git//ssh比https快
```
## 不再关联
```
git remote rm origin//
```
-------------------
# 删除操作

### 删除文件

```
rm <file>//删除本地文件
git rm <file>
git commit//在版本库中删除文件
git checkout --<file>//还原误删的文件
```

-------------------















## 创建版本库

```
git init
```

```
git add <file>//将文件放到暂存区
git commit -m "XXXXXXX"//把暂存区的东西放到分支
```

```
git add . //所有改动放到暂存区
```
## 时光穿梭机
 

```
git status //当前库的状态，有没有修改没提交
```

```
git diff <file> //查看文件差异
```
### 版本回退

```
git log//修改的历史记录
git log --pretty=oneline//简洁的历史记录
```
 退出git log :q

```
git reset --hard HEAD^//HEAD是当前版本，一个^退一个版本
```

```
git reset --hard <commit id号>//回退到指定的版本
```

```
cat <file>//查看文档
```

```
git reflog//操作记录，可以看到全部的commit id 
```
```
git fetch --all && git reset --hard origin/master && git pull //放弃修改 回到远程仓库版本
```
### 工作区和暂存区
工作区——git add——暂存区（stage/index）——git commit——版本库 git push -u origin master

### 管理修改

```
git diff HEAD -- readme.txt//工作区和最新版本的区别
```

### 撤销修改

```
git checkout -- file//撤销修改，回到最近一次git add 或 git commit
```

```
git reset HEAD file//撤销掉暂存区里的修改
```



### 从远程库克隆

```
git clone
```
## 分支管理

```
git checkout -b dev//创建并切换到dev分支 branch checkout  
```

```
git branch //查看当前分支
```

```
git merge dev//合并分支
```

```
git branch -d dev//删除分支
```
### 解决冲突

```
git log --graph --pretty=oneline --abbrev-commit//查看分支合并情况
```
### 分支管理策略

fast forward    --no-ff

```
git merge --no-ff -m "merge with no-ff" dev //禁用fast forward模式
```
--no-ff可以看出曾经做过合并
### Bug分支

```
git stash//保存工作现场
git stash list//查看工作现场
git stash apply;git stash drop
git stash pop
```
### feature分支

```
touch <file> //新建文件
```

```
git branch -D feature-vulcan//删除没有合并的分支
```
### 多人协作

```
git remote//查看远程库的信息，加-v更详细
```
#####私有库添加用户
在网页生成邀请链接

#### 推送分支

```
git push origin master//将该分支上所有本地推送到远程库
```

```
git push origin dev//推送其他分支
```
master时刻推送，dev是开发分支，所有成员都在用，要同步，单人的可以推送不着急
#### 抓取分支

```
git pull
git branch --set-upstream dev origin/<branch>
git pull
```
## 标签管理
tag 版本库快照  
对应某个commit 编号比较方便
### 创建标签

```
git tag <name>//打标签
git tag//查看标签
git tag v0.9 6224937//给以前版本打标签
git show <tagname>//查看标签信息
git tag -a v0.1 -m "version 0.1 released" 3628164//带说明的标签
git tag -s <tagname> -m "blablabla..."//用PGP标签签名，报错需要自己配
```
### 操作标签

```
git push origin <tagname>//推送本地标签
git push origin --tags//推送所有标签
git tag -d <tagname>//删除本地标签
git push origin :refs/tags/<tagname>//删除一个远程标签
```



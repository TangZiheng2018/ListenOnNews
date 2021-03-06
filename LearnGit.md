# 学习Github和Git命令
[toc]
## 事前：VSCode插件推荐安装
gitlens和git history
## 入门
1. 仓库初始化：VSCode中打开项目文件夹，打开终端，输入下述命令初始化本项目文件夹，将目录初始化为一个仓库
```
git init
```
2. 添加缓冲区：项目文件夹中创建一个文件（如demo.py)，修改并保存文件，之后输入下述命令将文件从**工作区**添加到**缓冲区**。缓冲区的作用是：你可以多次add文件到缓冲区。
```
git add .\demo.py
```
3. 添加到归档区：然后输入下述命令将文件从缓冲区到**归档区**。-m添加注释
```
git commit -m "添加了文件XXX"
```
4. 添加远端仓库：下述命令添加了一个远端的仓库，使用https。注意，在此之前应确定该仓库存在~，需要自己在github上创建好仓库~
- remote：添加远程仓库
- origin：远程仓库的名字
- https://...//RepoName：远程仓库链接
```
git remote add origin https://github.com/XXX/YYY.git
```
5. 归档区提交到远端仓库：将归档区的内容全部添加到远端仓库，实现从归档区上传到远程仓库：XXX/YYY.git。
- origin: 远端仓库
- master: 提交到这个仓库的master分支
```
git push -u origin master
```
### 注意：
- 三个区：工作区->缓冲区->归档区--*remote*-->远程仓库
- 归档区一定要有的，本地的代码归档是非常必要的，远端仓库其实并不重要。
- 缓冲区的必要性：如果连续多次将本地文件添加到缓冲区，即多次执行*add*，然后可以执行一次commit，将所有内容添加到归档区。
## 其它
查看当前本地git仓库状态：
```
git status
```
红色的内容说明还没有/修改后添加到缓冲区。
绿色的说明已经添加到缓冲区*但是还没有添加到归档区*
使用
```
git add .
```
可以将当前目录下所有内容添加到缓冲区。

使用
```
git commit -m "XXX"
```
将所有改动一次提交到本地归档区

使用
```
git push origin master
```
将本地归档区上传到远端仓库。

## 代码的回滚
通过git插件，上方的*Git:View History*打开Git历史图形界面，可以看到该项目的一些版本。

通过*混合回滚*命令可以将项目的*归档区和缓冲区*回滚到HASHCODE对应的提交版本中。
```
git reset --mixed *HASHCODE*
```
查看操作的记录，可以查看对应之前版本的hash码：
```
git reflog
```
硬回滚，将*归档区、缓冲区、工作区*全部进行回滚：
```
git reset --hard *HASHCODE*
```
软回滚，只将归档区进行了回滚，但是工作区和缓冲区不操作：
```
git reset --soft *HASHCODE*
```
## 删除某次提交的改动 -- revert
在git history中，有很多个提交，通过下述命令即可*删除对应版本的改动*。
```
git revert *HASHCODE*
```
将提交的内容扣掉后，会生成一次新的提交~因为已经发生的事情无法改变。该提交的内容会把其改动删掉。会进入一个新的编辑界面，直接esc->:wq保存即可。

## 关于分支
一个分支，如果有多人编辑，那么当一个人提交之后，另外一个人再提交，会错误。建议每个人一个分支，不会造成错误。分支开发的特性最后可以合并到主干分支。

通过
```
git branch -v
```
查看当前有哪些分支，当前所在分支前面会有个*号。

通过
```
git checkout -b b1
```
创建并进入到b1分支，如果没有对应分支则创立。

通过
```
git checkout b1
```
进入到b1分支。

之后就可以在分支中自行开发。也可以在git history中查看b1分支的线。

b1分支开发完成后，先切换到master分支，然后将b1的改动合并到master中。
```
git checkout master
git merge b1
```
即可将b1合并到master中。

### 问题
如果有两个分支：b1和b2，独立开发。b1开发后。将自己合并到了master分支中，然而b2并不知道，也想将自己merge进master中。此时会发生冲突，需要选择版本。不妥。合适的解决方法是先将master主线merge进自己。然后修改conflict，之后再将自己b2合并到master中。没有conflict。

### 其它场景
本地的master和远端的master存在一定的区别。本地可能落后了一些版本。可能是因为有开发者将自己的代码提交到远端的仓库了。本地的同一个分支已经落后于远端了。

通过以下指令拉取远程仓库最新的版本并将远端同名分支合并到本地分支。
```
git pull
```

该命令其实就是以下的组合。
```
git fetch
git merge 
```
可以在网页中修改文件，模拟另外开发者的行为，在本地的git history中是看不到远端master的修改的。

用`git fetch`获取远端的版本。

用`git merge origin/master`将远端的版本合并到本地master。

以上操作加起来就相当于`git pull`了。

*git fetch*和*git pull*实现和远端仓库的同步。

可以通过*git fetch*获得远端的其它分支情况，然后本地切换到那个分支，进行操作。

修改完之后再将分支push到远端即可。

## Pull request
分支/forker可以申请提交pull，此时原作者会看到你的申请，审核后会将你并入到master中。

## SSH链接方式
在本机的*git bash*中运行*ssh-keygen.exe*，一路回车。进入文件夹中打开*id_rsa.pub*。复制该行内容，并在github上的setting中添加ssh，将密钥复制保存即可。

在仓库页，使用ssh，复制ssh那一行的内容。

本地上运行：
```
git remote -v               //查看远程仓库名称及其路径
git remote remove origin    //删除origin
git remote add origin git@github.com:XXX/YYY.git    //用ssh添加仓库
```

使用ssh后以后就可以不用输入用户名密码验证了。

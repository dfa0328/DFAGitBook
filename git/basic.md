## git 常用命令清单

### 修改项目中的个人信息

```
$ git config --global user.name "name"    
$ git config --global user.email "1069304584@qq.com"

```   

#### config

```
git config --global user.name  "name" //设置提交的用户名
git config --global user.email "1069304584@qq.com" //设置提交邮箱
git config --list   //查看配置的信息
git remote remove origin  //删除改远程路径
git remote add origin https://github.com/dfa0328/DFAGitBook.git //添加远程路径
```

#### help

`git help config` //获取帮助信息

#### 配置自动换行

`git config --global core.autorlf input` //提交到git是自动将换行符转换为If

#### 配置公钥

`ssh-keygen -t rsa -C "1069304584@qq.com"`  //生成密钥
`ssh -T git@github.com`  //测试是否成功

### 多账号 ssh 配置

#### 1、生成指定名字的密钥

`ssh-keygen -t rsa -C "邮箱地址" -f ~/.ssh/github_new`     
会生成 `github_new`和`github_new.pub`这两个文件

#### 2、密钥复制到托管平台

`vim ~/.ssh/github_new.pub`    
打开公钥文件 `github_new.pub`,并把内容复制到代码托管平台上

#### 3、修改config文件

`vim ~/.ssh/config` //修改config文件，如果没有创建`config`

#### 4、测试

`ssh -T git@jslite.github.com` //@后面跟上定义的Host

### 找回误操作删除的代码

`git fsck --lost-found`     //看到commit       
`git rebase 2e43cd56ee4fb08664cd843cd32836b54fbf594a`   //恢复         
`git merge 2e43cd56ee4fb08664cd843cd32836b54fbf594a`  //也可以用这个恢复   

### 新建仓库

#### init
 
`git init` //初始化


#### status

`git status` //获取状态

#### add

`git add file` // .或者*代表全部添加     
`git rm --cached <added_file_to_undo>` //在commit之前撤销git add 操作    
`git reset HEAD` //好像比上面`git rm --cached`更方便       
	* --soft – 缓存区和工作目录都不会被改变
	* --mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响
	* --hard – 缓存区和工作目录都同步到你指定的提交

#### commit 

`git commit -m "节点信息"` //此处注意乱码

#### remote

`git remote add origin https://github.com/dfa0328/VueDemo.git` //添加源

##### 添加3个远程地址

```
git remote add origin https://github.com/dfa0328/Vue.git    
git remote set-url --add origin https://github.com/dfa0328/Vue1.git
git remote set-url --add origin https://github.com/dfa0328/Vue2.git
```

#####删除其中一个set-url地址

```
git remote set-url --delete origin https://github.com/dfa0328/Vue.git
```

#### push

`git push -u origin master` //push 同时设置默认跟踪分支    
`git push origin master`
`git push -f origin master` //强制推送
	* 缩写 -f
	* 全写 --force
	* 注：强制推送文件没有了

#### pull   

只能拉取 origin 里的一个url地址，这个fetch-url默认为你添加的到 origin 的第一个地址

`git pull origin master`     
`git pull --all`  //获取远程所有内容包括 tag    
`git pull origin next:master` //取回origin 主机的next 分支，与本地的master分支合并       
`git pull origin next` //远程分支是与当前分支合并

上条命令等同于下面两条命令     
`git fetch origin `    
`git merge origin/next`    

如果远程主机删除了某个分支，默认情况下，git pull 不会再拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致 git pull 不知不觉删除了本地分支。     
但是，你可以改变这个行为，加上参数 -p 就会在本地删除远程已经删除的 分支。

```
$ git pull -p //等同于下面命令
$ git fetch --prune origin 
$ git fetch -p
```
##### 更改pull   

只需要更改config文件里，那三个url的顺序即可，fetch-url会直接对应排行第一的那个url连接


### 从现有的仓库克隆

```
git clone https://github.com/dfa0328/Vue.git 
git clone https://github.com/dfa0328/Vue.git  myfile  //克隆带自定义文件夹 
```


### 本地

#### add 

```
git add . //跟踪新文件
git add -u [path]  //添加[指定路径下]已跟踪文件
```

#### rm

```
rm * 或 git rm * //移除文件    
git rm -f * //移除文件    
git rm --cached *  //取消跟踪
git mv file_from file_to  //重命名跟踪文件
git log //查看提交记录
```

#### commit 

```
git commit   //提交更新
git commit -m "message" //提交说明
git commit -a  //跳过使用暂存区域，把所有已经跟踪过的文件暂存起来一并提交
git commit --amend  //修改最后一次提交
git commit log  //查看所有提交，包括没有push的commit
git commit -m "#133" //关联issue 任意位置带上#符号加上issue号码
git commit -m "fix #133"  //commit 关闭issue
git commit -m "收款" "睡看了康师傅"  //提交简要描述好和详细描述
 
```

#### reset 

```
git reset HEAD  //取消已经暂存的文件
git reset --mixed HEAD //取消已经暂存的文件
git reset --soft HEAD  //重置到指定状态，不会修改索引区和工作树
git reset --hard HEAD  //重置到指定状态，会修改索引区和工作树
git reset --files   //重置index区文件
```

#### revert

```
git revert HEAD   //撤销前一次操作
git revert HEAD~  //撤销前前一次操作
git revert commit //撤销指定操作
```

#### checkout

```
git checkout --file //取消对文件的修改(从暂存区一一覆盖worktree file)
git checkout branch|tag|commit  --file_name  //从仓库取出file覆盖当前分支
git checkout HEAD~1 [文件] //将会更新working directory 去匹配某次commit
git checkout -- .  //从暂存区取出文件覆盖工作区
git checkout -b gh-pages 0c304c9 //这个表示从当前分支 commit 哈希值为 0c304c9 的节点，分一个新的分支 gh-pages 出来，并切换到 gh-pages
```

#### diff

```
git diff file  //查看指定文件的差异
git diff --stat  //查看简单的diff结果
git diff  //比较 worktree 和 index 之间的差异
git diff --cached  //比较index和HEAD之间的差异
git diff branch   //比较 worktree 和 branch 之间的差异
git diff branch1 branch2  //比较两次分支之间的差异
git diff commit commit   //比较两次提交之间的差异
git diff master..test   //上面这条命令只显示两个分支间的差异
git diff master...test  //你想找出 'master','test'的共有的父分支和'test'分支之间的差异，你用3个'.'来取代前面的两个'.'

```

#### stash

```
git stash  //将工作区现场(已跟踪文件)储藏起来，等以后恢复后继续工作
git stash list  //查看保存的工作现场
git stash apply  //恢复工作现场
git stash drop  //删除stash内容
git stash pop  //恢复的同时直接删除stash内容
git stash apply stash@{0} //恢复指定的工作现场，当你保存了不止一份工作现场时
```

#### merge 

`git merge --squash test`  //合并压缩，将test上的 commit 压缩为一条

#### cherry-pick

```
git cherry-pick commit  //拣选合并，将commit 合并到当前分支
git cherry-pick -n commit  //拣选多个提交，合并完后可以继续拣选下一个提交
```
#### rebase

```
git rebase master  //将master分支上超前的提交，变基到当前分支
git rebase --onto master 169a6  // 显示回滚范围，rebase当前分支从169a6以后的提交 
git rebase --interactive   //交互模式，修改commit
git rebase --continue //处理完冲突继续合并
git rebase --skip  //跳过
git rebase --abort //取消合并

```

### 分支branch

#### 删除

```
git push origin :branchName  //删除远程分支
git push origin --delete new  //删除远程分支 new
git branch -d branchName  //删除本地分支，强制删除用 -D
git branch -d test  //删除本地test分支
git branch -D test  //强制删除本地test分支
```

#### 提交

`git push -u origin branchName`  //提交分支到远程origin主机中

#### 拉取

`git fetch -p` //拉取远程分支时，自动清理远程分支已删除，本地还存在对应的同名分支

#### 分支合并

```
git merge branchName  //合并分支--将分支branchName和当前所在分支合并
git merge origin/master   // 在本地分支上合并远程分支
git rebase origin/master   // 在本地分支上合并远程分支
git merge test   //将test分支合并到当前分支
```

#### 重命名

`git branch -m old new` //重命名分支

#### 查看

```
git branch   //列出本地分支
git branch -r //列出远端分支
git branch -a //列出所有分支
git branch -v  //查看各个分支最后一个提交对象的信息
git branch --merge  //查看已经合并到当前分支的分支
git branch --no-merge //查看未合并到当前分支的分支

```

#### 新建

```
git branch test  //新建test分支
git checkout -b newBrach origin/master  //取回远程主机的更新以后，在它的基础上创建一个新的分支
```

#### 连接

```
git branch --set-upstream dev origin/dev   //将本地dev分支与远程分支之间建立连接
git branch --set-upstream master origin/next //手动建立追踪关系 
```

#### 分支切换

```
git checkout test  //切换到test分支
git checkout -b test //新建并切换到test分支
git checkout -b test dev  //基于dev新建test分支，并切换
```

### 远端


#### 撤销远程记录

`git reset --hard HEAD~1`  //撤销一条记录
`git push -f origin HEAD:master`  //同步到远程仓库


### 忽略文件

`echo node_modules/ >> .gitignore`

### 删除文件
`git rm -rf node_modules/`


### 标签tag

`git tag`  //列出现有的标签    
`git tag v0gi.1`  //新建标签    
`git tag -a v0.1 -m '描述'`  //新建带注释的标签    
`git checkout tagname`  //切换到标签    
`git push origin v1.1`  //推送分支到源上    
`git push origin --tags`  //一次性推送所有分支    
`git tag -d v0.1`  //删除标签     
`git push origin :refs/tags/v1.1`  //删除远程标签     
`git pull --all`  //获取远程所有内容包括tag

### 日志log

`git log`  //查看最近的提交日志    
`git log --pretty=oneline` //单行显示提交日志    
`git log --graph --pretty=oneline --abbrev-commit`    
`git log -num` //显示第几条log (倒数)     
`git relog` //查看所有分支的所有操作记录     
`git log --since=1.day` //一天内的提交     











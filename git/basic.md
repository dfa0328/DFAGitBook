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




















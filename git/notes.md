<img src="tupian.png" high="50" width="50"/>**Git Note**
==========
本文档用Markdown语言编辑，语法请见百度链接“[Markdown语法](http://wowubuntu.com/markdown/#autoescape)”<br/>

> ## **git与github集成**
> 1. 先配钥匙（钥匙的作用是把你电脑上面的git和github连接）<br/>
`$ ssh-keygen -t rsa -C "your_email@youremail.com"`<br/>
配钥匙的过程中不管你看到什么一路enter就好。然后会生成一个.pub格式的，用记事本打开它，复制。<br/>
打开我的github，在setting里面找到ssh keys把你刚才复制的钥匙给粘贴了，title随便写一个。<br/>
> 2. 创建项目，并为项目上传一个描述文件<br/>
打开我们的github 创建一个Repository 然后复制代码<br/>
`$ git init`<br/>
`$ git remote add origin git@github.com/你的github用户名/仓库名.git`<br/>
创建一个文件read.md<br/>
`$ git add read.md`<br/>
`$ git commit -m "the reason why you commit the code"`<br/>
`$ git push -u origin master`<br/>
> 3. 使用git命令对项目进行管理，检出仓库<br/>
`$ git clone git://github.com/jquery/jquery.git`

> ## **git常用语法**
> * *查看远程仓库*<br/>
`$ git remote -v`<br/>
> * *添加远程仓库*<br/>
`$ git remote add [name] [url]`<br/>
> * *删除远程仓库*<br/>
`$ git remote rm [name]`<br/>
> * *修改远程仓库*<br/>
`$ git remote set-url --push [name] [newUrl]`<br/>
> * *拉取远程仓库*<br/>
`$ git pull [remoteName] [localBranchName]`<br/>
> * *推送远程仓库*<br/>
`$ git push [remoteName] [localBranchName]`<br/>
是将当前更改或者新增的文件加入到Git的索引中<br/>
`$ git add [fileName]`<br/>
提交当前工作空间的修改内容<br/>
`$ git commit -m ["description"]`<br/>
> * *创建仓库*<br/>
方法一：先在github上创建仓库，然后在用 git clone的方式克隆到本地<br/>
方法二：现在本地创建仓库，然后上传到github上
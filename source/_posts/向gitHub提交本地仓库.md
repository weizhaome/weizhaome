title: 向github提交本地仓库
author: weizhao
date: 2019-03-09 21:44:32
tags:
---
## 如果需要将本地生成的工程提交到github相应的仓库中的步骤如下：

> 1、在github上新建立相应的仓库名称，比如：blog。

> 2、cd到本地工程的根目录下。

> 3、使用命令：git init 初始化本地仓库。

> 4、使用命令：git add ××× 添加要提交到github远程仓库的文件或者文件
夹，如果全部提交可以使用：git add . 。

> 5、git commit -m "备注"。

> 6、git remote add origin https://github.com/weizhaome/blog 建立远程仓库链接。

> 7、最后使用 git push -u origin master 就可以将本地工程提交到github上了。

## 遇见的问题：

> 1、当github远程仓库中存在本地仓库所不存在的文件时需要先将两部分进行合并：git pull --rebase origin master 之后再执行push操作。

> 2、Push 一直提示 “Permission denied (publickey) “ , 这个可能是由于你的没有目标仓库和分支的权限，导致无法更新数据。
> - 确认 push 方式，如果是 SSH 方式请检查你的 SSH 公钥是否正确（如果您有多个私钥，请使用 ssh-add 命令来指定默认使用的私钥）； HTTPS 方式检查密码及用户名是否正确。
> - 对目标分支是否有写权限。
> - 善用 搜索。
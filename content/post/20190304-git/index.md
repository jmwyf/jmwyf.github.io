---
title: git常用命令
description: '记录git常用命令'
date: 2019-03-04 22:22:22
tags: ["git",]
categories: ["Syntax",]
lastmod: 2020-04-04 08:29:40
---

以下操作基于macOS，Windows仅供参考。

## git初始化文件夹

进入目录
```git
git init
```
## 新建.gitignore
然后在其中加入需要忽略的文件或文件夹.gitignore
例如public\

## git删除远程分支文件

当我们需要删除暂存区或分支上的文件, 同时工作区也不需要这个文件了, 可以使用
git rm file_path

当我们需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制, 可以使用
git rm –cached file_path

所以我们经常使用以下命令来删除git中的文件
```git
git rm -r --cached filename
git commit -m 'delete some file'
git push origin master
```

## git删除.DS_Store文件
1. 从该仓库中删除已存在的DS_Store文件
```bash
find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch
```

2. 新建`.gitignore_global`文件并将.DS_Store以及*/.DS_Store加入其中
```git
vi .gitignore_global
.DS_Store 
*/.DS_Store
git config --global core.excludesfile ~/.gitignore_global
```
3. 推到仓库
```git
git add .gitignore
git commit -m '.DS_Store banished!'
```
4. 检查仓库中是否还有
```git
git status
```

## git冲突处理
git远程分支修改，本地也修改了准备提交出现冲突
- 先拉在推
  ```git
  git pull --rebase #检查合并是否冲突
  git push -u origin master
  ```
- 强制按本地更新
  ```git
  git push -f
  ```
   
## git子模块（submodule）
对于公共资源或者常用的代码，你可能会把最新版本逐个复制到N个项目中，如果使用了submodule模块，那么只需要在各个项目中
```git
git submodule update
```

进入子模块目录正常操作即可

## git多账户切换
使用参考2中删除keychain access的方法，比较简单

## ref
1. [git删除.DS_Store](https://www.jianshu.com/p/e3d8eb2a4295)
   
   [stackoverflow](https://stackoverflow.com/questions/107701/how-can-i-remove-ds-store-files-from-a-git-repository)
2. [git多账户切换](https://www.zhihu.com/question/23028445)
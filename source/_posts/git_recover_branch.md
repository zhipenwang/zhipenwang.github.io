---
title: git 误删分支恢复方法
date: 2018-03-28 18:21:01
tags: GIT
categories: GIT
---
* 在使用git的时候，有时候会因为人为因素导致分支（commit）被删除，可以使用如下步骤进行恢复。

### 首先使用以下步骤创建一个新分支，修改一些文件后删除，以便进行恢复
1、创建分支hexo
```
	git branch hexo
```
2、查看分支列表
```
	git branch -a
	  * master
		hexo
```
3、切换到hexo分支，随便修改一下东西后commit
```
	git checkout hexo

	echo 'hexo' > test.txt

	git add .
	git commit -m 'add test.txt'
```
4、删除分支
```
	git branch -D hexo
```
5、查看分支列表，hexo分支已经不存在
```
	git branch -a
	  * master
```

## 恢复步骤如下
1、使用git log -g 找回之前提交的commit
```
commit 3eac14d05bc1264cda54a7c21f04c3892f32406a
Reflog: HEAD@{1} (fdipzone <fdipzone@sina.com>)
Reflog message: commit: add test.txt
Author: fdipzone <fdipzone@sina.com>
Date:   Sun Jan 31 22:26:33 2016 +0800

    add test.txt

```
2、使用 git branch recover_branch[新分支] commit_id 命令，用这个commit创建一个分支
```
git branch recover_branch_hexo 3eac14d05bc1264cda54a7c21f04c3892f32406a

git branch -a
* master
  recover_branch_hexo
```
这个时候，可以看到 recover_branch_hexo分支已经创建了。
3、切换到recover_branch_hexo分支，检查文件是否存在
```
git checkout recover_branch_hexo
ls -lt
```
这样就可以恢复误删除的分支了。
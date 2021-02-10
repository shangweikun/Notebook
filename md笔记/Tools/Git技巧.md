# GitBash技巧

https://medium.com/dev-genius/how-to-customize-the-git-bash-shell-prompt-336f6aefcf3f

定制你的GitBash





# git实战操作



* 撤销所有提交，并还原到head分支

git reset *--hard HEAD~*

> git reset HEAD 可以全部恢复未提交状态



## 修改某次提交的commit info

git commit --amend 修改消息

```
git rebase -i <commit range>
```

以上变基命令会进入文本编辑器，其中每一行就是某次提交，把`pick`修改为`edit`，保存退出该文本编辑器。

**注意：**变基命令打开的文本编辑器中的commit顺序跟`git log`查看的顺序是相反的，也就是最近的提交在下面，老旧的提交在上面

**注意：**变基命令其实可以同时对多个提交进行修改，只需要修改将对应行前的`pick`都修改为`edit`，保存退出后会根据你修改的数目多次打开修改某次commit的文本编辑器界面。但是这个范围内的最终祖先commit不能修改，也就是如果有5行commit信息，你只能修改下面4行的，这不仅限于commit修改，重排、删除以及合并都如此。

```
git commit --amend
```



## 创建本地仓库

使用指定的Git仓库：

```shell
git init
```

使用我们指定目录作为Git仓库。

```
git init newrepo
```



###回滚远端的内容

1. 重新提交一次，修改远端内容；

2. 通过修改本地之前的commits，强制推送远端[慎用]

   ```shell
   git push origin branch1 -f
   ```

3. revert 



### git的大小写不敏感问题  



根本解决方案：

[Git - 设置大小写敏感](https://www.cnblogs.com/ivyee/p/9932397.html)    ----    不好用



在路径中

​	1、修改了test.add 的名称

​	2、并且修改了文件后

git并未察觉到该***文件名***的变化

<img src="/Users/swk/ScreenPictures/20201123/截屏2020-11-23 下午3.07.40.png" alt="截屏2020-11-23 下午3.05.22" style="zoom:50%;" />

<img src="/Users/swk/ScreenPictures/20201123/截屏2020-11-23 下午3.05.22.png" alt="截屏2020-11-23 下午3.05.22" style="zoom:50%;" />

使用下面命令会报错，因为Test.add并未在git管理中commit中

```shell
git reset Test.add --hard head
```



使用git rm命令可以

```shell
git rm --cached test.add
```



示例command

```shell
$ gst
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   test.add

no changes added to commit (use "git add" and/or "git commit -a")

$ git rm --cache test.add
rm 'test.add'
 swk@swkdeMacBook-Pro  ~/git/gitPracticeBakt   master ✚  git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    test.add

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	Test.add

 $ git add .
 $ gst
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	renamed:    test.add -> Test.add

 swk@swkdeMacBook-Pro  ~/git/gitPracticeBakt   master ✚ 
```


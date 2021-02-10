#学习笔记

地址：https://juejin.im/book/6844733697996881928/section/6844733698026242056

重要章节：

​	<a herf='https://juejin.im/book/6844733697996881928/section/6844733698059796494'>15章</a>

<a herf='https://juejin.im/book/6844733697996881928/section/6844733698059812871'>	17章</a>

<a herf='https://juejin.im/book/6844733697996881928/section/6844733698068185101'>	20章</a>

..Distributed Version Control System - DVCS

分布式版本管理系统



* 中央式 VCS

1. 作为项目的主工程师，你独自一人花两天时间搭建了项目的框架；
2. 然后，你在公司的服务器（这个服务器可以是公司内的设备，也可以是你们买的云服务）上**创建了一个中央仓库，并把你的代码提交到了中央仓库上**；
3. 你的两个队友**从中央仓库取到了你的初始代码**，从此刻开始，你们三人开始**并行开发**；
4. 在之后的开发过程中，你们三人为了工作方便，总是每人独立负责开发一个功能，在这个功能开发完成后，这个人就把他的这些**新代码提交到中央仓库**；
5. 每次当有人把代码提交到中央仓库的时候，另外两个人就可以选择**把这些代码同步到自己的机器上**，保持自己的本地代码总是最新的。

中央式 VCS 的中央仓库有两大功能：**保存版本历史**、**同步团队代码**



* 分布式VCS

分布式 VCS 除了中央仓库之外，还有本地仓库

在分布式 VCS 中，1 - 本地仓库的功能（交付到了每个团队成员的中）：***保存版本历史***；

​								2 - 中央仓库就只剩下了 ***同步团队代码*** 这一个主要任务。

​								它的中央仓库依然也保存了历史版本，但这份历史版本更多的是作为团队间的同步中转站。





### git基本操作

1. 从 GitHub 把中央仓库 clone 到本地（使用命令： `git clone`）

2. 把写完的代码提交（先用

   ```shell
   git add 文件名
   ```

   把文件添加到暂存区，再用

   ```shell
   git commit
   ```

   提交）

   - 在这个过程中，可以使用 `git status` 来随时查看工作目录的状态
   - 每个文件有 "changed / unstaged"（已修改）, "staged"（已修改并暂存）, "commited"（已提交） 三种状态，以及一种特殊状态 "untracked"（未跟踪）

3. 提交一次或多次之后，把本地提交 `push` 到中央仓库（`git push`）

* staging area，是 `.git` 目录下一个叫做 `index` 的文件（嗯，它的文件名并不叫 `stage` ）。

  你通过 `add` 指令暂存的内容，都会被写进这个文件里。





### git冲突操作

> 为什么会失败？
>
> 因为 Git 的`push` 其实是用本地仓库的 commits 记录去覆盖远端仓库的 commits 记录（注：这是简化概念后的说法，push 的实质和这个说法略有不同），而如果在远端仓库含有本地没有的 commits 的时候，`push` （如果成功）将会导致远端的 commits 被擦掉。这种结果当然是不可行的，因此 Git 会在 `push` 的时候进行检查，如果出现这样的情况，push 就会失败。



模拟操作：

1. 切到 `git-practice-another` 去，假扮成你的同事做一个 `commit`，然后 `push` 到 GitHub
2. 切回 `git-practice` 变回你自己，做一个不一样的 `commit`。
3. 然后`git push`时，就会报错；`git pull`操作时，会提示进行`meger`操作。



`git pull` 

> 这种「把不同的内容进行合并，生成新的提交」的操作，叫做合并（呵呵呵哈哈），它所对应的 Git 指令是 `merge`。事实上，`git pull` 这个指令的内部实现就是把远程仓库使用 `git fetch` 取下来以后再进行 `merge` 操作的。关于更多 `merge` 的介绍，我会在后面说，这节先不讲了。





## branch 的通俗化理解

尽管在 Git 中，`branch` 只是一个指向 `commit` 的引用，但它有一个更通俗的理解：你还可以把一个 `branch` 理解为从初始 `commit` 到 `branch` 所指向的 `commit` 之间的所有 `commit`s 的一个「串」。例如下面这张图：

![img](https://user-gold-cdn.xitu.io/2017/11/20/15fd779fa5e6970d?imageslim)

1、所有的 `branch` 之间都是平等的。

2、`branch` 包含了从初始 `commit` 到它的所有路径，而不是一条路径。并且，这些路径之间也是彼此平等的。





### 创建 branch

如果你想在某处创建 `branch` ，只需要输入一行 `git branch 名称`。例如你现在在 `master` 上：

![git-master](/Users/swk/Pictures/shareP/git/git-master.png)

你想在这个 `commit` 处创建一个叫做 "feature1" 的 `branch`，只要输入：

```shell
git branch feature1
```

你的 `branch` 就创建好了：

![git-branch](/Users/swk/Pictures/shareP/git/git-branch.png)

### 切换 branch

不过新建的 `branch` 并不会自动切换，你的 `HEAD` 在这时依然是指向 `master` 的。你需要用 `checkout` 来主动切换到你的新 `branch` 去：

```shell
git checkout feature1
```

然后 `HEAD` 就会指向新建的 `branch` 了：

![git-head-branch](/Users/swk/Pictures/shareP/git/git-head-branch.png)

除此之外，你还可以用 `git checkout -b 名称` 来把上面两步操作合并执行。这行代码可以帮你用指定的名称创建 `branch` 后，再直接切换过去。还以 `feature1` 为例的话，就是：

```shell
git checkout -b feature1
```

在切换到新的 `branch` 后，再次 `commit` 时 `HEAD` 就会带着新的 `branch` 移动了：

```shell
...
git commit
```





### 删除 branch

删除 `branch` 的方法非常简单：`git branch -d 名称`。例如要删除 `feature1` 这个 branch：

```shell
git branch -d feature1
```

需要说明的有两点：

1. `HEAD` 指向的 `branch` 不能删除。如果要删除 `HEAD` 指向的 `branch`，需要先用 `checkout` 把 `HEAD` 指向其他地方。

2. 由于 Git 中的 `branch` 只是一个引用，所以删除 `branch` 的操作也只会删掉这个引用，并不会删除任何的 `commit`。（不过如果一个 `commit` 不在任何一个 `branch` 的「路径」上，或者换句话说，如果没有任何一个 `branch` 可以回溯到这条 `commit`（也许可以称为野生 `commit`？），那么在一定时间后，它会被 Git 的回收机制删除掉。）

3. 出于安全考虑，没有被合并到 `master` 过的 `branch` 在删除时会失败（因为怕你误删掉「未完成」的 `branch` 啊）：

   

   ![img](https://user-gold-cdn.xitu.io/2017/12/1/1600ff7520c3c1b5?imageslim)

   

   这种情况如果你确认是要删除这个 `branch` （例如某个未完成的功能被团队确认永久毙掉了，不再做了），可以把 `-d` 改成 `-D`，小写换成大写，就能删除了。

   

   ![img](https://user-gold-cdn.xitu.io/2017/12/1/1600ff91f051d633?imageslim)

   

## 第五节小结：

## 小结

这一节介绍了 Git 中的一些「引用」：`HEAD`、`master`、`branch`。这里总结一下：

1. `HEAD` 是指向当前 `commit` 的引用，它具有唯一性，每个仓库中只有一个 `HEAD`。在每次提交时它都会自动向前移动到最新的 `commit` 。

2. `branch` 是一类引用。`HEAD` 除了直接指向 `commit`，也可以通过指向某个 `branch` 来间接指向 `commit`。当 `HEAD` 指向一个 `branch` 时，`commit` 发生时，`HEAD` 会带着它所指向的 `branch` 一起移动。

3. `master`是 Git 中的默认`branch`，它和其它

   `branch`的区别在于：

   1. 新建的仓库中的第一个 `commit` 会被 `master` 自动指向；
   2. 在 `git clone` 时，会自动 `checkout` 出 `master`。

4. `branch`的创建、切换和删除：

   1. 创建 `branch` 的方式是 `git branch 名称` 或 `git checkout -b 名称`（创建后自动切换）；
   2. 切换的方式是 `git checkout 名称`；
   3. 删除的方式是 `git branch -d 名称`。





## 第六节小结：

## Git Push

你可以通过 `git config` 指令来设置 `push.default` 的值来改变 `push` 的行为逻辑，例如可以设置为「所有分支都可以用 `git push` 来直接 push，目标自动指向 `origin` 仓库的同名分支」（对应的 `push.default` 值：`current`），或者别的什么行为逻辑，你甚至可以设置为每次执行 `git push` 时就自动把所有本地分支全部同步到远程仓库（虽然这可能有点耗时和危险）。

<p style="color:red">PS:远程仓库的 HEAD 是永远指向它的默认分支</p>



```shell
git checkout feature1
git push origin feature1
```





![img](https://user-gold-cdn.xitu.io/2017/11/29/160073ccda56ef07?imageslim)





### Meger 合并Commits



`merge` 做的事是：

从目标 `commit` 和当前 `commit` （即 `HEAD` 所指向的 `commit`）分叉的位置起，把目标 `commit` 的路径上的所有 `commit` 的内容一并应用到当前 `commit`，然后自动生成一个新的 `commit`。

<p style = "color:red">应用到一个新的commit上</p>



### 放弃解决冲突，取消 merge？

同理，由于现在 Git 仓库处于冲突待解决的中间状态，所以如果你最终决定放弃这次 `merge`，也需要执行一次 `merge --abort` 来手动取消它：

```shell
git merge --abort
```





### 第七节小结

## 小结

本节对 `merge` 进行了介绍，内容大概有这么几点：

1. `merge` 的含义：从两个 `commit`「分叉」的位置起，把目标 `commit` 的内容应用到当前 `commit`（`HEAD` 所指向的 `commit`），并生成一个新的 `commit`；
2.  `merge`的适用场景：
   1. 单独开发的 `branch` 用完了以后，合并回原先的 `branch`；
   2. `git pull` 的内部自动操作。
3.  `merge`的三种特殊情况：
   1. 冲突
      1. 原因：当前分支和目标分支修改了同一部分内容，Git 无法确定应该怎样合并；
      2. 应对方法：解决冲突后手动 `commit`。
   2. `HEAD` 领先于目标 `commit`：Git 什么也不做，空操作；
   3. `HEAD` 落后于目标 `commit`：fast-forward。





## 协同开发

```shell
git checkout -b books #新建一个books分支代码
```

```shell
git push origin books #推送到中央仓库
```

```shell
# 明明的电脑：
git pull
git chekcout books
```



* 合并到主分支上

```shell
git checkout master
git pull # merge 之前 pull 一下，让 master 更新到和远程仓库同步
git merge books
```

```shell
git push
git branch -d books
git push origin -d books # 用 -d 参数把远程仓库的 branch 也删了
```



* 通过Pull Request来进行操作



## 2. 一人多任务

除了代码分享的便捷，基于 Feature Branch 的工作流对于一人多任务的工作需求也提供了很好的支持。

安安心心做事不被打扰，做完一件再做下一件自然是很美好的事，但现实往往不能这样。对于程序员来说，一种很常见的情况是，你正在认真写着代码，忽然同事过来跟你说：「内个……你这个功能先放一放吧，我们最新讨论出要做另一个更重要的功能，你来做一下吧。」

其实，虽然这种情况确实有点烦，但如果你是在独立的 `branch` 上做事，切换任务是很简单的。你只要稍微把目前未提交的代码简单收尾一下，然后做一个带有「未完成」标记的提交（例如，在提交信息里标上「TODO」），然后回到 `master` 去创建一个新的 `branch` 就好了。

```shell
git checkout master
git checkout -b new_feature
```





## add 添加的是文件改动，而不是文件名

当修改了文件`1.txt`后，通过`git add 1.txt`，添加到缓存中；

然后再次修改文件后`1.txt`后，通过`git status`会看到两次文件修改记录；



## log -p 查看详细历史

`-p` 是 `--patch` 的缩写，通过 `-p` 参数，你可以看到具体每个 `commit` 的改动细节：

```shell
git log -p
```



## log --stat 查看简要统计

如果你只想大致看一下改动内容，但并不想深入每一行的细节（例如你想回顾一下自己是在哪个 `commit` 中修改了 `games.txt` 文件），那么可以把选项换成 `--stat`。

```shell
git log --stat
```



## show 查看具体的 commit

如果你想看某个具体的 `commit` 的改动内容，可以用 `show`：

### 看当前 commit

直接输入：

```shell
git show
```



### 看任意一个 commit

在 `show` 后面加上这个 `commit` 的引用（`branch` 或 `HEAD` 标记）或它的 `SHA-1` 码：

```shell
git show 5e68b0d8
```



### 看指定 commit 中的指定文件

在 `commit` 的引用或 `SHA-1` 后输入文件名:  相对路径

```shell
git show 5e68b0d8 shopping\list.txt
```



## 看未提交的内容

如果你想看未提交的内容，可以用 `diff`。

### 比对暂存区和上一条提交

使用 `git diff --staged` 可以显示暂存区和上一条提交之间的不同。换句话说，这条指令可以让你看到「如果你立即输入 `git commit`，你将会提交什么」：

```shell
git diff --staged
```

`--staged` 有一个等价的选项叫做 `--cached`。这里所谓的「等价」，是真真正正的等价，它们的意思完全相同。



### 比对工作目录和暂存区

使用 `git diff` （不加选项参数）可以显示工作目录和暂存区之间的不同。换句话说，这条指令可以让你看到「如果你现在把所有文件都 `add`，你会向暂存区中增加哪些内容」：

```shell
git diff
```

### 比对工作目录和上一条提交

使用 `git diff HEAD` 可以显示工作目录和上一条提交之间的不同，它是上面这二者的内容相加。换句话说，这条指令可以让你看到「如果你现在把所有文件都 `add` 然后 `git commit`，你将会提交什么」（不过需要注意，没有被 Git 记录在案的文件（即从来没有被 add 过 的文件，untracked files 并不会显示出来。为什么？因为对 Git 来说它并不存在啊）。

```shell
git diff HEAD
```

可以将`HEAD`替换成`commit sha-1`值



## 第十结小结

这一节介绍了一些查看改动内容的方法，大致有这么几类：

1. 查看历史中的多个`commit`：`log`
   1. 查看详细改动： `git log -p`
   2. 查看大致改动：`git log --stat`
2. 查看具体某个`commit`: `show`
   1. 要看最新 `commit` ，直接输入 `git show` ；要看指定 `commit` ，输入 `git show commit的引用或SHA-1`
   2. 如果还要指定文件，在 `git show` 的最后加上文件名
3. 查看未提交的内容：`diff`
   1. 查看暂存区和上一条 `commit` 的区别：`git diff --staged`（或 `--cached`）
   2. 查看工作目录和暂存区的区别：`git diff` 不加选项参数
   3. 查看工作目录和上一条 `commit` 的区别：`git diff HEAD`





## rebase——在新位置重新提交

`rebase` ，又是一个中国人看不懂的词。这个词的意思，你如果查一下的话：

其实这个翻译还是比较准确的。`rebase` 的意思是，给你的 `commit` 序列重新设置基础点（也就是父 `commit`）。展开来说就是，把你指定的 `commit` 以及它所在的 `commit` 串，以指定的目标 `commit` 为基础，依次重新提交一次。例如下面这个 `merge`：

```shell
git merge branch1
```



如果把 `merge` 换成 `rebase`，可以这样操作：

```
git checkout branch1
git rebase master
```



![img](https://user-gold-cdn.xitu.io/2017/11/30/1600abd620a8e28c?imageslim)



可以看出，通过 `rebase`，`5` 和 `6` 两条 `commit`s 把基础点从 `2` 换成了 `4` 。通过这样的方式，就让本来分叉了的提交历史重新回到了一条线。这种「重新设置基础点」的操作，就是 `rebase` 的含义。

另外，在 `rebase` 之后，记得切回 `master` 再 `merge` 一下，把 `master` 移到最新的 `commit`：

```
git checkout master
git merge branch1
```



<p style='color:red'>相当于是变一下branch1的基点从2变成4，相当于在 master 当前基础上重新提交两次</p>



![img](https://user-gold-cdn.xitu.io/2017/12/2/160149e054fe485c?imageslim)

`git rebase` `targetBranch`

为了避免和远端仓库发生冲突，一般不要从 `master` 向其他 `branch` 执行 `rebase` 操作。而如果是 `master` 以外的 `branch` 之间的 `rebase`（比如 `branch1` 和 `branch2` 之间），就不必这么多费一步，直接 `rebase` 就好。



## 第十一节小结

本节介绍的是 `rebase` 指令，它可以改变 `commit` 序列的基础点。它的使用方式很简单：

```
git rebase 目标基础点
```

需要说明的是，`rebase` 是站在需要被 `rebase` 的 `commit` 上进行操作，这点和 `merge` 是不同的。







## 第十二节小结

这一节的内容只有一点：用 `commit --amend` 可以修复当前提交的错误。使用方式：

```
git commit --amend
```

需要注意的有一点：`commit --amend` 并不是直接修改原 `commit` 的内容，而是生成一条新的 `commit`。





### 开启交互式 rebase 过程（修改旧的提交记录📝）

现在再用 `commit --amend` 已经晚了，但可以用 `rebase -i`：

```
git rebase -i HEAD^^
```

> 说明：在 Git 中，有两个「偏移符号」： `^` 和 `~`。
>
> `^` 的用法：在 `commit` 的后面加一个或多个 `^` 号，可以把 `commit` 往回偏移，偏移的数量是 `^` 的数量。例如：`master^` 表示 `master` 指向的 `commit` 之前的那个 `commit`； `HEAD^^` 表示 `HEAD` 所指向的 `commit` 往前数两个 `commit`。
>
> `~` 的用法：在 `commit` 的后面加上 `~` 号和一个数，可以把 `commit` 往回偏移，偏移的数量是 `~` 号后面的数。例如：`HEAD~5` 表示 `HEAD` 指向的 `commit`往前数 5 个 `commit`。

上面这行代码表示，把当前 `commit` （ `HEAD` 所指向的 `commit`） `rebase` 到 `HEAD` 之前 2 个的 `commit` 上：



如果没有 `-i` 参数的话，这种「原地 rebase」相当于空操作，会直接结束。而在加了 `-i` 后，就会跳到一个新的界面：

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fdf5fd04f46d6e?imageslim)

个编辑界面的最顶部，列出了将要「被 rebase」的所有 `commit`s，也就是倒数第二个 `commit` 「增加常见笑声集合」和最新的 `commit`「增加常见哭声集合」。需要注意，这个排列是正序的，旧的 `commit` 会排在上面，新的排在下面。





你需要修改这两行的内容来指定你需要的操作。每个 `commit` 默认的操作都是 `pick` （从图中也可以看出），表示「直接应用这个 `commit`」。所以如果你现在直接退出编辑界面，那么结果仍然是空操作。

但你的目标是修改倒数第二个 `commit`，也就是上面的那个「增加常见笑声集合」，所以你需要把它的操作指令从 `pick` 改成 `edit` 。 `edit` 的意思是「应用这个 commit，然后停下来等待继续修正」。其他的操作指令，在这个界面里都已经列举了出来（下面的 "Commands…" 部分文字），你可以自己研究一下。

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fdf5fd020c87f6?imageslim)

把 `pick` 修改成 `edit` 后，就可以退出编辑界面了：

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fdf5fd007159fa?imageslim)

上图的提示信息说明，`rebase` 过程已经停在了第二个 `commit` 的位置，那么现在你就可以去修改你想修改的内容了。

### 修改写错的 commit

修改完成之后，和上节里的方法一样，用 `commit --amend` 来把修正应用到当前最新的 `commit`：

```
git add 笑声
git commit --amend
```

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fdf5fd04de0d40?imageslim)



### 继续 rebase 过程

在修复完成之后，就可以用 `rebase --continue` 来继续 `rebase` 过程，把后面的 `commit` 直接应用上去。

```
git rebase --continue
```





然后，这次交互式 `rebase` 的过程就完美结束了，你的那个倒数第二个写错的 `commit` 就也被修正了：

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fdf5fd4e7d5257?imageslim)

实质上，交互式 `rebase` 并不是必须应用在「原地 rebase」上来修改写错的 `commit` ，这只不过是它最常见的用法。你同样也可以把它用在分叉的 `commit` 上，不过这个你就可以自己去研究一下了。





## 第十三节小结

这节介绍了交互式 `rebase`，它可以在 `rebase` 开始之前指定一些额外操作。交互式 `rebase` 最常用的场景是修改写错的 `commit`，但也可以用作其他用途。它的大致用法：

1. 使用方式是 `git rebase -i 目标commit`；
2. 在编辑界面中指定需要操作的 `commit`s 以及操作类型；
3. 操作完成之后用 `git rebase --continue` 来继续 `rebase` 过程。





## reset --hard 丢弃最新的提交

写完回头看了看，你觉得「不行这得重新写」。那么你可以用 `reset --hard` 来撤销这条 `commit`。

```shell
git reset --hard HEAD^
```

`HEAD^` 表示你要恢复到哪个 `commit`。因为你要撤销最新的一个 `commit`，所以你需要恢复到它的父 `commit` ，也就是 `HEAD^`。那么在这行之后，你的最新一条就被撤销了：



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe19c8a3235853?imageslim)



不过，就像图上显示的，你被撤销的那条提交并没有消失，只是你不再用到它了。如果你在撤销它之前记下了它的 `SHA-1` 码，那么你还可以通过 `SHA-1` 来找到他它。

## 第十四节小结

这一节的内容是撤销最新的提交，方式是通过 `reset --hard`：

```shell
git reset --hard 目标commit;
git reset --hard 6f7e2d924269846073dc3db5d7d8d4047424592b
```





# 想丢弃的也不是最新的提交？

这种情况的撤销，就要用之前介绍过的一个指令：交互式 `rebase` ——`rebase -i`。

## 用交互式 rebase 撤销提交

之前介绍过，交互式 `rebase` 可以用来修改某些旧的 `commit`s。其实除了修改提交，它还可以用于撤销提交。

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe243fc74f48c7?imageslim)

你想撤销倒数第二条 `commit`，那么可以使用 `rebase -i`：

```shell
git rebase -i HEAD^^
```

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe243fc7ac1154?imageslim)

然后就会跳到 `commit` 序列的编辑界面，这个在之前的第 10 节里已经讲过了。和第 10 节一样，你需要修改这个序列来进行操作。不过不同的是，之前讲的修正 `commit` 的方法是把要修改的 `commit` 左边的 `pick` 改成 `edit`，而如果你要撤销某个 `commit` ，做法就更加简单粗暴一点：直接删掉这一行就好。

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe243fcf5f6607?imageslim)

`pick` 的直接意思是「选取」，在这个界面的意思就是应用这个 `commit`。而如果把这一行删掉，那就相当于在 `rebase` 的过程中跳过了这个 `commit`，从而也就把这个 `commit` 撤销掉了。



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe243fce5804fd?imageslim)



现在再看 `log`，就会发现之前的倒数第二条 `commit` 已经不在了。

```shell
git log
```





## 用 rebase --onto 撤销提交

除了用交互式 `rebase` ，你还可以用 `rebase --onto` 来更简便地撤销提交。

`rebase` 加上 `--onto` 选项之后，可以指定 `rebase` 的「起点」。一般的 `rebase`，告诉 Git 的是「我要把当前 `commit` 以及它之前的 `commit`s 重新提交到目标 `commit` 上去，这其中，`rebase` 的「起点」是自动判定的：选取当前 `commit` 和目标 `commit` 在历史上的交叉点作为起点。

例如下面这种情况：



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe24400508e3c8?imageslim)



如果在这里执行：

```shell
git rebase 第3个commit
```

那么 Git 会自动选取 `3` 和 `5` 的历史交叉点 `2` 作为 `rebase` 的起点，依次将 `4` 和 `5` 重新提交到 `3` 的路径上去。

而 `--onto` 参数，就可以额外给 rebase 指定它的起点。例如同样以上图为例，如果我只想把 `5` 提交到 `3` 上，不想附带上 `4`，那么我可以执行：

```shell
git rebase --onto 第3个commit 第4个commit branch1
```

`--onto` 参数后面有三个附加参数：目标 `commit`、起点 `commit`（注意：rebase 的时候会把起点排除在外）、终点 `commit`。所以上面这行指令就会从 `4` 往下数，拿到 `branch1` 所指向的 `5`，然后把 `5` 重新提交到 `3` 上去。



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe24400d7d73d0?imageslim)



同样的，你也可以用 `rebase --onto` 来撤销提交：

```shell
git rebase --onto HEAD^^ HEAD^ branch1
```

上面这行代码的意思是：以倒数第二个 `commit` 为起点（起点不包含在 `rebase` 序列里哟），`branch1` 为终点，`rebase` 到倒数第三个 `commit` 上。



##第十五节小结

这节的内容是「撤销过往的提交」。方法有两种：

1. 用 `git rebase -i` 在编辑界面中删除想撤销的 `commit`s
2. 用 `git rebase --onto` 在 rebase 命令中直接剔除想撤销的 `commit`s

方法有两种，理念是一样的：在 `rebase` 的过程中去掉想撤销的 `commit`，让他它消失在历史中。



通过git reset可以回到删除的commit上面去，但是紧随其后的commit sha-1值就会找不到；

9cf483aab015fd3d39957601e4568e39782012c3 第一次提交

5140b42fb43bb8111258ad9db792b9c5429113af 第二次提交

```shell
#删除第一次提交
git rebase -i HEAD^^

#可以找回第一次提交
git reset --head 9cf483aab015fd3d39957601e4568e39782012c3

#可以找回两次提交
git reset --head 5140b42fb43bb8111258ad9db792b9c5429113af

```



## 1. 出错的内容在你自己的 branch

假如是某个你自己独立开发的 `branch` 出错了，不会影响到其他人，那没关系用前面几节讲的方法把写错的 `commit` 修改或者删除掉，然后再 `push` 上去就好了。不过……

![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe2638ac5c1dd0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

由于你在本地对已有的 `commit` 做了修改，这时你再 `push` 就会失败，因为中央仓库包含本地没有的 `commit`s。但这个和前面讲过的情况不同，这次的冲突不是因为同事 `push` 了新的提交，而是因为你刻意修改了一些内容，这个冲突是你预料到的，你本来就希望用本地的内容覆盖掉中央仓库的内容。那么这时就不要乖乖听话，按照提示去先 `pull` 一下再 `push` 了，而是要选择「强行」`push`：

```shell
git push origin branch1 -f
```

`-f` 是 `--force` 的缩写，意为「忽略冲突，强制 `push`」。

## 2. 出错的内容已经合并到 master

增加一个新的提交，把之前提交的内容抹掉。

它的用法很简单，你希望撤销哪个 `commit`，就把它填在后面：

```shell
git revert HEAD^
```

上面这行代码就会增加一条新的 `commit`，它的内容和倒数第二个 `commit` 是相反的，从而和倒数第二个 `commit` 相互抵消，达到撤销的效果。

在 `revert` 完成之后，把新的 `commit` 再 `push` 上去，这个 `commit` 的内容就被撤销了。它和前面所介绍的撤销方式相比，最主要的区别是，这次改动只是被「反转」了，并没有在历史中消失掉，你的历史中会存在两条 `commit` ：一个原始 `commit` ，一个对它的反转 `commit`。



## 第十六节小结

这节的内容是讲当错误的 `commit` 已经被 `push` 上去时的解决方案。具体的方案有两类：

1. 如果出错内容在私有 `branch`：在本地把内容修正后，强制 `push` (`push -f`）一次就可以解决；
2. 如果出错内容在 `master`：不要强制 `push`，而要用 `revert` 把写错的 `commit` 撤销。





## reset 的本质：移动 HEAD 以及它所指向的 branch

实质上，`reset` 这个指令虽然可以用来撤销 `commit` ，但它的实质行为并不是撤销，而是移动 `HEAD` ，并且「捎带」上 `HEAD` 所指向的 `branch`（如果有的话）。也就是说，`reset` 这个指令的行为其实和它的字面意思 "reset"（重置）十分相符：它是用来重置 `HEAD` 以及它所指向的 `branch` 的位置的。

而 `reset --hard HEAD^` 之所以起到了撤销 `commit` 的效果，是因为它把 `HEAD` 和它所指向的 `branch` 一起移动到了当前 `commit` 的父 `commit` 上，从而起到了「撤销」的效果：



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe19c8a3235853?imageslim)



> Git 的历史只能往回看，不能向未来看，所以把 `HEAD` 和 `branch` 往回移动，就能起到撤回 `commit` 的效果。

所以同理，`reset --hard` 不仅可以撤销提交，还可以用来把 `HEAD` 和 `branch` 移动到其他的任何地方。

```
git reset --hard branch2
```



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe333cb605b0de?imageslim)



不过……`reset` 后面总是跟着的那个 `--hard` 是什么意思呢？

`reset` 指令可以重置 `HEAD` 和 `branch` 的位置，不过在重置它们的同时，对工作目录可以选择不同的操作，而对工作目录的操作的不同，就是通过 `reset` 后面跟的参数来确定的。

<a style='color:red'>解释：为什么reset到第一次提交后,又重新通过一次reset到了第二次的之后,第一次的commit仍能看到 - 本质在于commit的链路没有改变</a>



## reset --hard：重置工作目录

`reset --hard` 会在重置 `HEAD` 和 `branch` 的同时，***重置工作目录里的内容***。当你在 `reset` 后面加了 `--hard` 参数时，你的工作目录里的内容会被完全重置为和 `HEAD` 的新位置相同的内容。换句话说，就是你的未提交的修改会被全部擦掉。



## reset --soft：保留工作目录

`reset --soft` 会在重置 `HEAD` 和 `branch` 时，保留工作目录和暂存区中的内容，并把重置 `HEAD` 所带来的新的差异放进暂存区。

什么是「重置 `HEAD` 所带来的新的差异」？就是这里：



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe333cb5c6a249?imageslim)



由于 `HEAD` 从 `4` 移动到了 `3`，而且在 `reset` 的过程中工作目录的内容没有被清理掉，所以 `4` 中的改动在 `reset` 后就也成了工作目录新增的「工作目录和 `HEAD` 的差异」。这就是上面一段中所说的「重置 `HEAD` 所带来的差异」。

<a style='color:red;font-size:40px'>|--hard|    in contrast    |--soft|</a>



## reset 不加参数：保留工作目录，并清空暂存区

`reset` 如果不加参数，那么默认使用 `--mixed` 参数。它的行为是：保留工作目录，并且清空暂存区。也就是说，工作目录的修改、暂存区的内容以及由 `reset` 所导致的新的文件差异，都会被放进工作目录。简而言之，就是「把所有差异都混合（mixed）放在工作目录中」。



## 第十七节小结

本节内容讲了 `reset` 指令的本质：重置 `HEAD` 以及它所指向的 `branch` 的位置。同时，介绍了 `reset` 的三种参数：

1. `--hard`：重置位置的同时，清空工作目录的所有改动；
2. `--soft`：重置位置的同时，保留工作目录和暂存区的内容，并把重置 `HEAD` 的位置所导致的新的文件差异放进暂存区。
3. `--mixed`（默认）：重置位置的同时，保留工作目录的内容，并清空暂存区。

除了上面这三种参数，还有一些没有列出的较为不常用的参数；另外除了我讲的功能外，`reset` 其实也还有一些别的功能和用法。不过 `reset` 最关键的功能、用法和本质原理就是上面这些了，想了解更多的话，可以去官网了解一下。





# checkout 的本质

`checkout` 并不止可以切换 `branch`。`checkout` 本质上的功能其实是：签出（ checkout ）指定的 `commit`。



## checkout 和 reset 的不同

`checkout` 和 `reset` 都可以切换 `HEAD` 的位置，它们除了有许多细节的差异外，最大的区别在于：`reset` 在移动 `HEAD` 时会带着它所指向的 `branch` 一起移动，而 `checkout` 不会。当你用 `checkout` 指向其他地方的时候，`HEAD` 和 它所指向的 `branch` 就自动脱离了。

事实上，`checkout` 有一个专门用来只让 `HEAD` 和 `branch` 脱离而不移动 `HEAD` 的用法：

```
git checkout --detach
```

执行这行代码，Git 就会把 `HEAD` 和 `branch` 脱离，直接指向当前 `commit`：



![git checkout --detach](https://user-gold-cdn.xitu.io/2017/11/30/1600acce7b90b009?imageslim)git checkout --detach





# Stash

> 注意：没有被 track 的文件（即从来没有被 add 过的文件不会被 stash 起来，因为 Git 会忽略它们。如果想把这些文件也一起 stash，可以加上 `-u` 参数，它是 `--include-untracked` 的简写。就像这样：

```
git stash -u
```





## reflog ：引用的 log

`reflog` 是 "reference log" 的缩写，使用它可以查看 Git 仓库中的引用的移动记录。如果不指定引用，它会显示 `HEAD` 的移动记录。假如你误删了 `branch1` 这个 `branch`，那么你可以查看一下 `HEAD` 的移动历史：

```shell
git reflog
```



![img](https://user-gold-cdn.xitu.io/2017/11/22/15fe3de05468c613?imageslim)



从图中可以看出，`HEAD` 的最后一次移动行为是「从 `branch1` 移动到 `master`」。而在这之后，`branch1` 就被删除了。所以它之前的那个 `commit` 就是 `branch1` 被删除之前的位置了，也就是第二行的 `c08de9a`。

所以现在就可以切换回 `c08de9a`，然后重新创建 `branch1` ：

```shell
git checkout c08de9a
git checkout -b branch1
```

这样，你刚删除的 `branch1` 就找回来了。

> 注意：不再被引用直接或间接指向的 `commit`s 会在一定时间后被 Git 回收，所以使用 `reflog` 来找回删除的 `branch` 的操作一定要及时，不然有可能会由于 `commit` 被回收而再也找不回来。



## 查看其他引用的 reflog

`reflog` 默认查看 `HEAD` 的移动历史，除此之外，也可以手动加上名称来查看其他引用的移动历史，例如某个 `branch`：

```shell
git reflog master
```

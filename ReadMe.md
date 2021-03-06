Git 学习笔记

Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.

Creating a new branch is quick.

Creating a new branch is quick AND simple && simple.


no-ff!!!


#### 在日常工作中，经常会用到Git操作。但是对于新人来讲，刚上来对Git很陌生，操作起来也很懵逼。本篇文章主要针对刚开始接触Git的新人，理解Git的基本原理，掌握常用的一些命令。

## 1.Git工作流程
![](https://cos.whatled.com/img/20190109142341.png)

以上包括一些简单而常用的命令，但是先不关心这些，先来了解下面这4个专有名词。


Workspace：工作区

Index / Stage：暂存区

Repository：仓库区（或本地仓库）

Remote：远程仓库

工作区



程序员进行开发改动的地方，是你当前看到的，也是最新的。平常我们开发就是拷贝远程仓库中的一个分支，基于该分支进行开发。在开发过程中就是对工作区的操作。



暂存区



.git目录下的index文件, 暂存区会记录git add添加文件的相关信息(文件名、大小、timestamp...)，不保存文件实体, 通过id指向每个文件实体。可以使用git status查看暂存区的状态。暂存区标记了你当前工作区中，哪些内容是被git管理的。



当你完成某个需求或功能后需要提交到远程仓库，那么第一步就是通过git add先提交到暂存区，被git管理。



本地仓库



保存了对象被提交 过的各个版本，比起工作区和暂存区的内容，它要更旧一些。git commit后同步index的目录树到本地仓库，方便从下一步通过git push同步本地仓库与远程仓库的同步。



远程仓库



远程仓库的内容可能被分布在多个地点的处于协作关系的本地仓库修改，因此它可能与本地仓库同步，也可能不同步，但是它的内容是最旧的。

小结


任何对象都是在工作区中诞生和被修改；

任何修改都是从进入index区才开始被版本控制；

只有把修改提交到本地仓库，该修改才能在仓库中留下痕迹；

与协作者分享本地的修改，可以把它们push到远程仓库来共享。

![](https://cos.whatled.com/img/20190109142513.png)

## 2.常用Git命令
![](https://cos.whatled.com/img/20190109142728.png)

HEAD
![](https://cos.whatled.com/img/20190109142812.png)

在掌握具体命令前，先理解下HEAD。HEAD，它始终指向当前所处分支的最新的提交点。你所处的分支变化了，或者产生了新的提交点，HEAD就会跟着改变。

add
![](https://cos.whatled.com/img/20190109142843.png)


add相关命令很简单，主要实现将工作区修改的内容提交到暂存区，交由git管理。

git add .	添加当前目录的所有文件到暂存区
git add <dir>	添加指定目录到暂存区，包括子目录
git add <file1>	添加指定文件到暂存区


branch

![](https://cos.whatled.com/img/20190109155201.png)

涉及到协作，自然会涉及到分支，关于分支，大概有展示分支，切换分支，创建分支，删除分支这四种操作。



git branch	列出所有本地分支
git branch -r	列出所有远程分支
git branch -a	列出所有本地分支和远程分支
git branch <branch-name>	新建一个分支，但依然停留在当前分支
git checkout -b <branch-name>	新建一个分支，并切换到该分支
git branch --track <branch><remote-branch>	新建一个分支，与指定的远程分支建立追踪关系
git checkout <branch-name>	切换到指定分支，并更新工作区
git branch -d <branch-name>	删除分支
git push origin --delete <branch-name>	删除远程分支


关于分支的操作虽然比较多，但都比较简单好记。



merge

![](https://cos.whatled.com/img/20190109155230.png)


merge命令把不同的分支合并起来。如上图，在实际开放中，我们可能从master分支中切出一个分支，然后进行开发完成需求，中间经过R3,R4,R5的commit记录，最后开发完成需要合入master中，这便用到了merge。



git fetch <remote>	merge之前先拉一下远程仓库最新代码
git merge <branch>	合并指定分支到当前分支


一般在merge之后，会出现conflict，需要针对冲突情况，手动解除冲突。主要是因为两个用户修改了同一文件的同一块区域。如下图所示，需要手动解除。


![](https://cos.whatled.com/img/20190109155302.png)



rebase


![](https://cos.whatled.com/img/20190109155339.png)



rebase又称为衍合，是合并的另外一种选择。

在开始阶段，我们处于new分支上，执行git rebase dev，那么new分支上新的commit都在master分支上重演一遍，最后checkout切换回到new分支。这一点与merge是一样的，合并前后所处的分支并没有改变。git rebase dev，通俗的解释就是new分支想站在dev的肩膀上继续下去。rebase也需要手动解决冲突。



rebase与merge的区别



现在我们有这样的两个分支,test和master，提交如下：
```
      D---E test
     /
A---B---C---F master
```

在master执行git merge test,然后会得到如下结果：
```
      D--------E
     /          \
A---B---C---F----G
```
test, master

在master执行git rebase test，然后得到如下结果：

A---B---D---E---C'---F'   test, master
可以看到，merge操作会生成一个新的节点，之前的提交分开显示。而rebase操作不会生成新的节点，是将两个分支融合成一个线性的提交。

如果你想要一个干净的，没有merge commit的线性历史树，那么你应该选择git rebase
如果你想保留完整的历史记录，并且想要避免重写commit history的风险，你应该选择使用git merge



reset

![](https://cos.whatled.com/img/20190109155410.png)

reset命令把当前分支指向另一个位置，并且相应的变动工作区和暂存区。



git reset —soft <commit>	只改变提交点，暂存区和工作目录的内容都不改变
git reset —mixed <commit>	改变提交点，同时改变暂存区的内容
git reset —hard <commit>	暂存区、工作区的内容都会被修改到与提交点完全一致的状态
git reset --hard HEAD	让工作区回到上次提交时的状态

revert

![](https://cos.whatled.com/img/20190109155448.png)

git revert用一个新提交来消除一个历史提交所做的任何修改。



revert与reset的区别

![](https://cos.whatled.com/img/20190109155514.png)

git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。



在回滚这一操作上看，效果差不多。但是在日后继续merge以前的老版本时有区别。因为git revert是用一次逆向的commit“中和”之前的提交，因此日后合并老的branch时，导致这部分改变不会再次出现，减少冲突。但是git reset是之间把某些commit在某个branch上删除，因而和老的branch再次merge时，这些被回滚的commit应该还会被引入，产生很多冲突。

git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要被revert的内容。


push


上传本地仓库分支到远程仓库分支，实现同步。



git push <remote><branch>	上传本地指定分支到远程仓库
git push <remote> --force	强行推送当前分支到远程仓库，即使有冲突
git push <remote> --all	推送所有分支到远程仓库

其他命令


git status	显示有变更的文件
git log	显示当前分支的版本历史
git diff	显示暂存区和工作区的差异
git diff HEAD	显示工作区与当前分支最新commit之间的差异
git cherry-pick <commit>	选择一个commit，合并进当前分支
以上就是关于Git的一些常用命令及详细阐述，相信能对Git有一个初步的认识。

#### 来自 https://mp.weixin.qq.com/s/baCSBOg30uQfIPXSDQkAuQ

这里修改一下，rebase到master。

好像有点rebase的感觉了。

再修改一下。

实验结果好像跟下面的不太一致。
rebase


![](https://cos.whatled.com/img/20190109155339.png)



rebase又称为衍合，是合并的另外一种选择。

在开始阶段，我们处于new分支上，执行git rebase dev，
那么new分支上新的commit都在master分支上重演一遍，
最后checkout切换回到new分支。这一点与merge是一样的，
合并前后所处的分支并没有改变。
git rebase dev，
通俗的解释就是new分支想站在dev的肩膀上继续下去。
rebase也需要手动解决冲突。

=========================================
我现在在dev分支那么。
在开始阶段，我们处于dev分支上，执行git rebase master，
那么dev分支上新的commit都在master分支上重演一遍，
最后checkout切换回到dev分支。这一点与merge是一样的，
合并前后所处的分支并没有改变。
git rebase master，
通俗的解释就是dev分支想站在master的肩膀上继续下去。
rebase也需要手动解决冲突。

实验结果好像跟下面的不太一致。
rebase


![](https://cos.whatled.com/img/20190109155339.png)



rebase又称为衍合，是合并的另外一种选择。

在开始阶段，我们处于new分支上，执行git rebase dev，
那么new分支上新的commit都在master分支上重演一遍，
最后checkout切换回到new分支。这一点与merge是一样的，
合并前后所处的分支并没有改变。
git rebase dev，
通俗的解释就是new分支想站在dev的肩膀上继续下去。
rebase也需要手动解决冲突。

=========================================
我现在在dev分支那么。
在开始阶段，我们处于dev分支上，执行git rebase master，
那么dev分支上新的commit都在master分支上重演一遍，
最后checkout切换回到dev分支。这一点与merge是一样的，
合并前后所处的分支并没有改变。
git rebase master，
通俗的解释就是dev分支想站在master的肩膀上继续下去。
rebase也需要手动解决冲突。

=====================================
idea 操作步骤
还是有点奇怪。

==================================
新建了new分支。
在开始阶段，我们处于new分支上，执行git rebase master，
那么new分支上新的commit都在master分支上重演一遍，
最后checkout切换回到new分支。这一点与merge是一样的，
合并前后所处的分支并没有改变。
git rebase dev，
通俗的解释就是new分支想站在dev的肩膀上继续下去。
rebase也需要手动解决冲突。

==========================
再试试。
==========================

线条开始变直了。
线条开始变直了。
===================

先在new分支提交。
切换到master
然后在idea执行git rebase
from new
onto master

![](https://cos.whatled.com/img/20190109171718.png)

new和master已经ok了。
rebase了后合并。


### 优化
![](https://cos.whatled.com/img/20190109172144.png)

### 调整
不知道这个流程是否ok

测试idea git rebase流程。new分支
#### 这个流程存在问题。本来只修改很小的部分。

######## 测试

### 测试new分支

### 测试new分支+1

### 测试new分支+2

### 测试new分支+3

### 测试new分支+1

使用git版本管理工具，必问git rebase的用法，但之前项目组人数5人，一直使用的是git pull，git commit 和git push，几乎没有用git rebase来变基，减少难看的merge 类型的commit。 
最近一个新项目，两地合作办公大概有10人共用一个项目分支，几分钟内可能有多人提交，造成连续多个merge在gitk的路径中，看不清某个人某次有价值的提交是哪一条，故组长建议大家使用git rebase。

```bash
git stash
git pull —rebase
git stash pop
手动解决冲突
git add -u
git rebase —continue
如果此时提示No rebase in progress?则表示已经没有冲突了；否则上面两步要重复多次
git commit -m “xxx”
git push origin [branch] -f
```

[精华部分]使用下面的关系区别这两个操作： 
git pull = git fetch + git merge 
git pull –rebase = git fetch + git rebase


![](https://cos.whatled.com/img/20190109181943.png)
git pull –rebase 理解 
这个命令做了以下内容： 
a.把你 commit 到本地仓库的内容，取出来放到暂存区(stash)（这时你的工作区是干净的） 
b.然后从远端拉取代码到本地，由于工作区是干净的，所以不会有冲突 
c.从暂存区把你之前提交的内容取出来，跟拉下来的代码合并 
所以 rebase 在拉代码前要确保你本地工作区是干净的，如果你本地修改的内容没完全 commit 或者 stash，就会 rebase 失败。

还附上了git add -u的解释：
git add 的几种参数区别： 
git add -A 保存所有的修改 
git add . 保存新的添加和修改，但是不包括删除 
git add -u 保存修改和删除，但是不包括新建文件。 
如果只想提交某个文件，可以使用git add 路径/文件名 或者 git add 路径/

这一篇解释了手动解决冲突时<<<<和=====的含义 
一般情况下rebase都是会有冲突的，详细查看冲突可以用命令git status然后就会显示哪个文件有冲突，然后打开有冲突的哪个文件，会发现有一些“<<<<<<<”， “=======”， “>>>>>>>” 这样的符号。


“<<<<<<<” 表示冲突代码开始

“=======” 表示test与master冲突代码分隔符

“>>>>>>>" 表示冲突代码的结束

<<<<<<<  
所以这一块区域test的代码

=======  
这一块区域master的代码

>>>>>>> 

对于rebase的工作流以代码的形式也体现的很清楚：

git rebase 
while(存在冲突) {
    git status
    找到当前冲突文件，编辑解决冲突
    git add -u
    git rebase --continue
    if( git rebase --abort )
        break; 
}


### 简化版
1.在new分支，提交干净。

2.git rebase master

发生冲突
while(存在冲突) {
    git status
    找到当前冲突文件，编辑解决冲突
    git add -u
    git rebase --continue
    if( git rebase --abort )
        break; 
}
rebase的时候，修改冲突后的提交不是使用commit命令，
而是执行rebase命令指定 --continue选项。
若要取消rebase，指定 --abort选项。

附上了git add -u的解释：
git add 的几种参数区别： 
git add -A 保存所有的修改 
git add . 保存新的添加和修改，但是不包括删除 
git add -u 保存修改和删除，但是不包括新建文件。 
如果只想提交某个文件，可以使用git add 路径/文件名 或者 git add 路径/

3. git checkout master
git merge new

4. git checkout new
git merge master

idea git rebase操作步骤
1.在new分支，提交干净。
2.选中项目右键->Git->Repository->Rebase
![](https://cos.whatled.com/img/20190109190117.png)

![](https://cos.whatled.com/img/20190109190252.png)
Onto为需要rebase的分支，比如master

使用命令的形式为:
    git rebase   [startpoint]   [endpoint]  --onto  [branchName]

其中，[startpoint]  [endpoint]仍然和上一个命令一样指定了一个编辑区间(前开后闭)，
--onto的意思是要将该指定的提交复制到哪个分支上。


发生冲突
while(存在冲突) {
    git status
    找到当前冲突文件，编辑解决冲突
    git add -u
    git rebase --continue
    if( git rebase --abort )
        break; 
}

3. git checkout master
git merge new

4. git checkout new
git merge master


git merge master

#### 上面操作有点问题，先理解下git pull --rebaes
git pull的作用是将远程库中的更改代码合并到当前分支中，默认为：git fetch + git merge

git fetch 的作用就相当于是从远程库中获取最新版本到本地分支，不会自动进行git merge

git pull –rebase 加上–rebase参数的原因是，在多人开发中，有多个merge commit，如果不加该参数，则有多个历史提交线，
而它的作用，就相当于把分叉的提交线中的一条，每一次提交都捡选出来， 在另一条提交线上提交。最后也形成一条单一的提交线。

#### 那么怎么用idea提交git pull --rebase呢？

1.先在new分支提交干净。
2.new分支rebase到master
3.master分支rebase到master
4.master分支合并new分支。
5.new分支合并master分支。

#### 多人开发 new分支。

#### 模拟多人开发。

五线谱了。。

突然想起廖雪峰的教程

**https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0015266568413773c73cdc8b4ab4f9aa9be10ef3078be3f000**

多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。

每次合并再push后，分支变成了这样：

```
$ git log --graph --pretty=oneline --abbrev-commit
* d1be385 (HEAD -> master, origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
| |/  
* |   12a631b merged bug fix 101
|\ \  
| * | 4c805e2 fix bug 101
|/ /  
* |   e1e9c68 merge with no-ff
|\ \  
| |/  
| * f52c633 add merge
|/  
*   cf810e4 conflict fixed
```

总之看上去很乱，有强迫症的童鞋会问：为什么Git的提交历史不能是一条干净的直线？

其实是可以做到的！

Git有一种称为rebase的操作，有人把它翻译成“变基”。

![rebase](https://cdn.liaoxuefeng.com/cdn/files/attachments/001526658989813e7aa16d765bc4ef29e5a99167c10c2e7000/l)

先不要随意展开想象。我们还是从实际问题出发，看看怎么把分叉的提交变成直线。

在和远程分支同步后，我们对`hello.py`这个文件做了两次提交。用`git log`命令看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 582d922 (HEAD -> master) add author
* 8875536 add comment
* d1be385 (origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
...
```

注意到Git用`(HEAD -> master)`和`(origin/master)`标识出当前分支的HEAD和远程origin的位置分别是`582d922 add author`和`d1be385 init hello`，本地分支比远程分支快两个提交。

现在我们尝试推送本地分支：

```
$ git push origin master
To github.com:michaelliao/learngit.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

很不幸，失败了，这说明有人先于我们推送了远程分支。按照经验，先pull一下：

```
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:michaelliao/learngit
   d1be385..f005ed4  master     -> origin/master
 * [new tag]         v1.0       -> v1.0
Auto-merging hello.py
Merge made by the 'recursive' strategy.
 hello.py | 1 +
 1 file changed, 1 insertion(+)
```

再用`git status`看看状态：

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

加上刚才合并的提交，现在我们本地分支比远程分支超前3个提交。

用`git log`看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
|\  
| * f005ed4 (origin/master) set exit=1
* | 582d922 add author
* | 8875536 add comment
|/  
* d1be385 init hello
...
```

对强迫症童鞋来说，现在事情有点不对头，提交历史分叉了。如果现在把本地分支push到远程，有没有问题？

有！

什么问题？

不好看！

有没有解决方法？

有！

这个时候，rebase就派上了用场。我们输入命令`git rebase`试试：

```
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: add comment
Using index info to reconstruct a base tree...
M    hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
Applying: add author
Using index info to reconstruct a base tree...
M    hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
```

输出了一大堆操作，到底是啥效果？再用`git log`看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master) add author
* 3611cfe add comment
* f005ed4 (origin/master) set exit=1
* d1be385 init hello
...
```

原本分叉的提交现在变成一条直线了！这种神奇的操作是怎么实现的？其实原理非常简单。我们注意观察，发现Git把我们本地的提交“挪动”了位置，放到了`f005ed4 (origin/master) set exit=1`之后，这样，整个提交历史就成了一条直线。rebase操作前后，最终的提交内容是一致的，但是，我们本地的commit修改内容已经变化了，它们的修改不再基于`d1be385 init hello`，而是基于`f005ed4 (origin/master) set exit=1`，但最后的提交`7e61ed4`内容是一致的。

这就是rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

最后，通过push操作把本地分支推送到远程：

```
Mac:~/learngit michael$ git push origin master
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 576 bytes | 576.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To github.com:michaelliao/learngit.git
   f005ed4..7e61ed4  master -> master
```

再用`git log`看看效果：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master, origin/master) add author
* 3611cfe add comment
* f005ed4 set exit=1
* d1be385 init hello
...
```

远程分支的提交历史也是一条直线。

### 小结

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。


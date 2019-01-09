Git 学习笔记

Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.

Creating a new branch is quick.

Creating a new branch is quick AND simple && simple.


no-ff!!!

#### 来自 https://mp.weixin.qq.com/s/baCSBOg30uQfIPXSDQkAuQ

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


# git rebase详解
#### git合并代码方式主要有两种方式，分别为：
#### 1、merge处理，这是大家比较能理解的方式。
#### 2、rebase处理，中文此处翻译为衍合过程。

### git rebase操作讲解例子：
```bash
cd /usr/local/test
mkdir hellogit
cd hellogit # 创建hellogit目录
git init # 初始化git项目
vim readme # 新建readme文件，往里边添加内容
git add . # 提交内容
git commit -m 'init project c1' # git系统默认创建一个master分支

# 接着我们创建一个dev分支，在dev分支上添加内容
git checkout -b dev # 此处其实是两步git branch dev加上git checkout dev
vim readme # 在原来基础上增加上内容
git add .
git commit -m 'add hello world c2'

# 切换回到master分支
git checkout master
vim readme # 编辑readme文件，在第二行增加hello world from master内容
# 此处先埋个点，因为此处会和dev分支上做的修改冲突
git add .
git commit -m 'add hello world c3'
vim hello.py # 新添加一个hello.py文件
git add .
git commit -m 'add hello.py c4'

# 切换回到dev分支
git checkout dev
vim helloworld.py # 添加上helloworld.py文件
git add .
git commit -m 'add helloworld.py c5'
```

### rebase前
![](https://cos.whatled.com/img/20190109135644.png)

### 切换到master分支

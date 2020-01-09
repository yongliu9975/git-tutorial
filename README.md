## 1. Git基础

基础部分比如`git clone`, `git add`, `git ignore`, `git commit`, `git push`等直接跳过

## 2. Git分支

### 创建自己的分支

使用以下命令查看当前所在分支：

```shell
$ git branch
```

这个时候应该可以看到：

```
* master
```

然后创建一个叫做`testing`的新分支：

```shell
$ git branch testing
```

再看一下当前分支可以看到多了一个自己创建的`testing`分支:

```
* master
  testing
```

使用以下命令切换分支：

```shell
$ git checkout testing
```

然后再使用命令`git branch`查看分支就可以看到当前分支已经切换了：

```
  master
* testing
```

如果你不想要当前这个`testing`分支了，可以在切换回`master`后直接删除这个分支：

```shell
$ git checkout master
$ git branch -d testing
```

使用以下命令查看提交历史：

```shell
$ git log
```

Note：

* 开发的时候要基于哪个版本去开发新的分支，就要在哪个分支上创建新的分支，比如我当前正在`testing`分支上开发，但是要基于`master`分支去开发另一个分支`iss53`, 那这个时候就要先切换回`master`分支，再创建新的分支，操作如下：

  ```shell
  $ git checkout master
  $ git branch issue53
  $ git checkout issue53
  ```

* 建议新建自己的分支并在分支上开发，可以以`master`分支为基础搞多个分支，不建议搞多个文件夹那样去开发

### 合并分支

假如你当前正在`hotfix`分支上开发，开发情况可以如下图所示：

![pic1](https://git-scm.com/book/en/v2/images/basic-branching-4.png)



经过测试后觉得没问题，想要将`hotfix`和`master`分支合并，那么这时候可以使用`git merge`来实现：

```shell
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

这个时候分支情况就变成了如下：

![pic2](https://git-scm.com/book/en/v2/images/basic-branching-5.png)

在这个时候，如果你想要进一步将`iss53`分支和master分支合并，可以执行：

```shell
$ git checkout master
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

现在分支情况如下：

![pic3](https://git-scm.com/book/en/v2/images/basic-merging-2.png)



但是实际情况中很有可能会出现合并冲突的情况，比如你和另一个人都修改了某个文件的某一行，这时候就会产生冲突而无法合并，类似于下面这样（以同时修改了`readme.txt`为例）：

```shell
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

信息显示在合并`readme.txt`的时候有冲突，这个时候可以使用`git diff`命令查看一下冲突的具体情况：

```
master
<<<<<<< HEAD 
hello world 1
=======  
hello world 2
>>>>>>> branch2　
```

查看到具体的冲突情况后就可以修改一下自己当前分支冲突位置的内容，改成和`master`分支一样的内容（上面的例子就是把"hello world 2"改回成"hello world 1"），然后再merge，成功合并之后再决定是否把之前冲突的位置修改成自己想修改的样子（上面的例子里自己想修改成的内容就是"hello world 2"）



## 3. Fork整个project开发

### Create Pull Request

在github中，另一种常用的开发形式就是将整个项目fork（类似于直接copy一份）到自己的github账号下面，这样就可以直接随心所欲的开发。开发完成后可以点击`new pull request`将自己开发的版本和原来的仓库进行合并：

![pic4](https://www.earthdatascience.org/images/earth-analytics/git-version-control/github-create-pull-request-screencast.gif)



### Code Review

在提交完pull request之后，仓库的拥有者就会看到pull request的请求，这个时候就可以借助github做code review了：

![pic5](https://www.earthdatascience.org/images/earth-analytics/git-version-control/github-merge-pull-request-screencast.gif)

确认没问题之后就可以同意pull request，如果觉得哪里有问题的话可以直接回复有哪些问题
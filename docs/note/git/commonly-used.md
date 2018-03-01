## git常用命令及技巧

#### git merge --no-ff

默认情况下，如果没有冲突那么 `git merge` 采用 `fast-forward`(快进) 的模式进行合并，所谓 `fast-forward` 指的是：不产生新的提交历史，直接移动 `HEAD` 至要合并的分支，显而易见的缺点是合并历史信息不清晰，如下图(一条线)：

<img src="../../asset/img/git-merge.png" width="200" />

所以为了保留分支的 `commit` 历史记录，我们可以采用 `--no-ff` 选项，这样合并后的历史记录图类似于这样：

<img src="../../asset/img/git-merge-noff.png" width="200" />

#### git merge --squash

`--squash` 选项用于压缩多个“无用”的 `commit` 为一个 `commit`，效果类似下图：

<img src="../../asset/img/git-merge-squash.png" width="200" />

#### fork 的仓库与远程仓库同步

以 `Github` 为例

* 1、设置 `upstream` 为原仓库地址

```sh
git remote add upstream https://github.com/some_project/some_project.git
```

* 2、获取原仓库的更新到本地

```sh
git fetch upstream
```

* 3、合并更新，以 `master` 分支为例

```
git checkout master
git merge upstream/master
```

#### 修改提交历史消息

##### 修改最近一次提交的历史

```sh
git commit --amend -m "New commit message"
```

注意，在执行上面的命令修改提交历史之前，要保证本地没有暂存的修改，否则改修改也会被提交（当你忘记提交一些内容的时候，利用这个特性补充）

##### 修改更早(最近一次提交之前的提交)的提交历史

`git` 没有提供直接修改任意一次提交历史的功能，但是我们可以通过 `rebase` 间接来完成这个任务，我们用一个例子说明。

通过 `git log` 命令查看提交历史，假设如下：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-123440.jpg)

上图中展示了过去三次提交，假设这三次提交的历史消息不正确，需要修改，那么首先你要执行：

```sh
git rebase -i HEAD~2
```

`-i` 选项用来告诉 `git` 我们是在交互式的 `rebase`（变基）。`HEAD~2` 中的 `2` 代表修改最近两次提交（注意：`2` 其实指的是第三次提交，也就是你要修改的最近几次提交的父提交），你当然也可以写 `3`、`4` 随你便。

结果如下图：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-124043.jpg)

可以看到 `git rebase -i HEAD~2` 这条命令把我们带到了文本编辑器。接下来你只要将需要修改的提交前面的 `pick` 改为 `edit` 即可，然后保存退出：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-124910.jpg)

保存退出之后，可以看到如下图内容：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-125114.jpg)

通过上图可以看到，`git` 已经给了我们非常好的提示了，它告诉我们可以使用 `git commit --amend` 修改本次提交历史，并且修改完成之后可以使用 `git rebase --continue` 语句应用该提交并继续执行交互式行为，那么我们就尝试一下执行 `git commit --amend`，结果如下图：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-125454.jpg)

可以看到，把我们带到了编辑器，这时我们就可以修改提交信息了，修改完成之后保存退出，接着执行 `git rebase --continue`，如下图：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-125717.jpg)

可以看到，交互式行为执行到了下一步，因为我们要修改两个提交的提交历史（因为我们把上两次提交的 `pick` 都改成了 `edit`），这时重复以上步骤，即执行 `git commit --amend` 修改历史，完成后执行 `git rebase --continue` 如下图：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-130024.jpg)

由于已经没有可交互内容，那么此时将自动退出交互模式，回到当前分支。现在我们通过 `git log` 命令查看提交历史，可以发现我们的历史提交信息成功修改了：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-01-130258.jpg)

#### 常用团队合作操作规范

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-02-20-113110.jpg)
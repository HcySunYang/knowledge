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

* 修改上一次提交的历史消息

```sh
git commit --amend -m "New commit message"
```

注意，在执行上面的命令修改提交历史之前，要保证本地没有暂存的修改，否则改修改也会被提交。

#### 常用团队合作操作规范

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-02-20-113110.jpg)
---
layout: post
title: 分享我之前画的 Git 操作流程图
datePublished: '2016-09-12 11:21:04'
category: Git
---

![git](https://www.processon.com/chart_image/57d6deace4b0942d7aac7328.png)
这是之前和别人说我的 Git 操作习惯的时候给画的一个一个简图。怎么说呢，Git 是很强大的，有些人用了很久可能Git的操作命令也没全用过，这很正常，而且每个团队的 Git 流程可能也不一样，但是社区有些最佳实践啥的
大家应该都是同意的。

不过我这上面，当时少写了一个代码回滚撤销提交的操作，也是因为这场景用的比较少，所以当时没写上去，这里在写下我对代码回滚的一些操作：

说明：这里我只说 `git reset` 和 `git revert` 的场景，因为 `git checkout <commit log id> ` 只是用来查看之前提交过的旧版本代码，不应该把代码拉下来继续开发。


首先我们要明白回滚或者撤销提交的场景是什么：

1.如果我觉得上一次的提交无意义，不想让他留在提交记录里面，这是侯我会用 `git reset <commit log id>`  (通过 git log 来查看上一次的提交ID) 来把上一次提交删除，当然他还支持其他的一些参数，具体使用看你的需求。
不过 reset 会重写修改提交历史，并不建议在团队的公共分支上使用，除非你不小心提交了不该提交的东西，
可以在自己的私有开发分支使用 reset 干掉它。

2.如果是提交发布后出现问题，需要回滚到前一次正常的代码，这时候我会用 `git revert  <commit log id>` 来回滚代码，revert 会创建一个新的提交来把上一次（具体哪次取决你填的哪个log id）的提交更改撤销，
这样就可以保证你的代码安全了，而且上一次出现错误的提交记录也还在，下次 code review 的时候也可以查看哪里出了问题。

当然 git reset 和 git revert 也都支持文件级别的回滚，还有上面的 ` <commit log id>` 也可以通过 HEAD 这个参数来简化。

最后，对于使用 git 命令的同学，大家应该通过 alias 来设置快捷命令，比如：

```
ggpur='git pull --rebase origin $(current_branch)'
ggpush='git push origin $(current_branch)'

```

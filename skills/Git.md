# Git

[ProGit](https://git-scm.com/book/zh/v2)

## 起步

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统（可以对任何文件进行版本控制）

### 本地版本控制系统

许多人习惯用复制整个项目目录的方式来保存不同的版本，或许还会改名加上备份时间以示区别。 这么做唯一的好处就是简单，但是特别容易犯错。 有时候会混淆所在的工作目录，一不小心会写错文件或者覆盖意想外的文件。

为了解决这个问题，人们很久以前就开发了许多种本地版本控制系统，大多都是采用某种简单的数据库来记录文件的历次更新差异。

![本地版本控制图解](https://git-scm.com/book/en/v2/images/local.png)





# Git

[learn Git Branching](https://learngitbranching.js.org/)

## Introduction to Git Commits(branching, merging, rebasing, etc)

commit: records a snapshot of all files(even better)

branch early, and branch often(branches are pointers to a specific commit)

Git rebase: second way of combining work

hash $\Rightarrow$ relative refs     git branch -f main HEAD~1

git reset / git revert

I want this work here and that work there

git cherry-pick

git rebase -i 

juggling commits



## Git Remotes

You can typically talk to this other computer through the Internet, which allows you to transfer commits back and forth.

git pull  $\rightleftarrows$ git fetch; git merge o/main

The difficulty comes in when the history of the repository diverges.

git fetch $\Rightarrow$ git rebase o/main $\Rightarrow$ git push      $\rightleftarrows$    git pull

git fetch $\Rightarrow$ git merge o/main $\Rightarrow$ git push	   $\rightleftarrows$    git pull --rebase


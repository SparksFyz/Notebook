### Modify last commit message

1. git commit --amend
2. git rebase
3. git push -f + branch

> This method is just for the branch which used by only one person.

#### Git2.0 之前的版本中,git push -f 不加分支名,默认为matching参数.

>‘matching’ 参数是 Git 1.x 的默认行为，其意是如果你执行 git push 但没有指定分支，
它将 push 所有你本地的分支到远程仓库中对应匹配的分支。

>而 Git 2.x 默认的是 simple，意味着执行 git push 没有指定分支时，
只有当前分支会被 push 到你使用 git pull 获取的代码。

> 修改默认参数 : `git config --global push.default simple`
# Git Tips

## 1. Export Git logs

Add a git command, so we do not need to re-type all this options each time. To do this edit **.git/config** file in your repo and add:

```txt
[alias]
    report = rev-list --oneline --pretty=format:'%cd | - %s%d' --graph --date=format:'%Y-%m-%d %H:%M' --all
```

Now you can obtains report just typing:

```txt
git report
```

## 2. 提交代码到多个分支

如果代码需要提交到多个分支，需要使用git cherry-pick功能。以下是一个示例

PS：master为主干分支，dev为开发版本分支。  
假设，我们当前在主干分支master，则：  
1，master分支上git add , git commit，并且记录下commit id  
2，git pull –rebase origin master // 确保master分支是最新状态  
3，git push origin master  
4，git checkout dev切换到dev分支  
5，git cherry-pick 后面加上master分支下的刚刚提交的commit id  
6，git pull –rebase origin dev // 确保dev分支是最新的  
7，git push origin dev

## 3. 追加修改

当push代码之后，review不通过或者想继续修改，可以继续编辑源代码，然后再通过git commit –amend来追加修改

```txt
    $ git add test.txt
    $ git commit –m “add test”
    $ git push origin master
    $ git add test2.txt
    $ git commit --amend
    $ git push origin master
```

## 4. 版本回退

git reset可以实现版本回退

```txt
    git reset --soft HEAD~
```

添加–soft选项，则本地修改仍然存在，commit信息将回退  
HEAD~：表示将HEAD版本向前退一个版本~~表示两个版本，亦可以用HEAD~2表示

## 5. 提交tag

```txt
    git tag -am “commit message” tagname
    git push origin tagname:refs/heads/tagname
    git push --tags
```

## 6. 本地仓库与远程仓库不一致

`Your branch is ahead of 'origin/master' by 1 commits.`

使用git status后，提示Your branch is ahead/behind of ‘origin/master’ by x commits. 表示本地仓库与远程仓库不同步，本地仓库比远程仓库提交x次提交。
出现的原因是有以下两种：

1. 本地提交push到gerrit，但是gerrit没有merge到远程仓库。
1. gerrit上已经merge过了，但是本地没有拉取最新的远程仓库。

解决办法：
先查看gerrit merge状态。然后再通过拉取代码获取最新的分支代码。如果提交的commit是你自己的，可以简单使用git pull，如果不是，强烈推荐`git pull --rebase origin master`来拉取分支。

## 7. Push代码被拒绝

push代码时，出现以下错误：
* `[remote rejected] HEAD -> refs/for/master (missing Change-Id in commit messag`

解决办法
1. 先确认是否有从远程下载commit-msg模板，如果没有，参考下载commit模板一节，下载commit-msg模板
1. 使用git status查看状态，发现ahead of ‘origin/master’的数目不对。则使用git log查看提交记录，发现提交记录中有Merge branch ‘master’ of ssh://…，则可以确定是因为pull代码时，出现了分支冲突，git自动合并并提交合并commit。具体的解决办法之一：  
        a) git reset –soft HEAD~将自动merge后的commit全部重置，并且保存到暂存区  
        b) git reset –hard HEAD~将自动merge的commit还原，再pull –rebase拉取最新的分支代码。并解决冲突。  
        c) 从暂存区恢复代码，并且重新提交即可

* `warning: push.default is unset`  
是由版本兼容性导致的，低版本的git push如果不指定分支名，就会全部推送，而新版只会推送当前分支。  
解决的办法: 我们只需要明确指定应该推送方式即可
1. 全部推送 `git config –global push.default matching`
1. 部分推送 `git config –global push.default simple`


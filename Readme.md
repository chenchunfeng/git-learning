
## git add
git add -u：将文件的修改、文件的删除，添加到暂存区。(整个仓库)
git add .： 将文件的修改，文件的删除，文件的新建，添加到暂存区。(当前路径及其子路径下)
git add -A：将文件的修改，文件的删除，文件的新建，添加到暂存区。(整个仓库)


常用 git add . 

## git log date 

> git log --date=iso

## git reset


有三个参数
|  参数  | 功能 | 场景 |
|  ---   | ---  | --- |
| --soft               | 保留工作区 保留缓存区 | 合并多个commit   |
| --mixed （default）  | 保留工作区 清空缓存区 | 有错误的commit需要修改   |
| --hard   | 清空工作区 清空缓存区 |  放弃目标版本后所有的修改   |

后面还可以跟commit id 默认为HEAD

1.  --soft 使用场景
假设有三个commit
```shell
git commit -m '开始'
git commit -m '提交1'
git commit -m '提交2'
git commit -m '提交3'
git reset --soft 第一次commitId
# 这个时候 提交1 提交2 提交3的代码都回归到暂存区 stage/index 
git commit -m '合并一次提交'
# 此时git log 只剩下 开始 跟 合并一次提交
```


## git mv    rename
```shell
# 直接用命令操作，不用修复文件名，再git add
git mv [oldName] [newName]
```

大小写敏感问题，直接设置大小写敏感
```shell
  git config core.ignorecase true
```
## git log
•	git log --all 查看所有分支的历史
•	git log --all --graph 查看图形化的 log 地址
•	git log --oneline 查看单行的简洁历史。
•	git log --oneline -n4 查看最近的四条简洁历史。
•	git log --oneline --all -n4 --graph 查看所有分支最近 4 条单行的图形化历史。
•	git help --web log 跳转到git log 的帮助文档网页
• git reflog 查看分支所有的操作记录(包括已经删除的等)
• gitk


## git branch 
• git branch 默认本地所有分支
• git branch -r 远端所有分支
• git branch -a 本地和远端所有分支
• 加上v参数== 会显示每个分支最后一个commit eg: git branch -av
• git brach -D [分支名称] 删除不需要的分支


## git checkout

git checkout [分支名] 切换分支
git checkout -b [分支名] 新建分支并切换到该分支
git checkout -f [分支名] -f 参数会强制切换分支，并清除掉 stage 和 work 的全部变动！！

git checkout [commit id]


## gitk 图形化

Author Committer 不一定一样， cherry-pick 后就有区别了



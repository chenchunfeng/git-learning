
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


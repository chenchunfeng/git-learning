
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
• git branch -m 旧分支名称  新分支名称


## git checkout

git checkout [分支名] 切换分支
git checkout -b [分支名] 新建分支并切换到该分支
git checkout -f [分支名] -f 参数会强制切换分支，并清除掉 stage 和 work 的全部变动！！

git checkout [commit id]


## gitk 图形化

Author Committer 不一定一样， cherry-pick 后就有区别了


## .git 目录

mac/linux cat 查看文件  windows type


- git cat-file -t 命令 ， 查看 git 对象的类型
- git cat-file -p 命令， 查看 git 对象的内容
- git cat-file -s 命令， 查看 git 对象的大小


•	COMMIT_EDITMSG
•	config               当前 git 的配置文件
•	description      （仓库的描述信息文件）
•	HEAD             （指向当前所在的分支），例如当前在 develop 分支，实际指向地址是 refs/heads/develop
•	hooks [文件夹]
•	index
•	info [文件夹]
•	logs [文件夹]
•	objects [文件夹]    （存放所有的 git 对象，对象哈希值前 2 位作为文件夹名称，后 38 位作为对象文件名, 可通过 git cat-file -p 命令，拼接文件夹名称+文件名查看）
•	ORIG_HEAD
•	refs [文件夹] 
      •	heads  （存放当前项目的所有分支）
      •	tags      (存放的当前项目的所有标签，又叫做里程碑)


ref 里面heads 里面有对于分支的commit id

使用git cat-file -s commitId  可查看查对应的结构

commit three blob


> objects 存放着所有commit, 为了节省空间，blob不使用文件名来命名，同一个内容的文件其哈希名一样，这样就可以节省很多存储空间


## 小作业

新建一个文件夹，里面有一个文件，请问有几个commit tree blob

- 空间文件夹，不会触发git
- add 后才会生成objects
- 使用路径两个字母加文件名前四位查看 git cat-file -s edd0ab type.txt生成一个blob类型的文件
- commit 后才会生成 commit 类型的object
- commit类型下，记录tree  这个tree指向另一个tree 这个tree最后才指向blob

1 commit 2 tree 1 blob


## 分离头指针注意事项

- git checkout commitId：会出现分离头指针的情况，这种情况下比较危险，因为这个时候你提交的代码没有和分支对应起来，当切换到其他分支的时候(比如master分支)，容易丢失代码；
- 应用场景，就是在自己做尝试或者测试的时候可以分离头指针，当尝试完毕没有用的时候可以随时丢弃
- 但是如果觉得尝试有用，那么可以新建一个分支，使用  git branch \<新分支的名称> commitId

## 进一步理解HEAD和branch

<!-- git checkout -b new_branch [具体分支 或 commit] 创建新分支并切换到新分支
git diff HEAD HEAD~1 比较最近两次提交
git diff HEAD HEAD~2 比较最近和倒数第三次提交
git diff HEAD HEAD^  比较最近两次提交
git diff HEAD HEAD^^ 比较最近和倒数第三次提交 -->

1
2
3
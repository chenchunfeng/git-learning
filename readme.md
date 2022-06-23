
# git 基础
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

- git checkout -b new_branch [具体分支 或 commit] 创建新分支并切换到新分支
- git diff HEAD HEAD~1 比较最近两次提交
- git diff HEAD HEAD~2 比较最近和倒数第三次提交
- git diff HEAD HEAD^  比较最近两次提交
- git diff HEAD HEAD^^ 比较最近和倒数第三次提交


[相关文档](https://stackoverflow.com/questions/2221658/whats-the-difference-between-head-and-head-in-git)

```
G   H   I   J
 \ /     \ /
  D   E   F
   \  |  / \
    \ | /   |
     \|/    |
      B     C
       \   /
        \ /
         A
```

- A = A^0
- B = A^   = A^1     = A~1
- C = A^2
- D = A^^  = A^1^1   = A~2
- E = B^2  = A^^2
- F = B^3  = A^^3
- G = A^^^ = A^1^1^1 = A~3
- H = D^2  = B^^2    = A^^^2  = A~2^2
- I = F^   = B^3^    = A^^3^
- J = F^2  = B^3^2   = A^^3^2


# git 常见场景

## 删除不需要的分支

删除：
`git branch -d branch_name`   可能会提示not fully merged
强制删除：
`git branch -D branch_name`

## 修改最近commit message

> git commit --amend 此修改一般用于未push之前修改commit信息
如果是远程的个人分支也是可以用的，千万别用于公共开发
> git commit --amend
> git push -f

## 修改老旧commit message

这里使用git rebase -i [basePonit] [endPoint]  一般不加endPoint，会丢弃endPoint后面的commit


pick	  p	保留该commit
reword	r	保留该commit，但需要修改该commit的注释
edit	  e	保留该commit, 但我要停下来修改该提交(不仅仅修改注释)
squash	s	将该commit合并到前一个commit
fixup	  f	将该commit合并到前一个commit，但不要保留该提交的注释信息
exec	  x	执行shell命令
drop	  d	丢弃该commit

把需要修改message的commit 前面的类型改成reword


## 连续的多个commit整理成1个

注意到上面的参数
squash 和 fixup 可用到


## 不连续的多个commit整理成1个

容易发生冲突,

> 都不要在公共分支上使用


## 比较暂存区和HEAD所含文件的差异

git diff --cached HEAD
git diff --cached

## 比较工作区和暂存区所含文件的差异

git diff

## 比较工作区和HEAD区所含文件的差异

git diff HEAD

## 暂存区恢复成和HEAD的一样

有三个参数
 --mixed 保留工作区 清除缓存区   default
 --soft  保留工作区 保留缓存区
 --hard  清除工作区 清除缓存区

 git reset

## 工作区的文件恢复为和暂存区一样


 git checkout filename

 已暂存区为中心

暂存区与HEAD比较：git diff --cached

暂存区与工作区比较:  git diff

暂存区恢复成HEAD : git reset HEAD

暂存区覆盖工作区修改：git checkout 

## 消除最近的几次提交

可以使用git reset --hard HEAD~n   

## 删除文件的方法

平时都是在系统管理器删除文件后，再add .

应该使用 git rm filename1 filename2  删除文件跟添加到暂存区了

## 加塞任务之 stash

WIP working in progress

- git stash
保存当前工作进度，会把暂存区和工作区的改动保存起来。执行完这个命令后，在运行git status命令，就会发现当前是一个干净的工作区，没有任何改动。使用git stash save 'message...'可以添加一些注释

- git stash list
显示保存进度的列表。也就意味着，git stash命令可以多次执行。

- git stash pop [-–index] [stash_id]
git stash pop 恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。 
git stash pop --index 恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区） 
git stash pop stash_id 恢复指定的进度到工作区。stash_id是通过git stash list命令得到的 通过git stash pop命令恢复进度后，会删除当前进度。

git stash apply [-–index] [stash_id]
除了不删除恢复的进度之外，其余和git stash pop命令一样。

git stash drop [stash_id]
删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

git stash clear
删除所有存储的进度。


## 如何指定不需要Git管理的文件

.gitignore 定义不跟踪目录、文件

常见配置
```
# 此为注释 – 将被 Git 忽略

*.a       # 忽略所有 .a 后缀结尾的文件  * 通用匹配零个或多个字符
!lib.a    # 但 lib.a 除外   !开头的模式标识否定
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
*ignore/  # 忽略名称中末尾为ignore的文件夹
*ignore*/ # 忽略名称中间包含ignore的文件夹
**/node_modules  #   ** 匹配多级目录，可在开始，中间，结束
```

如果文件已经添加到暂存区 可使用git rm -- cached name_of_file 删除


## 备份文件

```shell
# 拉取克隆远程仓库
git clone --bare file:///XXXX/.git  remoteRepoName.git
# bare不包含工作区，作为远端，以后备份方便一点


# 本地初始化仓库关联远程

git remote add remoteRepoName file:///XXXX/remoteRepoName.git

# 本地发送一些改动，比如新增分支develop 推送分支变动到远程

git push --set-upstream remoteRepoName develop

# 再到远程库下面查看就会发现远程也有develop分支了
```

协议
- 本地协议
- 远程http/https 
- ssh


# github
  [ssh文档链接](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

  1. 打开 Git Bash。  输入ls -al ~/.ssh以查看是否存在现有的 SSH 密钥。
  2. ssh-keygen -t ed25519 -C "your_email@example.com"
  3. 打开id_ed25519.pub 复制内容
  4. 打开github SSH and GPG keys 设置 new keys

这样就使用ssl关联了一对的公私钥

远程新建项目,生成带readme license的项目

1. 关联项目 git remote add origin url/ssh
2. 通过 git branch -va 可查看是否添加到远程
3. 使用 git push origin/master --all 会拒绝master分支提交 但dev会提交成功
4. git fetch origin/master
5. git merge origin/master 出现报错fatal: refusing to merge unrelated histories
6. 添加参数    --allow-unrelated-histories  allow merging unrelated histories
7. git merge origin/master --allow-unrelated-histories
8. git push origin --all



```shell
# 设置默认上游分支
# -u, --set-upstream    set upstream for git pull/status
git push -u origin master 
# 其实，执行添加了-u 参数的命令 git push -u origin master就相当于是执行了
git branch --set-upstream master origin/master
git push origin master
# 可通过git branch -vv查看跟踪关系
```


# 同一分支多人协做

本地再clone 一个项目，修改username email 模拟多人

## 不同人修改不同文件
- 其它人先push 远程分析，这时你再push，会被拒绝。
- 这种情况不会产生冲突，但需要git fetch git merge/ git pull 产品新的merge commit 后就可以git push


## 不同人修改同文件不同区域

这个跟上面情况一样，没有冲突


## 不同人修改同文件同区域

要处理冲突后 再
- git add
- git commit
- git push


## 不同人修改同文件同区域
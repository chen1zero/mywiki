

## git中的几个概念

工作区 Working Directory  也即自己本地存放代码的文件夹目录  

暂存区 stage   可以理解为git add后git commit 前  

版本库 Repository 工作区有一个隐藏目录 .git ，这个不算工作区，而是 Git 的版本库。  

Git 的版本库里存了很多东西，其中最重要的就是称为 stage（或者叫 index）的暂存区，还有 Git 为我们自动创建的第一个分支 master，以及指向 master的一个指针叫 HEAD。  

将文件往 Git 版本库里添加的时候，其实是分两步执行的：  

- 第一步是用 **git add 将需要文件修改都添加到暂存区**。  
- 第二步是用 **git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支**在我们创建 Git 版本库时，Git 会自动为我们创建分支。  

**git status查看git状态：红色文件表示在工作区，绿色文件表示在暂存区 ** 



## 常用git命令

### 最常用

git init 在当前目录下初始化git仓库  

git clone [url]  克隆远程仓库到当前目录  

**git add . 将所有文件的修改添加到暂存区**  

注：git add [file]  将指定文件的修改添加到暂存区  

**git commit -m "commit message" 提交修改，可以自定义修改的备注信息 ** 

git remote add origin https://github.com/chen1zero/mywiki.git   如果是第一次提交，需要先设置下远端仓库

**git push origin main 将暂存区的修改推送到远程仓库**  

注：第一次推送可以使用-u选项git push -u origin main建立本地分支和远端分支的跟踪关系，后续推送只需要git push即可  

### git fetch

获取所有远程分支的更新，但不会对当前分支进行任何更改——所以不会产生冲突  

### git merge

git merge 分支名feature  将feature 分支的更改合并到当前分支‌  

### git pull

同步远程仓库的最新提交并将其合并到当前分支，**适合在保持本地分支与远程分支同步时使用**  

有可能产生冲突  

git pull = git fetch+git merge   



### git push

git push origin main 将本地分支的修改推送到远程仓库  

git push -f origin main 强制将本地的更改推送到远程仓库，会覆盖远程仓库中的相应分支  

### git checkout

git checkout 检出分支或文件——**危险操作，会重写工作区，慎用 ** 

git checkout 分支名称如dev   将工作目录切换到dev分支，并更新本地代码库为该分支的最新状态‌  

git checkout <commit_id> <file_path>  将指定文件恢复到上一版本的状态‌  

怎么理解检出：  

git checkout <file_path> 检出的默认是从暂存区检出，用暂存区的文件覆盖工作区的文件，**撤销工作区的更改**  

git checkout <commit_id> <file_path>  如果＜commit_id＞不省略，则是用指定提交中的文件覆盖暂存区和工作区中对应的文件  

git checkout <commit_id> <file_path>  这种用法不会改变HEAD头指针，因为只有HEAD切换到一个分支才可以对提交进行跟踪，否则会进入“分离头指针”的状态。在“分离头指针”状态下的提交不能被引用关联到，可能丢失  

git checkout 分支名称branch   检出branch分支，包含三步：更新HEAD以指向branch分支、用branch指向的树更新暂存区、更新工作区  

git checkout branch -- filename 维持HEAD的指向不变。用branch所指向的提交中的filename替换暂存区和工作区中相应的文件。会将暂存区和工作区中的filename文件直接覆盖  

git checkout 相当于命令后的参数为一个点"."。**这条命令最危险**！会取消所有本地的修改，相当于用暂存区的所有文件直接覆盖本地文件  

git checkout -b [new-branch] 创建并切换到新的分支（从当前提交开始创建）  



### git branch

git branch -d [branch-name]命令可以删除已合并的分支  

注：如果分支还有未合并的更改，需要使用git branch -D [branch-name]命令强制删除‌  

git branch -m [old-branch-name] [new-branch-name]命令可以重命名当前检出的分支。  

注：如果新名称已存在，需要使用git branch -M [old-branch-name] [new-branch-name]命令强制重命名‌  

git branch --merged  列出所有已被合并到当前分支的分支‌  

git branch --no-merged 列出还未被合并到当前分支的分支‌  

git branch 查看所有本地分支  

git branch -r 列出所有远程分支  

git branch -a 列出所有本地分支和远程分支  

### git reset

适用于需要**彻底回到某个特定版本**，并丢弃后续所有更改的场景；**撤销暂存区的更改**，reset执行后不会产生commit记录  

git reset --hard [commit_id] 回退到指定commit  

git reset [--soft | --hard  | --mixed] [HEAD]  

git reset --soft 回退到某个版本，但保留工作目录和暂存区的所有更改  

git reset --hard 回退到某个版本，同时放弃工作目录和暂存区的所有更改  

git reset --mixed **默认方式**，回退到某个版本，保留本地未提交的更改，但暂存区的更改会被移除  

**获取远端仓库的最新提交并强制覆盖本地代码：**  

git fetch  

git reset –hard origin/main  

### git revert

适用于需要撤销某个特定提交，但保留后续更改的情况，revert执行后会产生commit记录  

git reset --hard HEAD^ 回退到上一个commit  

git revert HEAD 撤销最近的提交  

git revert commitId 撤销指定的提交，同时保留该提交之后的所有其他提交  

### 一些查询

git log 查看提交历史  

git remote -v 查看远端仓库  

git diff [file] 查看文件的不同  
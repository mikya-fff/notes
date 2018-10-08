
### git checkout 
未添加到暂存区的时候即只在工作区进行了更改，git status会出现git checkout  
> 不保存工作区的修改，拷贝另外一个文件来覆盖它  
> git checkout -- \<file\>
> git checkout commit/branch-name
> git checkout -b new-branch base-branch 基于base-branch拉取出来的new-branch


### git reset
更改已经添加到暂存区，即git add <file> --> git status会显示{+ git reset +}  
> 取消暂存，将暂存区的文件用HEAD版本中的文件代替  
> git reset HEAD <file> 

如果使用git reset命令，则工作区和暂存区的文件是不一样的，此时的状态与只在工作区做了修改未提交到暂存区一样  
没有指明--soft或者--hard则代表--mixed，即修改暂存区的内容

> 只是将HEAD指针指向HEAD^2,不修改暂存区和工作区的内容  
> git reset --soft HEAD^2 

> HEAD指针指向HEAD^2,修改暂存区,不修改工作区的内容  
> git reset [--mixed] HEAD^2 

> HEAD指针指向HEAD^2，修改暂存区和工作区的内容  
> git reset --hard HEAD^2 

> 撤销 git reset --soft|--hard|--mixed HEAD^2对文件的操作  
> git reset --soft|--hard|--mixed HEAD@{2}  退到当前版本的后2个版本
            

### git diff
 1. git diff默认比较的是当前工作区与上次添加到暂存区中的文件的不同
 2. git diff --cached或者git diff --staged 比较的是最新一次提交到暂存区与倒数第二此提交到暂存区内容
 3. git diff commitid1 commitid2,比较指定id,默认生成patch文件.commitid1是指要更改的提交 commitid2是指文件更改后的内容的那笔提交

### git apply 
 git apply patch_file,如果patch可以成功打入，则diff时commitid1必须时最新的commitid,应用patch后文件会恢复到commit2时候的内容

### git rm
 1. 当文件git add加入暂存区后，只能使用git rm --cached file 或者git rm -f file 
    git rm log/\*.a 删除log下的所有以.a文件
 2. git commit 后文件才可以使用git rm 命令，git rm命令会将工作区的文件一起删除，相当于rm后再 git add

### git fetch 
  git fetch remote_name 拉取代码,不合并，remote_name是添加远程仓库的名称
    当本地仓库与远程仓库的提交不同步的时候使用git fetch更新远程仓库的引用
  git fetch origin :foo 在本地创建foo分支

### git clone 
  git clone repository_url 拉取代码，远程分支,将远程名默认为origin，默认设置本地master跟踪远程master分支  
  git clone -o mine 将远程名定义为mine
  git clone repository_url local_repo_name ,不指定local_repo_name时，使用远程仓库名作为新的仓库名，即文件夹的名称
      这会在当前目录下创建一个名为 “local_repo_name” 的目录,并在这个目录下初始化一个.git 文件夹,从远程仓库拉取下所有数据放入.git 文件夹,然后从中读取最新版本的文件的拷贝。

### git pull
  git pull remote_name 拉取代码并合并
  git pull remotename remotebranch:localbranch

### git branch
  
 1. git branch branch_name,git checkout branch_name == git checkout -b branch_name
 2. git checkout -b local_branch remote_name/remote_branch 创建本地分支指定这个分支所跟踪的远程分支，并切换到这个新建的分支上  
    等同于 git checkout --track remote_name/remote_branch 此时本地分支名与远程分支名相同
 3. git branch -vv 查看本地分支和追踪的远程分支以及最后一笔提交，但是远程分支是从服务器上最后一次抓取的数据
    git fetch --all ;git branch -vv  可以统计出最新的远程分支与本地分支的差别
 4. git branch -u remote_branch brnach-name 将当前分支追踪远程分支
    等同于 git checkout branch-name && git branch -u remote-branch
 5. git branch -r 查看远程的分支情况 
 6. git branch --merged|--no-merged ,列出已经合并会未合并到当前分支的所有分支列表
 7. git branch -f branchname commitid 将master分支指向commitid（有可能是其他分支上的id）

### git merge
 1. 当merge有冲突的时候，解决冲突之后，要重新提交
    git merge another—branch ,将another-branch merge到当前的分支上
    git fetch 后，git merge origin/serverfix 合并fetch下来的文件

### git rebase
 1. git rebase -i commitid或HEAD^^ commitid或HEAD^^是指你要回退的那个点的前面一笔提交的commitid  
    根据弹出的注释修改相应的操作,完成后保存并退出
 2. 会有操作提示,如果需要修改多笔提交，则git rebase --continue之后会有操作提示
    - 先git commit --amend   
      - 如果需要更改用户名和邮箱，修改.gitconfig之后 git commit --amend --reset-author
    - 再git rebase --continue
      - 如果不执行这一步，执行其他的操作时，会提示有rebase操作未完成，结合提示和实际情况
 3. 通过rebase合并分支，将testing分支合并到master分支，master是基底  
    变基：找到testing和master的共同祖先，然后testing相对于祖先的历次提交的修改文件相当于patch，然后让testing指向master，并应用这个patch  
    git checkout testing   
    git rebase master  
    合并：此时合并就是快速合并了，直接移动指针指向即可，因为变基之后所有的提交都在一条线上  
    git checkout master  
    git merge testing   
 4. git rebase master testing ,以master为基底执行变基
    git checkout master
 5. git rebase --onto master server client 选中在 client 分支里但不在server 分支里的修改(即 C8 和 C9),将它们在 master 分支上重演
 6. git rebase 原来分支上的提交都在master上了，testing分支依然存在
 7. 如果每笔提交都在不同的分支上，想用rebase到另外一个分支的某笔提交，则rebase时要指明那个分支上的那笔提交，否则就rebase到其他分支上了

### git push 
 1. 删除远程服务器上的分支 git push --delete remote_branchname  
    git branch -r -d branch_name 只是删除了追踪关系，并没有真正删除服务器上的分支
 2. 在远程服务器上创建分支 git push origin -u branch_name
 3. git push出错，需要重新git pull
    git pull --rebase  
    因为删除了.git文件所以重新git remote add ,所以需要设置分支  
    git branch --set-upstream-to=origin/master master  
    git pull --rebase   
    解决冲突后  
    git rebase --continue  
    git push origin HEAD:master  
   .gitignore忽略文件，ide本身生成的文件，属于你个人的文件都需要被忽略  
 4. git push remote_name localbranch:remotebranch
 5. git push --force覆盖服务器上的提交
 6. git push :foo 删除远程仓库的foo这个分支

### git remote
 1. git ls-remote 远程服务器的信息
 2. git remote show remote-name 显示远程仓库的信息

### git add
 1. git add file1 [file2]/dir 如果add一个目录，则会递归地跟踪目录下所有的文件并且将其添加到暂存区

### git mv
 1. mv old new -> git rm old > git add new = git mv

### git tag
 1. 轻量标签,不使用-a,-s,-m
  git tag tagname [commitid] 不指定commitid,默认是对HEAD
 2. 标注标签
  git tag -a tagname -m "tag message" [commitid] 不指定commitid,默认是对HEAD
 3. git show tagname
 4. git push remotename tagname
    git push remotename --tags 将所有不在远程仓库服务器上的标签全部传送到那里

### git revert
 1. 提交一个新的版本，将需要revert的版本的内容再反向修改回去，即撤销指定revert版本所做的所有修改，版本会递增，不影响之前提交的内容
  如果不是反向HEAD，则会把指定的sha1到HEAD之间的所修改的内容都撤销
 2. 撤销指定的版本，撤销也会作为一次提交进行保存，这个新的commit会将sha commit所做的一切撤销
> git revert commitid|HEAD^ 
 3. 当你的提交已经push到远程，使用rebase和reset时会影响其他用户，这个时候使用revert即可

### git reflog
  可以查看所有分支的所有操作记录，包括被删除的commit和reset，如果想回退到某个操作命令，可以使用git reflog来查看commit  
  查询结果显示:
> ab866d7 HEAD@{0}: checkout: moving from master to ab866d7

### git log
  1. 以图形的方式查看所有分支的log,--decorate会显示分支信息
> git log --oneline --decorate --graph --all
  2. 指定gitlog的现实内容，使用--pretty=format:"需要显示的内容以及显示的格式"
> git log --pretty=format:"%h %s" %h简短的sha1,%s是commit message

### git cherry-pick
将其他分支的改动cherrypick到当前分支上

### git describe
离最近的 tag 差了多少个 commit
返回值是 tag_num-commit_g<hash>,tag标签名，num-commit代表最近的tag距离当前HEAD相差的提交数，hash是拥有tag的commitid


### git fakeTeamwork
在本地已经克隆远程仓库的情况下，使用这个命令可以在远程仓库上直接提交
git fakeTeamwork remote-branch-name commit-num
git fakeTeamwork master 3 ,在远程仓库的master分支上，生成三笔提交


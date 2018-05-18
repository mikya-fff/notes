### git checkout 
 未添加到暂存区的时候即只在工作区进行了更改，git status会出现git checkout  
 git checkout -- <file> 是指不保存工作区的修改
 
### git reset
 更改已经添加到暂存区，即git add <file> --> git status会显示git reset  
 git reset HEAD <file>将暂存区的文件用HEAD版本中的文件代替  
 如果使用git reset命令，则工作区和暂存区的文件是不一样的，此时的状态与只在工作区做了修改未提交到暂存区一样  
 
### git diff
 1. git diff默认比较的是当前工作区与上次添加到暂存区中的文件的不同
 2. git diff --cached或者git diff --staged 比较的是最新一次提交到暂存区与倒数第二此提交到暂存区内容
 3. git diff commitid1 commitid2,比较指定id,默认生成patch文件
 
### git apply 
 git apply patch_file,如果patch可以成功打入，则diff时commitid1必须时最新的commitid,应用patch后文件会恢复到commit2时候的内容
 
### git rm
 1. 当文件git add加入暂存区后，只能使用git rm --cached file 或者git rm -f file 
 2. git commit 后文件才可以使用git rm 命令，此命令会将工作区的文件一起删除，相当于rm命令
 
### git fetch 、 git clone 、 git pull
 1. git fetch remote_name 拉取代码,不合并，remote_name是添加远程仓库的名称
 2. git clone repository_url 拉取代码，远程分支,将远程名默认为origin，默认设置本地master跟踪远程master分支  
    git clone -o mine 将远程名定义为mine
 3. git pull remote_name 拉取代码并合并
 
### git branch , git merge
 1. 当merge有冲突的时候，解决冲突之后，要重新提交
 2. 以图形的方式查看所有分支的log,--decorate会显示分支信息 git log --oneline --decorate --graph --all
 3. git branch branch_name,git checkout branch_name == git checkout -b branch_name
 4. git checkout -b local_branch remote_name/remote_branch 创建本地分支指定这个分支所跟踪的远程分支，并切换到这个新建的分支上  
    等同于 git checkout --track remote_name/remote_branch 此时本地分支名与远程分支名相同
 5. git branch -vv 查看本地分支和追踪的远程分支以及最后一笔提交
 6. git branch -u remote_branch 将当前分支追踪远程分支
 7. git branch -r 查看远程的分支情况 
 
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
    合并：  
    git checkout master  
    git merge testing   
    

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
    
### git remote
 1. git ls-remote 远程服务器的信息

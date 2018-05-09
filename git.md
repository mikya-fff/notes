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
 2. git clone repository_url 拉取代码，远程分支

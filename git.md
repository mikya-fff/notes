### git checkout 
 未添加到暂存区的时候即只在工作区进行了更改，git status会出现git checkout  
 git checkout -- <file> 是指不保存工作区的修改
### git reset
 更改已经添加到暂存区，即git add <file> --> git status会显示git reset  
 git reset HEAD <file>将暂存区的文件用HEAD版本中的文件代替  
 如果使用git reset命令，则工作区和暂存区的文件是不一样的，此时的状态与只在工作区做了修改未提交到暂存区一样  

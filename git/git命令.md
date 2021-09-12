

### git创建分支并配置命令
**创建本地分支并推送到远程：**

git checkout -b dev            本地创建并切换分支

git push origin dev:dev      将dev分支推送到远程（冒号后的分支远程origin没有，会自动创建）
或者 git push --set-upstream origin dev           *dev为创建分支的名字(远程分支自动创建)*

git branch --set-upstream-to=origin/<远程分支> <本地分支>      设置上游分支，本地分支可忽略

git branch --unset-upstream 分支名       取消对分支的跟踪

git branch -d dev           删除本地分支

git push origin --delete dev          删除远程分支

**设置、查看上游分支**
git branch -vv             查看本地分支及对应的远程分支




### 回退版本
git log   		 //查看每次更改的记录
git reset --hard 版本标识符          回退到具体某一版本



git stash  //将当前修改存入缓存

git apply   //取出存入的缓存

上传目录下所有文件：     git add ./*
















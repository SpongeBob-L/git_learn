Git的学习与应用
------

1、拉取仓库到本地
> git clone git@github.com:SpongeBob-L/git_learn.git

2、查看所有分支信息
>\# 查看本地分支（当前分支前有 * 标记）
>git branch
>
>\# 查看所有分支（本地 + 远程）
>git branch -a

3、添加分支
>\# 创建新分支（不切换）
>git branch <新分支名>
>
>\# 创建并切换到新分支
>git checkout -b <新分支名>

4、切换分支
>git switch <分支名>

5、删除分支
>\# 删除本地分支（需先切换到其他分支）
>git branch -d <分支名>      # 安全删除（已合并）
>git branch -D <分支名>      # 强制删除
>
>\# 删除远程分支
>git push origin --delete <分支名>

6、选择分支推送到远程仓库
>\# 推送当前分支到远程（同名分支）
>git push origin HEAD
>
>\# 推送本地指定分支到远程（自动创建远程分支）
>git push origin <本地分支名>
>
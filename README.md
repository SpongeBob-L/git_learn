# 有关Git的学习内容

&emsp;&emsp;这是我为了学习并理解Git而编写的文章。

### 一、Git 的基础概念

&emsp;&emsp;学习Git，主要就要理解两个概念：<br>
&emsp;&emsp;1、什么是回溯？<br>
&emsp;&emsp;用一个“游戏存档”的比喻来理解 Git。<br>
&emsp;&emsp;当你玩一款游戏，游戏途中每完成一个重要任务，我们都会做一次存档点，而Git的存档会比游戏当中还要高级一些，Git能够回溯到任意的存档点并且还拥有更强大的功能。<br>
&emsp;&emsp;2、什么是分支？<br>
&emsp;&emsp;以Minecraft(我的世界)这款游戏为例，我的世界里可以创建多个世界，而Git中可以创建多个分支。当你想要为自己的主分支做出改变但又不想影响主分支时，你就可以拷贝一份主分支到一个新分支当中，这样就能在新分支中随意改动，当改动到满意的程度还能合并到主分支当中。分支可以理解为副本，该副本可以是空的也可以是拷贝下来的。

### 二、最常用的基础命令

-  **git init** 

&emsp;&emsp;在文件夹内使用该命令，该文件夹就会升级成超级无敌记忆文件夹，并且出现以下语句：<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**Initialized empty Git repository in <XX路径>**<br>
&emsp;&emsp;这个语句的意思是&emsp;&emsp;**在XX路径中初始化空Git仓库**<br>

-  **git add .**

&emsp;&emsp;执行该命令时啥也不会发生，但是！已经把改动所有的文件加入“准备存档”列表了。

-  **git commit -m '说明'**

&emsp;&emsp;当执行完后，会出现一串字，上面有说明，有改动的文件数量，有执行的操作，这就是完成了存档，会在log中记录存档点。

&emsp;&emsp;如果出现了**nothing to commit**，那说明啥也没干，那存个球档，无法存档。

-  **git log**

&emsp;&emsp;能看到所有存档点记录，每个存档点都有一个唯一的ID(是一串乱码)。


### 三、回溯功能 —— 时光倒流

&emsp;&emsp;Git 的“回溯”就是把整个项目变回以前某个存档点的样子。

&emsp;&emsp;假设你有三个存档点：

&emsp;&emsp;存档点 A ：写了第一章

&emsp;&emsp;存档点 B ：写了第二章

&emsp;&emsp;存档点 C ：写了第三章（当前进度）

&emsp;&emsp;现在你想回到存档点 B（回到第二章刚写完的状态），有几种方法：

#### 1、git checkout —— 只“看”过去，不破坏未来

- **git checkout b789abc**   # b789abc 是存档点 B 的 ID

&emsp;&emsp;执行后，你的游戏界面显示的是第二章刚写完的样子，你可以看看代码、修个小 bug，但 Git 会提醒你：“现在是只读模式，如果修改，最好新开一个分支。”

&emsp;&emsp;特点：未来的存档点 C 还在，随时可以切回去：

- **git checkout main**         # 回到最新分支（存档点 C ）

#### 2、git reset --hard —— 真·时光倒流（可能丢掉未来）

- **git reset --hard b789abc**

&emsp;&emsp;这下真的把项目状态强行改回存档点 B ，同时会把当前分支的指针也挪到 B。

&emsp;&emsp;此时再用 **git log** 看，再也看不到存档点 C 了，好像它被删除了一样。

&emsp;&emsp;那未来存档点 C 真的没了吗？

&emsp;&emsp;不是真的消失。Git 其实偷偷留着所有历史，只要还没运行 **git gc** 清理垃圾，可以用 **git reflog** 找回存档点 C 的 ID，然后恢复。但一般人会觉得“我回不去了”，所以慎用 reset --hard。

>   log里面会这样变动：A → B → C  变为 A → B （C看不到了）

#### 3、git revert —— 反做某个存档，不改变历史

- **git revert c789abc**   # 新产生一个存档点 D，内容刚好是“把存档点 C 的改动撤销掉”即存档点 B 的拷贝

&emsp;&emsp;这样在主线里“假装”没有发生过存档点 C，但存档点 C 依然存在，只是效果被抵消了。最安全，适合多人协作。

>   log里面会这样变动：A → B → C   变为 A → B → C → D( D = B , C还在 ）

#### 结论

- 如果用 git checkout 切到过去，未来的存档点完好无损放在仓库里，随时可以切回去。

- 如果用 git reset --hard 并切到过去，未来的存档点在分支记录里看不见了，但物理上还存在，可以用 git reflog 找回（就像电脑回收站里的文件）。

- 如果用 git revert 产生反做存档点，所有存档点都在，只是多了一个“抵消”的新存档点。

### 四、分支基础命令

#### 1、查看所有分支信息

- **git branch**&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;# 查看本地分支（当前分支前有 * 标记）
- **git branch -a**&emsp;&emsp;&emsp;&emsp;# 查看所有分支（本地 + 远程）
- **git switch <分支名>**&emsp;# 切换分支

#### 2、添加分支

- **git branch <新分支名>**&emsp;&emsp;&emsp;&nbsp;# 创建新分支（不切换）
- **git checkout -b <新分支名>**&emsp;# 创建并切换到新分支

#### 3、删除分支

&emsp;&emsp;# 删除本地分支（需先切换到其他分支）

- **git branch -d <分支名>**&emsp;# 安全删除（已合并）
- **git branch -D <分支名>**&emsp;# 强制删除

&emsp;&emsp;# 删除远程分支

- **git push origin --delete <分支名>**

### 五、上传云端

#### 1、简单操作

- (1) 拉取自己的仓库到本地

&emsp;&emsp;**git clone git@github.com:SpongeBob-L/git_learn.git**

- (2) 编写内容并保存

&emsp;&emsp;**git add .**&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;# 保存内容<br>
&emsp;&emsp;**git commit -m '存档点名称'**&emsp;# 提交存档

- (3) 选择分支推送到远程仓库
- **git push origin HEAD**&emsp;&emsp;&emsp;&emsp;# 推送当前分支到远程
- **git push origin <本地分支名>**&nbsp;&nbsp;# 推送本地指定分支到远程（自动创建远程分支）

#### 2、不简单操作

- **本地存档**：从零开始新建了一个“游戏存档”，已经玩了 10 个小时，存了好几个档（commit）。
- **云端存档**：是另一个朋友（或者模板）已经存了一些内容，比如有一个 `README.md` 文件。

---------

&emsp;&emsp;这时候想**直接把本地存档推上去覆盖云端**

&emsp;&emsp;Git 会说：“不行！云端已经有内容了，而且跟本地没有任何共同的历史记录。我无法判断你是想合并还是想彻底毁灭云端的成果。”

&emsp;&emsp;这就是典型的 **“本地和云端没有公共祖先”** 或 **“本地落后于云端”** 的情况。

---------

当执行 `git push origin main`（假设分支叫 main）时，可能会看到：

```
error: failed to push some refs to 'github.com:xxx/xxx.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
```

翻译成人话：**“云端上有本地没有的东西，请先把它拉下来合并，再推上去。”**



**有哪几个选项？该如何操作？**

##### 1、正常的合并（推荐，最安全）

把云端已有的内容合并到本地，然后再把自己的内容推上去。

**步骤：**

```
# 1. 拉取云端的内容，并合并到本地工作
git pull origin main --allow-unrelated-histories
```

注意：因为两个存档没有共同祖先，需要加上 `--allow-unrelated-histories`，告诉 Git：“我就是要合并两个不同的历史。”

执行后 Git 可能会自动合并（如果文件不冲突），或者让你手动解决冲突（比如云端有个 README，你本地也有个 README，内容不同）。

```
# 2.解决冲突（如果有）：Git 会在冲突文件里标出 `<<<<<<<` 和 `>>>>>>>`，手动编辑保留想要的内容，然后：
git add .
git commit -m "合并云端内容"
```

```
# 3. 最后推送：
git push origin main
```

这样云端就会包含本地的所有内容，且云端的原有内容也没有丢失。



##### 2、强制推送（危险！会覆盖云端）

如果**确定云端的内容完全不想要**，比如云端只是一个空的初始化文件或者错误的内容，要用自己的本地存档**完全取代**云端。

```
git push --force origin main
# 或者更安全的 --force-with-lease
git push --force-with-lease origin main
```

**效果**：云端的存档会被本地存档整个替换掉，云端原有的 commit 会丢失（除非你提前备份）。
**适用场景**：

- 新建了一个仓库，云端只生成了一个 .gitignore 或 LICENSE。
- 在做自己的个人项目，没有别人协作。

**不适用场景**：和别人一起开发，别人的提交会被你一把抹掉，不推荐。



##### 3、如果云端是完全空的（初始化仓库但没有提交）

比如在 GitHub 上新建了一个仓库，**没有**勾选“添加 README”、“添加 .gitignore”等，那么这个仓库是真正空的（没有任何分支和提交）。此时可以直接推送，不需要特殊参数。

```
git push -u origin main   # -u 会设置上游，以后直接 git push 即可
```


##### 结论

| 情况                                                         | 推荐操作                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 云端有内容（哪怕只有一个文件），希望保留它，并把自己的工作加进去 | `git pull --allow-unrelated-histories origin main`，解决冲突，再 `push` |
| 云端有内容，但确定那些东西是垃圾，要完全用自己的覆盖         | `git push --force-with-lease origin main`（慎用）            |
| 云端是全新空仓库（没有任何提交）                             | 直接 `git push -u origin main`                               |
| 本地落后于云端但有共同祖先（正常多人协作情况）               | 先 `git pull`（不需要 `--allow-unrelated-histories`），再 `push` |

“万能检查”方法：

```
# 查看远程有哪些东西
git fetch origin
git log origin/main --oneline

# 如果发现远程有不认识的提交，且不想丢失
git pull origin main --allow-unrelated-histories
# 然后 push
```


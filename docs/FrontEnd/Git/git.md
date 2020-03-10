参考链接：[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

------



#### 创建版本库

- 先选择一个文件夹`cd d:/web` 
- 创建文件 `mkdir test`
- 初始化仓库 `git init`
- 添加文件到暂存区 `git add <file>`
- 提交文件到仓库 `git commit -m"<message>"`

#### 时光穿梭机

- 查看仓库状态 `git status`
- 忘记了修改内容，使用 `git diff <file>` 查看（工作区和仓库对比）

#### 版本回退

- 假设已对文件修改N次，也都提交仓库了，可使用 `git log` 查看由近到远的更改日志（会显示 `-m` 后面的信息，在后面加上 --`pretty=oneline` 可查看简洁版
- `HEAD^`表示上一版本，`HEAD^^`上上版本，`HEAD~100`上100个版本
- 退回 `git reset --hard HEAD^`
- 若退回想前进回去，找到版本号 `git reset --hard <id>` (输入前几位就可以）
- 可以用 `git reflog` 查看所有更改日志及版本号
- 查看文件内容 `cat <file>`
- 使用 `git diff HEAD -- <file>` 查看工作区和暂存区的区别

#### 撤销修改

- 用 `git checkout -- <file>` 撤销工作区修改，退回到`commit`版本或者`add`版本
- 用`HEAD`表示的是最新版本，指向当前分支
- 如果工作区的修改已经田添加到暂存区，使用 `git reset HEAD <file>` 将暂存区文件撤销到工作区，最后使用 `git checkout -- <file>` 将工作区的修改也撤销

#### 文件删除

- 删除 可以在文件管理器中删除，也可以用  `rm <file>` ,然后`add`、`commit`，此时仓库的就彻底删除了，若误删，不要`commit` 用上面的的撤销文件修改即可。
- 删除文件夹：`rm -r <file>`

#### 远程仓库

- 创建SSH	`KEY: ssh-keygen -t rsa -C "youremail@example.com"` (github邮箱）
- 关联远程仓库 `git remote add origin git@github.com:Xuliang-GitHub/test.git`
- 将本地内容推送到远程 `git push -u origin master` 加了 `-u` 参数后，`Git`不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令 例：`git push`代替 `git push origin master`

#### 从远程克隆

- 将远程文件克隆到本地计算机 `git clone git@github.com:Xuliang-GitHub/my-website.git`

- 创建合并分支

- 创建并切换分支 `git checkout -b dev` 表示 `git branch dev` 和 `git checkout dev`
- 查看分支 `git branch` 分支前面有*的表示当前分支
- 切换回主分支 `git checkout master`
- 合并指定分支到当前分支 `git merge dev`
- 删除指定分支 `git branch -d dev`

#### 解决冲突

- 当`master`分支和`dev`分支均对同一个地方修改时，在合并分支时就需要手动解决冲突，在文件中会显示 `Git`用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容
- `git log --graph` 可以看到分支合并图

#### 分支管理策略

- 加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并
- `git merge --no-ff -m"message" dev` 普通模式就是把`master`和`dev`分支合并成一个新的分支，而`fast forward` 时把`dev`分支合并到`master`上

#### bug分支

- 正在处理工作，突然休要创建新的分支修改bug，但是工作还没完成，又不能提交，此时可以将工作区文件零时存储起来 `git stash` 
- 处理完bug后用 `git stash list` 查看工作区的临时存储
- 恢复工作区 1.`git stash apply` ，然后删除`stash` 中的内容 `git stash drop`

​          2.`git stash pop` 恢复内容的同时删除 `stash` 中内容

#### feature分支

- 开发一个新`feature`，最好新建一个分支
- 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除
- 要切换回其主分支才能删除下面的子分支，不能在子分支上进行删除操作

#### 多人协作

- 推送分支 `git push origin master`

​        `git push origin dev` (推送其他分支）

- 查看可 `push` 和 `fetch 的地址 git remote -v`

- 并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

  - `master`分支是主分支，因此要时刻与远程同步

  - `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

  - `bug`分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug

  - `feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发

- 另一台电脑从远程`clone`到本地默认只有`master` ，需要创建一个本地分支与远程分支对应（名字最好一样） `git checkout -b dev origin/dev`

- `git pull`失败的话用 `git branch --set-upstream-to=origin/<branch> dev`

- 因此，多人协作的工作模式通常是这样：

  1. 首先，可以试图用`git push origin `推送自己的修改；
  2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用`git push origin `推送就能成功！

  如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`

- rebase操作可以把本地未push的分叉提交历史整理成直线 git rebase

#### 标签管理

- 版本号很复杂，为了简便用一个标签代表版本号，可以理解为网址代表域名，方便大家记忆，`tag`就是一个让人容易记住的有意义的名字，它跟某个`commit`绑在一起

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个`commit id: git tag V1.1 e1e9c68`

- `git tag` 查看所有标签

- 如果忘记打标签，可以找出其`id`号，然后指定`id` 打标签

- `tag` 不是按时间排序的，是按字母排序，查看标签详细信息：`git show <tagname>`

- 创建带有说明的标签，用-`a`指定标签名，`-m`指定说明文字

- 删除标签 `git tag -d v0.1`

- 标签只在本地存储，不会自动推送到远程，如果要推送到远程 `git push origin <tagname>`

- 一次性推送全部标签 `git push origin --tags`

- 删除远程标签 `git push origin :refs/tags/<tagname>`

- 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

  ```
  $ git tag -d v0.9
  Deleted tag 'v0.9' (was f52c633)
  ```

  然后，从远程删除。删除命令也是push，但是格式如下：

  ```
  $ git push origin :refs/tags/v0.9
  To github.com:michaelliao/learngit.git
   - [deleted]         v0.9
  ```

  要看看是否真的从远程库删除了标签，可以登陆GitHub查看

#### 补充

- `git add -A`  提交所有变化
- `git add -u`  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
- `git add .`  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件（已经验证，的确有效）
- `ls` 列出所有文件
- 按`tab`键填充文件名
- `touch`新建文件
- 创建多个文件夹 `mkdir audios images videos` 用空格分隔开
- 在另一条电脑上使用 `clone` 如果没添加`SSH` ，只能用http协议:`git clone https://github.com/Xuliang-GitHub/my-website.git`  `SSH：git@github.com:Xuliang-GitHub/my-website.git` 
- 可以直接先克隆，无需初始化，之前电脑的修改日志就在上面，里面包含`.git`文件
- 切换成`http`方式：`git remote set-url origin http://git.haohuoduo.cn/haohuoduo/android.git`切换成`ssh`方式：`git remote set-url origin git@git.haohuoduo.cn:haohuoduo/android.git`
- `git push` 后不加参数的时候，默认就是`git push origin` 当前的分支名，比如对本地的`master`分支执行`git push`，其实就是`git push origin master`
- 删除本地分支：`git branch -d` 分支名（`remotes/origin/分支名`）

- 强制删本地：`git branch -D 分支名`

- 删除远程分支：`git push origin --delete 分支名`（`remotes/origin/分支名`）

- `git branch -r` 查看远程分支
- `git branch -a` 查看所有分支
- 更新本地远程已删除的分支
  `git remote prune origin`
- 删除 `untracked files`：`git clean -f`
  连`untracked`的目录也一起删除：`git clean -fd`
  用上述`git clean` 前，建议加上`-n`参数先看看会删除那些文件
  `git clean -nf`
  `git clean -nfd`
- 查看某个指令怎么用 例：`git clean --help`



------

参考[git教程](https://www.liaoxuefeng.com/wiki/896043488029600)、[git官方文档](https://git-scm.com/doc)
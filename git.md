#git note
>整理自廖雪峰的git教程

##git配置
git config --global user.name "141220000-Zhang San"    // your student ID and name
git config --global user.email "zhangsan@foo.com"    // your email
git config --global core.editor vim            // your favorite editor
git config --global color.ui true

##初始化一个Git仓库
git init

添加文件到Git仓库，分两步：

##添加文件到仓库
git add file  //注意，可反复多次使用，添加多个文件
git commit


##查看工作区状态
git status

##查看比较修改内容
git diff file
git diff commit_id -- file

##版本回退
git reset --hard commit_id
>commit_id:
 * 哈希码的前几位或完整的哈希码
 * HEAD
 * HEAD^
 * HEAD^^
 * HEAD~100

##查看提交历史
git log

##查看命令历史
git reflog

##撤销修改
git checkout -- file
>把file文件在工作区的修改全部撤销，这里有两种情况：
一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次git commit或git add时的状态。

git reset HEAD file
>use "git reset HEAD file..." to unstage

##删除文件
git rm file
git commit
>如果误删了文件但还没有commit,可以用git checkout -- file回退;如果已经commit了，则直接回退即可

##远程仓库
1. 创建SSH key 
 ssh-keygen -t rsa -C "njuallen@foxmail.com"
 如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

2.登陆GitHub，打开“Account settings”，“SSH Keys”页面：
 然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

##添加远程仓库
1. 添加远程仓库
2. 在本地建立相应仓库，或者直接将本地仓库与远程仓库关联
 git remote add origin git@github.com:XXX/XXX.git
 git push -u origin master

##从远程库克隆
git clone git@github.com:XXX/XXX.git

##创建与合并分支
创建分支
git branch dev
切换分支
git checkout dev
创建并切换分支
git checkout -b dev
查看当前分支
git branch
删除分支
git branch -d dev
合并指定分支到当前分支
git merge dev
查看分支合并图
git log --graph --pretty=oneline --abbrev-commit(后面两项是可选的)
--graph 显示图形
--abbrev-commit commit_id简要显示
--pretty=oneline 每条git log只显示一行
>当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

##分支管理策略
fast forward模式合并分支时在主干记录上回直接把较新分支的更改历史以及相应的log复制到主干上，导致在主干上看不出分支以及合并记录。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
>分支策略
分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

##保存以及恢复还没有提交的工作(快照)
git stash 保存快照
git stash list 查看所有保存的快照
git stash apply 恢复快照但不删除stash内容
git stash drop  删除stash内容
git stash apply stash@{0} 如果有多个stash，可以恢复指定内容
git stash pop 恢复及删除stash

##feature分支
开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D name强行删除。

##多人协作
git remote 查看远程库的信息
git remote -v 显示更详细的信息，会显示可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
远程库的默认名称是origin

git push origin name 推送分支
>
 * master分支是主分支，因此要时刻与远程同步；
 * dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
 * bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
 * feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

git checkout -b dev origin/dev 创建远程origin的分支到本地
他人的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送。

git branch --set-upstream dev origin/dev 指定本地dev分支与远程origin/dev分支的链接

>因此，多人协作的工作模式通常是这样：
1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

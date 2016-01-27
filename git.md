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

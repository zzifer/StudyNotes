# 链接

[尚硅谷新版Git快速入门](https://www.bilibili.com/video/BV1wm4y1z7Dg?p=1)
019-021
029 ssh
030 gitlab

[尚硅谷Git入门到精通全套教程（涵盖GitHub\Gitee码云\GitLab）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vy4y1s7k6?p=1)
[(25条消息) Git学习笔记_巨輪的博客-CSDN博客](https://blog.csdn.net/u011863024/article/details/118562748)
038-040 gitee
041-044 gitlab


# Git工作机制
![](attachments/Pasted%20image%2020230209183632.png)
>一旦提交到本地库就会生成历史版本，代码就删不掉了，除非删掉本地库



# 设置用户签名
```git
# 不加--global的话就只对当前仓库配置
git config --global user.name 用户名
git config --global user.email 邮箱

# 可以在c盘用户下面的.gitconfig文件查看
```
>说明：**签名的作用是区分不同操作者身份**。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。 **Git 首次安装必须设置一下用户签名，否则无法提交代码**。

# 初始化本地库
```git
# 每次新建一个本地库就要初始化一下
git init
```

# 查看本地库状态
```git
# 查看暂存区状态 红色表示文件还未添加到暂存区
git status
```

# 文件误删除
```
# 从存储区中恢复工作区中删除的文件，
# 如果删除的的文件也已经commit则无法恢复
git restore 文件

# 可以使用reset重置到前面的版本恢复
# 但是可能会丢失提交过程
git reset --hard 版本号

# 还原到提交前的一个版本
# 这个操作不会丢失提交过程
git revert 版本号
```


# 添加/删除暂存区
```git
git add 文件名

# 将这个类型的文件添加到暂存区 
git add *.文件后缀

# 全部添加到暂存区
git add .


# 从暂存区中删除（只是从暂存区中删除，工作区还存在）
git rm --cached 文件名
```

# 提交本地库
```git
git commit -m "日志信息" 文件名

# 给提交增加标签，相当于给版本号起别名
git tag 标签 版本号

# 删除标签
git tag -d 标签
```

# 查看版本信息
```git
# 查看版本信息
git reflog

# 查看版本详细信息
git log
```

# 版本穿梭
```git
git reset --hard 版本号
```

# 分支的操作
```git
# 创建分支
git branch 分支名

# 查看分支
git branch -v

# 创建并切换分支
git checkout -b 分支名

git checkout -b 分支名 标签

# 切换分支
git checkout 分支名
git switch 分支名

# 删除分支
git branch -d 分支名

# 把指定分支合并到当前分支上
git merge 指定分支
```

# 合并冲突
```git

# 下面是要合并的txt文件，进入要合并的文件中，修改文件
# <<<<到==== 之间是当前分支的代码,====到>>>>是要合并的代码
# 修改文件然后删除<<<<  ====  >>>>这些符号
hello, git!
hello, git!
hello, git!
hello, git!
<<<<<<< HEAD
hello, git!hot-fix test
hello, git!master test
=======
hello, git!
hello, git!hot-fix test
>>>>>>> hot-fix
# 然后添加到暂存区再提交到本地库（注：这时使用git commit提交到本地库时不要带文件名，否则会报错）
# 修改并提交完成后，只会修改当前分支的文件，另外那个要合并的分支的文件不会发生改变
```

# Idea集成Git
## 配置Git忽略文件
与项目的实际功能无关，不参与服务器上部署运行的文件，把它们忽略掉能够屏蔽 IDE 工具之间的差异
创建忽略规则文件 xxxx.ignore（前缀名随便起，建议是git.ignore），这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig 文件引用，**建议**也放在用户家目录下。
模板如下
```git
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml
```
在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）
```git
[user]
    name = Layne
    email = Layne@atguigu.com
[core]
# 注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）”
	excludesfile = C:/Users/asus/git.ignore
```
然后再Idea配置Git程序，在菜单栏File->Setting->搜索栏搜Git，配置Git的安装路径（git安装目录下bin目录下的 git.exe）。

## Idea初始化Git
- 在菜单栏VCS -> Import into Version Control -> Create Git Repository -> 选择要创建 Git 本地仓库的工程（默认选中的目录就是当前项目的根目录）
- 添加到暂存区：右键红色的文件（如果是选择根目录就会添加整个项目到暂存区，这时会提示是否强制提交git.ignore中我们要忽略的文件选择cancel） -> Git -> Add
- 提交至本地库：右键要提交的文件，选择Git->Commit

## 切换版本
在 IDEA 的左下角，点击 Git，然后点击 Log 查看版本
![](attachments/Pasted%20image%2020230209205412.png)
右键选择要切换的版本，然后在菜单里点击 Checkout Revision
![](attachments/Pasted%20image%2020230209205521.png)
注意：Idea实现版本切换的功能，并不是使用reset，reset这种方法会直接把HEAD以及MASTER一起拉回那个版本，并且会丢失目标版本之后的内容，虽然可以通过reflog找到后面的版本，但是这种方法比较强烈，可能造成数据丢失。idea使用的是git checkout -b <branch-name> <commit>的方法临时再目标版本新建一个分支切过去看的，再切回来的时候，他会使用git branch -d <branch-name>的方法，再把临时创建的那个仅供会看目的的分支给删除，他的底层是创建一个临时的匿名分支，<commit为版本号>这种方式会让HEAD处于游离状态，恢复HEAD的方式就是用git switch master或者 git checkout master就可以回最新版本。

## 创建分支/切换分支
创建分支：右键项目选择Git -> Repository -> Branches 
![](attachments/Pasted%20image%2020230209205734.png)
或者 点击右下角（Git:当前分支）这个图标然后选择点击New Branch
![](attachments/Pasted%20image%2020230209205827.png)

切换分支：跟创建分支步骤相似，点击IDEA的右下角（Git:当前分支）这个图标，选择你想要切换的分支，然后checkout
![](attachments/Pasted%20image%2020230209210025.png)
或者在log窗口，右键点击分支，选择checkout：
![](attachments/Pasted%20image%2020230209210006.png)

## 分支合并
合并分支： 点击右下角（Git:当前分支）这个图标然后选择要合并的分支再点击Merge Into Current
![](attachments/Pasted%20image%2020230209210227.png)

如果代码没有冲突，分支直接合并成功，分支合并成功以后，代码自动提交，无需手动提交本地库。下面是合并冲突的情况，这时需要手动合并，点击merge。
![](attachments/Pasted%20image%2020230209210521.png)
点击merge后会出现下面3个框，左右分别是两个分支的代码中间是没有冲突的代码
![](attachments/Pasted%20image%2020230209210700.png)

# GitHub
![](attachments/Pasted%20image%2020230209165719.png)
![](attachments/Pasted%20image%2020230209165732.png)

## 推送本地库到远程库
```git
# 查看当前所有远程地址别名
git remote -v

# 给远程库创建别名
# 远程地址链接太长了记不住可以起别名
git remote add 别名 远程地址

# 修改别名
git remote rename 原别名 新别名

# 删除别名
git remote rm 别名

# 推送本地分支上的内容到远程仓库
# 下面的别名是远程库的别名,分支是本地库的分支
git push 别名 分支
git push <远程主机名> <本地分支名>:<远程分支名>

# 强制推送
git push -f

```

## 拉取远程库到本地库
```git
# pull的前提是已经有本地库
git pull 远程库地址别名 远程分支名
```

## 代码克隆  Clone
```git
# 克隆会自动初始化本地库，并为远程地址起别名为origin
git clone 远程地址
git clone 远程地址 新的本地仓库名
```

## SSH免密登录
先到用户的主页目录，删除.ssh文件夹（如果没有.ssh文件夹，忽略此步）：
```
# 运行下面的代码，连敲3次回车即可生成.ssh目录
# -t是指定哪种加密算法生成 rsa是一种非对称加密协议
# abc@123.com表示当前免密登录协议是针对这个账号
ssh-keygen -t rsa -C abc@123.com
```
id_rsa.pub是生成的公钥，将公钥添加至github账号设置，添加公钥后，可不用输入Github账号密码便可推送
![](attachments/Pasted%20image%2020230209203933.png)

## Idea集成GitHub
菜单栏File->Setting->搜索栏搜GitHub，添加GitHub账号
![](attachments/Pasted%20image%2020230209211001.png)
由于网络问题，会时常登陆不了，可通过Token登陆：进入github->setting->developer settings->personal access tokens->generate new token(note顺便写，note下面的选项是口令的权限全部打满)
另外一个方法是：![](attachments/Pasted%20image%2020230209165758.png)

将idea的项目直接分享到github（不用创建远程库）：vcs->import into version control->share projetct on github
repository name是远程库的名字，remote是远程地址的别名
![](attachments/Pasted%20image%2020230209211405.png)

右键项目->git->commit directory（先提交本地库）
本地库推送到远程库：右键项目->git->repository->push/vcs->git->push（但是这两个都是用https协议)
（使用上面的推送方式可能失败所以可以使用ssh）先复制远程库的ssh地址->vcs->git->push -> 点击左上角 -> define remote -> 输入name和url -> 点击左上角就可以选择使用ssh进行push
![](attachments/Pasted%20image%2020230209211903.png)
![](attachments/Pasted%20image%2020230209212020.png)
![](attachments/Pasted%20image%2020230209212126.png)

网络不行的其他解决方法：
![](attachments/Pasted%20image%2020230209212323.png)
![](attachments/Pasted%20image%2020230209212344.png)

pull远程库到本地库：vcs->git->pull

克隆代码到本地：打开idea时点击get from version control
![](attachments/Pasted%20image%2020230209212654.png)





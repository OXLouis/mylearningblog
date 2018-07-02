# Git 简介
Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

## Git 配置
    Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

    这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

        1 /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。
        2  ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
        3 当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。
    
    在 Windows 系统上，Git 会找寻用户主目录下的 .gitconfig 文件。主目录即 $HOME 变量指定的目录，一般都是 C:\Documents and Settings\$USER。

    此外，Git 还会尝试找寻 /etc/gitconfig 文件，只不过看当初 Git 装在什么目录，就以此作为根目录来定位。
    #用户信息

## Git 工作区、暂存区和版本库
    基本概念:
        工作区：就是你在电脑里能看到的目录。
        暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
        版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库
    
    基本变量：
        第一分支 master
        第一指针 HEAD 指向master
    基本操作：
        当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
        当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
        当执行 "git reset HEAD" 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
        当执行 "git rm --cached <file>" 命令时，会直接从暂存区删除文件，工作区则不做出改变。
        当执行 "git checkout ." 或者 "git checkout -- <file>" 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
        当执行 "git checkout HEAD ." 或者 "git checkout HEAD <file>" 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。
    分支操作：
        git branch (branchname)
        git checkout (branchname)
        git merge (branchname)
    查看历史：
        git log :
            --graph
            --reverse
            --author
            --oneline
            --since
            --before={x.weeks.ago}
            --until
            --after={year-mo-da}
    创建标签：
        git tag -a message [追加至版本的MD5] -m '标签信息'
        git tag  //查看所有标签
        发布一个版本时，我们通常先在版本库中打一个标签（tag），这样就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。
        所以，标签也是版本库的一个快照。
        Git 的标签虽然是版本库的快照，但其实它就是指向某个 commit 的指针
        Git 使用的标签有两种类型：轻量级的（lightweight）和含附注的（annotated）。
        轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。
        而含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 GNU Privacy Guard (GPG) 来签署或验证。
    远程仓库：
        git remote: 列出远程库的简称。 克隆某个远程项目后，可以看到一个名为origin的远程库
        git默认使用origin标识你所克隆的远程仓库。
        加上 -v 显示对应的克隆地址。

        git remote add [shortname] [url] :用于添加新的远程仓库。
        git fetch [shortname]: 从shortname中提取更新。
        git push [remote-name] [branch-name]: 将本地的分支推送到远端服务器上
        git remote show [remote-name]: git push 缺省时推送的分支是什么。
        哪些远端分支没有同步到本地。
        哪些已同步的分支在远端服务器上已经被删除。
        git pull 缺省时自动合并到哪些分支。
        git remote rename [a] [b] : 重命名远端在本地的简称。
        git remote rm [a]:移除远端服务器。


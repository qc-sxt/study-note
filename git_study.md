# git study

## git是什么

相比于其他版本控制系统，git直接记录快照而非进行差异比较。在git中，每当提交更新或保存项目状态时，它基本上就会对当时的全部文件创建一个快照并保存这个快照的索引。为了效率，如果文件没有修改，git不再重新存储该文件，而是只保留一个链接指向之前存储的文件。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506151810408.png" alt="image-20250615181008355" style="zoom:80%;" />

git有三种状态：

- 已提交（committed）：表示数据已经安全地保存在本地数据库中。
- 已修改（modified）：表示修改了文件，但还没保存到数据库中。
- 已暂存（staged）：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

这让git项目拥有三个阶段：工作区、暂存区以及git目录。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506151815452.png" alt="image-20250615181500395" style="zoom: 67%;" />

- 工作区：是对项目的某个版本独立提取出来的内容。这些从git仓库的压缩数据库中提取出来的文件，放在磁盘上供使用或修改。
- 暂存区：是一个文件，保存了下次将要提交的文件列表信息，一般在git仓库目录中。
- git仓库目录：是git用来保存项目的元数据和对象数据库的地方。这是git最重要的部分，从其他计算机克隆仓库时，复制的就是这里的数据。

## git配置

git自带一个`git config`工具来帮助设置控制git外观和行为的配置变量，这些变量存储在三个不同的位置，且每一个级别会覆盖上一级别的配置：

1. `/etc/gitconfig`文件：包含系统上每一个用户及他们仓库的通用配置。如果在执行git config时带上--system选项，那么它就会读写该文件中的配置变量。
2. `~/.gitconfig或~/.config/git/config`文件：只针对当前用户，可以传递--global选项让git读写此文件，这会对系统上所有的仓库生效。
3. `.git/config`：当前使用仓库的git目录中的config文件。针对该仓库可以传递--local选项让git强制读写此文件，虽然默认情况下用的就是它。

可以通过`git config --list --show-origin`查看所有的配置以及它们所在的文件：

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506151827739.png" alt="image-20250615182754678" style="zoom:67%;" />

在使用git之前还要设置一下用户名和邮件地址，这些信息会写入到git的每一次提交中(使用--global选项对于当前系统就只需要配置一次了）：

`git config --global user.name "sxt"`

`git config --global user.email 1264803708@qq.com`

可以使用`git config --list`列出所有git能找到的配置，或者使用`git config <key>`来检查git的某一项配置：

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506151832888.png" alt="image-20250615183210892" style="zoom:67%;" />

![image-20250615183347870](https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506151833911.png)

如果想获取git的帮助信息，可以使用`git help <verb>`命令，或者使用`-h`选项获得更简明的帮助输出，如：

`git help config` / `git config -h`




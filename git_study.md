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

## 远程仓库

远程仓库指托管在互联网或其他网络中的项目的版本库。

1. 查看远程仓库可以使用`git remote`命令，指定`-v`选项会显示需要读写远程仓库使用的git保存的简写及其对应的url。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506152334537.png" alt="image-20250615233432497" style="zoom: 80%;" />

2. 可以使用命令`git remote add <shortname> <url>`添加一个新的远程git仓库url，同时指定一个简写shortname。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506152354502.png" alt="image-20250615235442436" style="zoom:80%;" />

3. 从远程仓库中获得数据可执行`git fetch <remote>`，这个命令会访问远程仓库，从中拉取所有本地还没有的数据。执行完成后，将拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

4. 推送项目到上游可以执行命令`git push <remote> <branch>`
5. 查看某个远程仓库的更多信息可以使用命令`git remote show <remote>`

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506160001809.png" alt="image-20250616000103760" style="zoom:67%;" />

6. 使用命令`git remote rename <old_remote> <new_remote>`来修改一个远程仓库的简写名；使用命令`git remote remove <remote>`来删除一个远程仓库。

## 打标签和git别名

git可以给仓库历史中的某一个提交打上标签，以示重要，人们也尝尝会使用这个功能来标记发布节点（v1.0、v2.0等）。

1. `git tag`命令可以列出所有已有的标签，如果想使用匹配标签名的通配模式，那么必须要使用`-l / --list`选项。

2. git支持两种标签：轻量标签和附注标签。轻量标签很像一个不会改变的分支即它只是某个特定提交的引用；而附注标签是存储在git数据库中的一个完整对象，它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间，此外还有一个标签信息。

- 附注标签：在运行`tag`命令时指定`-a`选项即可。此外还需要`-m`选项指定一条将会存储在标签中的信息，如果没有为附注标签指定一条信息，git会启动编辑器要求输入信息。通过`git show`命令可以看到标签信息和与之对应的提交信息。
- 轻量标签：本质上是将提交校验和存储到一个文件中，没有保存任何其他信息。创建轻量标签，不需要使用任何选项，直接提供标签名字即可。这时在标签上运行`git show`，将不会看到额外的标签信息，只会显示出提交信息。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506222219789.png" alt="image-20250622221919703" style="zoom:67%;" />

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506222219054.png" alt="image-20250622221939000" style="zoom:67%;" />

3. 也可以对过去的提交打标签，需要在命令的末尾指定提交的校验和（或部分校验和） ==**todo**== 

4. 默认情况下，`git push`命令并不会传送标签到远程仓库服务器上。在创建完标签后必须显式地推送标签到共享服务器上，这个过程就像共享远程分支一样，运行命令`git push origin <tagname>` 。而如果想要一次性推送很多标签，也可以使用带有`--tags`选项的`git push`命令，这将会把所有不在远程仓库服务器上的标签全部传送到那里。并且这一命令不会区分轻量标签和附注标签，没有简单的选项可以实现只选择推送一种标签。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506222228361.png" alt="image-20250622222830294" style="zoom:67%;" />

5. 要删除掉本地仓库上的标签，可以使用命令`git tag -d <tagname>`，但这个命令并不会从远程仓库中移除这个标签。必须使用`git push <remote> :refs/tags/<tagname>`来更新远程仓库，这个操作的含义是将冒号前的控制推送到远程标签名，从而高效地删除它。也可以使用命令`git push origin --delete <tagname>`来更直观的删除远程标签。

<img src="https://cdn.jsdelivr.net/gh/qc-sxt/note-image@main/img/202506222233098.png" alt="image-20250622223319045" style="zoom:67%;" />

6. 可以通过`git config`文件来为每一个命令创建别名，使得git的操作体验更加的简单、熟悉。比如：

```bash
git config --global alias.co checkout  # 为checkout创建别名co
git config --global alias.br branch 
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --' # 取消暂存别名
git config --global alias.last 'log -1 HEAD' # 查看最后一次提交
```


# Git

## Git 简介

Git 是一个开源的**分布式版本控制系统**，用于敏捷高效地处理任何或小或大的项目。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

### 集中式 VS 分布式

集中式版本控制系统，版本库是集中存放在中央服务器的，我们每次需要从中央服务器下载到最新的版本到本地，然后干完活再将其推送给中央服务器。

![central-repo](./git.assets/l.jpeg)

而分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。

和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

![distributed-repo](./git.assets/l-20221010143259736.jpeg)



## Git 安装配置

### Windows 平台上安装

在 [Git 官网](https://git-scm.com/download/win) 上下载好 Git 的安装包直接安装即可。

我们可以将默认分支的名称设为 main，这样以后我们每次在本地创建版本库时，默认的分支名称就是 main。也可以不进行修改，选择第一个选项，则默认分支名称为 master。

![image-20221010150527437](./git.assets/image-20221010150527437.png)

其他的设置保持默认即可。

安装完成后，在命令行中输入命令：

```
git -v
```

若输出如图信息，则说明 Git 安装成功。

![image-20221010151014688](./git.assets/image-20221010151014688.png)



### macOS 平台上安装

在 Mac 上安装 Git 有许多方法，这边建议安装 Xcode Command Line Tools。

打开终端，输入如下命令：

```
xcode-select --install
```

确认是否安装成功：

```
xcode-select --version
```

如果输出版本信息，则证明 Xcode Command Line Tools 安装成功，接下来我们可以检验 Git 是否安装成功。

检验 Git 是否安装成功，在终端输入：

```
git -v
```

若输出信息如下图所示，则证明 Git 安装成功。

![image-20221010145612916](./git.assets/image-20221010145612916.png)

### Git 配置

#### Git 配置文件

我们使用 `git config` 命令来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- `/etc/gitconfig` 文件：对所有用户都普遍适用的配置。若使用 `git config` 时添加 `--system` 选项，读写的就是这个文件。
- `~/.gitconfig` 文件：用户目录下的配置文件，该配置只针对该用户。若使用 `git config` 时添加 `--global` 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。

每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

在 Windows 系统上，Git 会找寻当前用户主目录下的 `.gitconfig` 文件。

#### 配置用户信息

在使用 Git 前，我们还需要进行最后一步设置，在命令行输入：

```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

上述命令用于配置个人的用户名称和电子邮件地址。

如果用了 `--global` 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。

如果要在某个特定的项目中使用其他名字或者电子邮件，只要去掉 `--global` 选项，重新在那个特定的项目目录下配置即可，新的设定保存在项目的 `.git/config` 文件里。

> 注意在这边配置的用户名称和电子邮件地址与我们之后要使用的 GitHub 的用户名和电子邮件没有任何关系。

#### 查看配置信息

我们可以通过如下命令查看已有的配置信息：

```
git config --list
```

也可以直接查阅某个环境变量，只要把特定的名字跟在后面即可，像这样：

```
git config user.name
```

有时候会看到重复的变量名，那就说明它们来自不同的配置文件（比如 `/etc/gitconfig` 和 `~/.gitconfig`），不过最终 Git 实际采用的是最后一个。这些配置我们也可以在先前提到的三个地方的配置文件中看到。



## Git 基本操作

### 创建版本库

什么是版本库呢？版本库又名仓库，英文名 **repository**，可以简单理解成一个目录，这个目录里面的所有文件都可以被 Git 管理起来，每个文件的修改、删除，Git 都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

Git 使用 `git init` 命令来初始化版本库。我们首先去到我们想追踪的目录下，执行 `git init` 命令。

在执行完成 `git init` 命令后，会在该目录下生成一个 `.git` 目录，这个目录是 Git 来跟踪管理版本库的，切记不要随意修改里面的内容。

同样，我们也可以指定目录作为 Git 版本库：

```
git init new-repository
```

该命令执行后，会初始化 `new-repository` 作为 Git 版本库，如果目录不存在则会创建目录。



### 添加文件到暂存区

我们现在切换到 `new-repository` 目录下，创建一个 `README.md` 文件，内容如下：

```
Git is a version control system.
```

完成后，我们在命令行输入 `git status` 命令，该命令可以查看当前版本库的状态：

![image-20221010225223599](./git.assets/image-20221010225223599.png)

如图显示，在分支 `main` 中，我们还未有任何提交，并且有一个 `README.md` 文件还未被追踪。同时，它提示我们可以使用 `git add <file>` 命令，将文件添加到暂存区。

让我们将 `README.md` 添加到暂存区：

```
git add README.md
```

该命令执行后，不会输出任何消息，但不必担心，这说明我们添加成功了。

同时，我们可以一次性添加多个文件到暂存区：

```
git add [file1] [file2] ...
```

添加指定目录到暂存区，包括其子目录：

```
git add [dir]
```

添加当前目录下的所有文件到暂存区：

```
git add .
```



### 将暂存区内容提交到本地仓库

在将 `README.md` 添加到暂存区后，我们可以使用 `git commit` 命令，将其提交到仓库中。

 `git commit` 命令使用方法如下（`commit message` 是本次提交的说明）：

```
git commit -m "commit message" [file]
```

我们执行如下命令，将 `README.md` 文件提交到我们的本地仓库中：

```
git commit -m "Add README" README.md
```

输出消息如下说明我们提交成功：

![image-20221010230223482](./git.assets/image-20221010230223482.png)

`-a` 参数可以让我们在修改文件后，不需要执行 `git add` 命令，直接将修改后的文件提交到本地仓库：

```
git commit -am "commit message" [file]
```



### 查看本地库状态

`git status` 命令用于查看在你上次提交之后是否有对文件进行再次修改。

如图显示，我们修改了 `README.md` 文件，但还未将其添加到暂存区。

![image-20221011084949726](./git.assets/image-20221011084949726.png)

通常我们使用 `-s` 参数来获得简短的输出结果：

```
git status -s
```

![image-20221011085123901](./git.assets/image-20221011085123901.png)



### 比较文件前后差异

首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如 TXT 文件，网页，所有的程序代码等等，Git 也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从 100KB 改成了 120KB，但到底改了啥，版本控制系统不知道，也没法知道。

不幸的是，Microsoft 的 Word 格式是二进制格式，因此，版本控制系统是没法跟踪 Word 文件的改动的。

我们使用 `git diff` 命令比较文件前后的差异：

![image-20221010231805334](./git.assets/image-20221010231805334.png)

如图所示，可以看出，我们在 `README.md` 文件中，新增了一行：

```
Git is a free software.
```



### 查看历史提交记录

在 Git 中，我们用 `git log` 命令查看历史提交记录：

![image-20221011085344324](./git.assets/image-20221011085344324.png)

`git log` 命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`Append GPL`，上一次是 `Add a new line`，最早的一次是 `Add README`。

我们可以使用 `--pretty=oneline` 参数，将每个提交控制在一行显示：

```
git log --pretty=oneline
```

![image-20221011085707656](./git.assets/image-20221011085707656.png)

我们看到的一大串类似 `0fd9bc72...` 的是 `commit id`，也就是版本号。

每提交一个新版本，实际上 Git 就会把它们自动串成一条时间线。大致如下图所示，具体的我们之后再进行解释。

![image-20221011092919570](./git.assets/image-20221011092919570.png)



### 版本回退

在 Git 中，用 `HEAD` 表示当前版本，也就是最新的提交 `0fd9bc72...`（注意我的提交ID和你的肯定不一样），上一个版本就是 `HEAD^`，上上个版本就是 `HEAD^^`，当然往上100个版本写100个 `^` 比较容易数不过来，所以写成 `HEAD~100`。

现在我们将版本回退到 `Add a new line`：

```
git reset --hard HEAD^
```

![image-20221011100159633](./git.assets/image-20221011100159633.png)

根据输出提示，我们已经回退到 `Add a new line`，同时，查看一下现在的版本库状态可以发现，最新的 `Append GPL` 版本不见了。

我们使用 `--hard` 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有提交信息。

如果我们想要回到最新的版本该怎么办呢？这时候我们可以使用命令 `git reflog`，该命令记录了你的每次命令：

![image-20221011100917539](./git.assets/image-20221011100917539.png)

我们可以看到最新版本的 `commit id` 为 `0fd9bc7`，所以我们便可以执行命令：

```
git reset --hard 0fd9bc7
```

我们就回到了最新的版本。版本号没必要写全，前几位就可以了，Git 会自动去找。当然也不能只写前一两位，因为 Git 可能会找到多个版本号，就无法确定是哪一个了。



### 撤销修改

现在，假设你在 `README.md` 文件中添加了一行：

```
I'm going to propose to my girlfriend.
```

你突然发现，你想给你女朋友一个惊喜，所以这句话绝对不能让她看到。

使用 `git status` 查看一下：

![image-20221011104103778](./git.assets/image-20221011104103778.png)

Git 告诉你，可以使用 `git restore <file>` 丢弃工作区的修改：

```
git restore README.md
```

如果你已经将文件添加到了暂存区：

![image-20221011104535860](./git.assets/image-20221011104535860.png)

这时候 Git 也告诉我们，可以使用 `git restore --staged <file>` 将文件从暂存区删除（这并不会丢弃工作区的修改）：

```
git restore --staged README.md
```

![image-20221011105209801](./git.assets/image-20221011105209801.png)

接着，我们就可以继续使用 `git restore <file>` 命令，丢弃工作区的修改。



### 删除文件

我们新建一个 `test.md` 文件，并将其提交到版本库，然后将其删除，使用 `git status` 查看版本库状态：

![image-20221011110903091](./git.assets/image-20221011110903091.png)

这时候 Git 告诉你，你删除了 `test.md` 文件，现在有两个选择，一是确实要从版本库中删除该文件，那就用命令 `git rm` 删掉，并且 `git commit`：

```
$ git rm test.md
$ git commit -m "Delete test"
```

另一种情况是删错了，这时候 Git 也提示我们，可以使用我们之前提到的 `git restore <file>` 取消工作区的修改：

```
git restore test.md
```



## Git 分支管理

几乎每一种版本控制系统都以某种形式支持分支，一个分支代表一条独立的开发线。使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。

![img](./git.assets/git-brance.svg)

在版本回退里，你已经知道，每次提交，Git 都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在 Git 里，这个分支叫主分支，即 `main` 分支。

Git 使用 `HEAD` 来指向我们当前所处的分支，当前，`HEAD` 指向的就是 `main` 分支，而 `main` 则指向 `commit`。

一开始的时候，`main` 分支是一条线，Git 用 `main` 指向最新的提交，再用 `HEAD` 指向 `main`，就能确定当前分支，以及当前分支的提交点：

![image-20221011092919570](./git.assets/image-20221011092919570.png)

每次提交，`main` 分支都会向前移动一步，即指向最新的提交，这样，随着你不断提交，`main` 分支的线也越来越长。



### 创建与合并分支

接下来，我们创建一个新的分支 `dev`：

```
git branch dev
```

利用 `git branch -v` 命令查看当前分支情况：

![image-20221011161028993](./git.assets/image-20221011161028993.png)

输出信息说明我们当前处在 `main` 分支上。

我们使用命令 `git switch` 切换分支：

```
git switch dev
```

![image-20221011161333240](./git.assets/image-20221011161333240.png)

从输出信息可以看到，我们当前已经处在 `dev` 分支上了。

当我们创建新的分支 `dev` 时，Git 新建了一个指针叫 `dev`，指向 `main` 相同的提交。我们切换分支时，Git 把 `HEAD` 指向 `dev`，就表示我们当前处在 `dev` 分支上：

![image-20221011155832286](./git.assets/image-20221011155832286.png)

从现在开始，对工作区的修改和提交就是针对 `dev` 分支了。

我们对 `README.md` 文件进行修改，然后提交。提交后，`dev` 指针往前移动一步，而 `main` 指针不变：

![CB6E4266-E0AE-4F44-9B01-B060340283C1](./git.assets/CB6E4266-E0AE-4F44-9B01-B060340283C1.png)

现在我们切换回 `main` 分支：

![image-20221011162750960](./git.assets/image-20221011162750960.png)

我们现在查看 `README.md` 文件，可以看到我们刚才添加的内容并不存在于 `main` 分支上。

假如我们在 `dev` 上的工作完成了，就可以把 `dev` 合并到 `main` 上。在 `main` 分支上，执行：

```
git merge dev
```

![image-20221011163126676](./git.assets/image-20221011163126676.png)

`git merge` 命令用于合并指定分支到当前分支。合并后，再查看 `README.md` 的内容，就可以看到，和 `dev` 分支的最新提交是完全一样的。

注意输出的信息，`Fast-forward` 是一种合并的方式，我们之后会提到其他的合并方式。

合并完成后，我们就可以删除 `dev` 分支了：

```
git branch -d dev
```

如果我们要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。



### 解决合并冲突

我们重新创建一个分支 `dev`，给 `README.md` 文件在第四行添加如下行，并将其提交到版本库。

```
Branch merge conflict test.
```

切换回 `main` 分支。在 `main` 分支上，给  `README.md` 文件在第四行添加如下行，并将其提交到版本库。

```
This is main branch.
```

这时候在 `dev` 和 `main` 分支上都有了新的提交，变成了这样：

![32504071-5906-4977-9C08-A75AA5764B10](./git.assets/32504071-5906-4977-9C08-A75AA5764B10.png)

接着我们试着使用命令 `git merge dev` 合并分支：

![image-20221011164536372](./git.assets/image-20221011164536372.png)

输出信息提示我们 `README.md` 文件合并产生了冲突，必须手动解决冲突后再提交。我们这时候查看 `README.md` 的内容：

![image-20221011165328409](./git.assets/image-20221011165328409.png)

Git用 `<<<<<<<`，`=======`，`>>>>>>>` 分隔出不同分支的内容，我们修改文件内容为如下后保存：

![image-20221011165451552](./git.assets/image-20221011165451552.png)

然后将其提交到版本库，这样我们就成功解决了合并冲突，并将 `dev` 分支与 `main` 分支进行了合并。

现在分支变成了下图所示：

![12FF618C-5133-4D94-B39A-2F84193321D5](./git.assets/12FF618C-5133-4D94-B39A-2F84193321D5.png)

在合并好分支后，我们便可以将 `dev` 分支删除了。



### 贮藏正在进行的工作

现在，假设我们要修复一个 bug，很自然的，我们想到要创建一个分支 `issue` 来修复它，但是我们当前在 `dev` 分支下的工作还没有提交，并不是我们不想提交，但是工作只进行到一半，我们没办法提交。

我们可以在未提交的情况下尝试切换到 `main` 分支：

![image-20221011232536146](./git.assets/image-20221011232536146.png)

如图，切换分支失败，我们必须提交我们的修改或者贮藏它们。

![image-20221011232728023](./git.assets/image-20221011232728023.png)

现在，用 `git status` 查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复 bug。

在我们将 bug 修复完成后，我们要继续我们在 `dev` 分支上的工作了。使用 `git stash list` 命令查看我们贮藏的工作现场：

![image-20221011233011443](./git.assets/image-20221011233011443.png)

现在，我们想恢复我们的工作现场，有两个办法：

* 一是用 `git stash apply` 恢复，但是恢复后，stash 内容并不删除，需要用 `git stash drop` 来删除：

![image-20221012080738094](./git.assets/image-20221012080738094.png)

* 另一种方式是用 `git stash pop`，恢复的同时把 stash 内容也删了

![image-20221012080923507](./git.assets/image-20221012080923507.png)

在 `main` 分支上修复好 bug 后，我们发现，`dev` 分支也是从 `main` 分支分出来的，所以这个 bug 也存在于 `dev` 分支，那么我们有没有什么办法，能不重复修复 bug，而是将我们在 `main` 上的修改复制到 `dev` 分支上呢？注意我们并不是把整个 `main` 分支 merge 过来。

我们使用 `git cherry-pick` 命令，复制一个特定的提交到当前分支：

```
git cherry-pick commit-id
```

Git 自动给 `dev` 分支做了一次提交，这两次的提交是不同的（`commit-id` 不同），用 `git cherry-pick`，我们就不需要在 `dev` 分支上手动再把修 bug 的过程重复一遍。



## 使用 GitHub

### 创建远程库

我们在 Github 上创建一个新的仓库，将其命名为 `new-repository`，其他保持默认配置：

![image-20221012082943344](./git.assets/image-20221012082943344.png)

目前，在 GitHub 上的这个 `new-repository` 仓库还是空的，GitHub 告诉我们，可以在本地创建一个新的仓库，并将其推送到当前仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到 GitHub 仓库。

![image-20221012083149345](./git.assets/image-20221012083149345.png)



### 推送到远程库

我们先展示将我们本地的 `new-repository` 仓库推送到 Github，根据提示，我们可以在本地的 `new-repository` 仓库下运行命令：

```
git remote add origin https://github.com/Airinfinite/new-repository.git
```

添加完成后，我们运行 `git remote -v` 命令查看我们添加的远程库信息：

![image-20221012083802812](./git.assets/image-20221012083802812.png)

`origin` 是远程库的别名，我们也可以改成别的，但一般不这样做。

现在，我们可以尝试将本地仓库的 `main` 分支推送到 GitHub 上：

```
git push origin main
```

![image-20221012084804678](./git.assets/image-20221012084804678.png)

我们需要输入我们 Github 的用户名和密码，用来验证我们是否有推送的权限。

在我们输入完用户名和密码后，输出信息告诉我们，在2021年8月13号，使用密码验证登录已经不再受支持，所以我们必须使用其他的验证方法。在这里，我只展示使用 `token` 验证。

在 GitHub 上，去到 `Settings/Developer settings`，找到 `Personal access tokens`。

![image-20221012085320164](./git.assets/image-20221012085320164.png)

我们选择 `Generate new token`：

![image-20221012085507376](./git.assets/image-20221012085507376.png)

你可以在 `Note` 输入框中说明你这个 `token` 是用来做什么的。

`Expiration` 是这个 `token` 的失效时间。

`scopes` 是这个 `token` 的权限，我们要将 `repo` 权限全部勾选，然后点击 `Generate token`。

这样我们就创建了一个 `token`，注意要将其保存好，因为 `token` 只会在新创建的时候可以查看，如果没有保存，我们只能将其删除，重新创建一个新的 `token`。

我们可以使用 `git remote --help` 查看该命令的帮助文档：

![image-20221012090149775](./git.assets/image-20221012090149775.png)

我们可以在帮助文档中看到，我们可以使用 `git remote set-url <name> <newurl> [<oldurl]` 修改 `origin` 的链接。

我们按照如下方法重新拼接一个 URL：

```
https://oauth2:token@github.com/username/repositoryname.git
```

然后将 `origin` 的链接修改为我们拼接的 URL，我们可以使用 `git remote -v` 命令查看修改是否成功。

在修改完成后，我们可以再次尝试使用 `git push origin main`，将 `main` 分支推送到远程仓库：

![image-20221012091153227](./git.assets/image-20221012091153227.png)

推送成功！这时候我们到 GitHub 仓库下，就可以看到我们刚才推送过去的文件了。



### 从远程库克隆

在上一节，我们展示了在先有本地库后有远程库的情况下，如何关联远程库。现在，假设我们从零开发，那么最好的方式是先创建远程库，然后从远程库克隆。

去到我们新创建的远程库中，复制 HTTPS 链接，然后按照上一节中的方法拼接新的 URL，这样在我们之后将本地库推送到远程库时，就不必再次进行修改。

![image-20221012082750016](./git.assets/image-20221012082750016.png)

在终端运行命令 `git clone` 克隆远程库：

```
git clone url
```



### 推送分支

在前边，我们讲到如何将本地库推送到远程库，那时候我们其实是将本地库的 `main` 分支推送到远程库的 `main` 分支。

我们利用 `git push origin main` 命令，把本地的 `main` 分支推送到远程库对应的 `main` 分支上。

如果要推送其他分支，比如 `dev`，就改成：

```
git push origin dev
```

推送成功后，我们就可以在远程库中看到 `dev` 分支了。

![image-20221012095805897](./git.assets/image-20221012095805897.png)



### 抓取分支

当我们从远程库克隆时，默认情况下，只会克隆远程库的 `main` 分支。不信可以用 `git branch` 命令看看：

![image-20221012100309715](./git.assets/image-20221012100309715.png)

现在，我们想将 `dev` 分支也克隆到本地来，于是我们可以使用命令：

```
git switch -c dev origin/dev
```

该命令在本地创建了 `dev` 分支，并切换到了 `dev` 分支。

![image-20221012100430221](./git.assets/image-20221012100430221.png)

我们现在就可以在 `dev` 分支上进行修改了。

当我们修改完成后，想将其推送到远程库时，碰巧你的小伙伴也将他修改过的 `dev` 分支推送到了远程库，如果这时候我们尝试推送，会收到如下报错信息：

![image-20221012101258233](./git.assets/image-20221012101258233.png)

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git 已经提示我们，先用 `git pull` 把最新的提交从 `origin/dev` 抓下来，然后，在本地合并，解决冲突，再推送。

![image-20221012101544512](./git.assets/image-20221012101544512.png)

我们执行 `git pull` 命令也失败了，它提示说，我们有不同的分支，需要指定如何去分辨它们。同时，它也提示我们如果要进行合并的话可以使用命令 `git config pull.rebase false`。

我们执行命令后，再执行 `git pull`：

![image-20221012102407525](./git.assets/image-20221012102407525.png)

可以看到，它提示我们说合并有冲突，这时候我们就可以利用之前学到的解决冲突的方法来解决它。

解决完冲突后，记得将其提交到版本库中。然后我们就可以尝试将其推送到远程库了：

![image-20221012102634194](./git.assets/image-20221012102634194.png)

非常完美，推送成功！



## 标签管理

如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 `git tag` 给它打上标签。



### 创建标签

在 Git 中打标签非常简单，我们只需要切换到需要打标签的分支上，然后执行命令 `git tag <name>` 就可以打一个新的标签。

默认标签是打在最新提交的 `commit` 上的，我们也可以指定 `commit` 版本：

```
git tag name commit-id
```

我们可以使用 `git show <tagname>` 查看标签的具体信息，同时也可以使用 `git tag` 查看所有的标签。



### 操作标签

如果标签打错了，也可以删除：

```
git tag -d name
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令 `git push origin <tagname>`：

![image-20221012103532372](./git.assets/image-20221012103532372.png)

或者也可以一次性推送所有尚未推送到远程的标签：

```
git push origin --tags
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```
git tag -d name
```

然后，从远程删除。删除命令也是 `push`，但是格式如下：

```
git push origin :refs/tags/name
```

要看看是否真的从远程库删除了标签，可以登陆 GitHub 查看。

# Git

## Git 配置

###  配置用户名和邮箱

因 Git 是分布式版本控制系统，需要填写用户名和邮箱作为一个标识。

```git shell
$ git config --global user.name "xxx"
$ git config --global user.email "xxxx@xxx.com"
```

注：git config --global 参数，表示目前这台机器上所有的 Git 仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和邮箱。

### 检查配置信息

利用：

```git shell
$ git config --list
```

来列出 git 所有配置；

也可以通过：

```git shell
$ git config xxx.xx
```

来检查 git 的某一项(xxx.xx)配置。

### 获取帮助

若使用 git 时需要获取帮助，可以使用以下三种指令(任意一种)找到 git 指令使用手册：

```git shell
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

若想获得 config 指令的手册，可执行：

```git shell
$ git help config
```

### 创建版本库

#### 方法一：直接在任意目录下创建版本库，目录右键选择 git bash

运行指令：

```git shell
$ git init
```

出现 xxx/ .git/  就被管理起来了。

#### 方法二：cd 到 git 工作目录， mkdir 创建版本库， pwk 查看，运行命令 git init

### 本地关联 GitHub (SSH 连接配置)

1. 拥有一个 GitHub 账号；
2. 创建 SSH Key；

```git shell
$ ssh-keygen -t rsa -C xxx.@xx.com
```

接下来会告诉生成文件位置；回车，接着设置每次更新仓库的密码；最后会生成 id_rsa 私钥和 id_rsa.pub 公钥文件，在用户主目录下 .ssh 文件下。

3. 登录 GitHub；

settings --> SSH Keys --> Add SSH Key --> new

4. 填上任意 title，Key 文本框黏贴 id_rsa.pub 文件的内容，点击 Add Key 。

### 与 GitHub 仓库连接

#### 方式一：在本地建仓并初始化，后传到远程仓库(GitHub)上的未初始化的仓库

1. 在 GitHub 上创立远程仓库：

   登录 GitHub 账户，在主页创立一个新仓库 (New repository)；

   起好仓库名，创建即可，不用勾选任何东西。创建好后复制 SSH 地址(也可以是 https://协议，以下不再赘述)。

2. 在电脑上选择一个文件夹作为自己的本地仓库。

   输入指令：

   ```git shell
   $ git init
   ```

   文件夹中会多出一个 .git 文件夹(隐藏)。此时本地仓库已经初始化，可以将自己的项目拖进来。

3. 将创建好的本地仓库与远程仓库连接起来。

   输入指令：

   ```git shell
   $ git add .
   ```

   将工作区的所有文件添加到暂存区。

   输入指令：

   ```git shell
   $ git commit -m "本次提交的注释"
   ```

   将暂存区的文件提交到本地仓库

   ```git shell
   $ git remote add origin git@github.com:xx/xx/xx.git
   ```

   origin 后面是复制的需要链接的GitHub仓库(上面复制过)的 SSH 地址。这个操作让本地仓库和远程仓库建立联系。

   ```git shell
   $ git push origin X
   ```

   将本地仓库中的内容 push 到远程仓库中，输入密码(输出 SSH 密钥时设置)。

4. 刷新 GitHub 即可看到上传的项目。

##### git remote add origin 时提示：error: remote origin already exists.

解决方法：

1. 删除远程 git 仓库

   ```git shell
   $ git remote rm origin
   ```

2. 重新添加远程 git 仓库

   ```git shell
   $ git remote add origin git@github.com:xx/xx/xx.git
   ```

3. 若执行还报错，则可手动修改 gitconfig 文件的内容。

   ```git shell
   $ vi .git/config
   ```

4. 删除 [remote "origin"] 行即可。

##### 配置 git 失败 can't be established

在输入下面指令后，

```git shell
$ ssh -T git@github.com
```

报错：

```git shell
The authenticity of host 'github.com (192.30.255.112)' can't be established. RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8. Are you sure you want to continue connecting (yes/no)?
```

解决方法：

在该处回答 yes ，会出现 ：

```git shell
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

原因：

配置时，虽然生成了 rsa 两个文件吗、，但并没有生成 hosts 文件，当回答 yes 后会自动生成所需要的 hosts 文件。

#### 方式二：在 GitHub 上建仓并初始化，再将 GitHub 上的仓库克隆到本地仓库。

1. 在 GitHub 上 Create a new repository：勾选 Add a README file ，

   创建后复制该仓库的 SSH 地址(绿色下拉菜单)；

2. 在本地选择一文件夹右键打开 Git Bash Here ，

   输入指令：

   ```git shell
   $ git clone git@github.com:xxxx/xxx.git
   ```

   即将远程仓库克隆，被克隆的文件夹里会有一个 .git 文件夹，该文件夹即为本地的一 git 仓库。

3. 将项目上传至 GitHub 远程仓库。

### 更新仓库

每次完成阶段性目标后，提交更新到仓库。

#### 文件状态

在工作目录下的每个文件都会有两种状态：已跟踪或未跟踪。

+ 已跟踪文件：被纳入了版本控制的文件，在上次快照被记录；在工作一段时间后，该文件状态可能处于未修改、已修改或已放入暂存区。
+ 未跟踪文件(Untracked files)：工作目录中的除已跟踪文件外的所有文件。既不存在于上次快照记录中，也未被放入暂存区。

初次克隆某个仓库时，工作目录中的所有文件都属于**已跟踪**文件，并处于**未修改**状态。当上次提交后对文件进行了修改，Git 将会把该文件标记为**已修改文件**。需要逐步将修改过的文件放入暂存区，后提交所有暂存的修改，进行更新。

#### 检查文件状态

```git shell
$ git status
```

#### git add 指令

```git shell
$ git add xxx.xx
```

##### 跟踪新文件

若在工作目录中创建了新文件，可用 git add 开始跟踪该文件(xxx.xx)。

此时运行 git status 指令会发现该文件状态会发生改变。

git add 指令使用文件或目录的路径作为参数；若参数是**目录的路径**，该命令将**递归地跟踪该目录下的所有文件**。

##### 暂存已修改文件

当一已被跟踪的文件(xxx.xx)每次被修改后，使用 git add 把最新版本添加到暂存区可提交更新，否则不会提交本次更新到仓库中。

git add 还可以把有冲突的文件标记为已解决状态，可理解为“添加内容到下一次提交中”。

#### 查看已暂存和未暂存的修改

可利用 git diff 指令比较工作目录中当前文件和暂存区域快照之间的差异，也就是修改后还未被暂存起来的变化内容。

```git shell
$ git diff
```

但该指令本身**只显示尚未暂存的改动**，而不是自上次提交以来所做的所有改动。

```git shell
$ git diff -cached
```

查看已暂存的将要添加到下次提交里的内容。

```git shell
$ git diff -staged
```

有同样的功能，但只有 git 1.6.1 及更高版本才允许使用。

#### git commit 指令

##### 提交更新

 git add 之后，即可提交到仓库。最好**每次准备提交前，都检查一下修改过的文件是不是都已被暂存了**，然后再 git commit 。否则容易漏掉那些未被暂存起来的变化，而这些修改过的文件只会保留再本地磁盘。

```git shell
$ git commit -m "xxxx"
```

利用 -m 选项添加提交信息。

##### 跳过使用暂存区

```git shell
$ git commit -a -m "xxx"
```

使用 -a 选项，git 会自动把所有已经跟踪过的文件暂存起来一并提交，这样便会跳过 git add 步骤。

### 移除文件

#### 从已跟踪文件清单中(暂存区)移除

```git shell
$ git rm <finame>
```

从 git 暂存区中移除某个文件，并连带从工作目录中删除指定的文件，这样该文件就不会出现再未跟踪文件清单中了。
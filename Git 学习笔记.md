# 准备使用Git:
    设置好用户名 和 Email
<mark>这使Git记录并追踪历史记录</mark>

**所有的Git命令都 从 git 开始：比如
```bash
    $ git -v   显示git版本信息
    $ git config --list 显示配置信息列表
```
ps:
    clear 命令可以清空当前控制台（命令解释器）的屏幕

设置用户名和密码
```bash
    $ git config --global user.name "My Name"

    $ git config --global user.email MyEmailAddress@xx.com //这里没有 ""

```
如果想做修改，就重新输入上述命令行修改值即可

# 如何更新Git
```bash
$ git update-git-for-windows  // 最后的这个-windows，如果是其他平台，就替换成其他平台的名字就可以更新了
```

# 习惯使用 git status git log等命令查看状态和历史记录


# 使用Git的常规流程
1 首先在指定目录创建一个Repository
```bash
$ git init # 在当前目录创建一个Repository(生成.git文件夹)
$ git initi <文件夹或目录名称> # 在当前目录创建一个指定名称的文件夹并在其中创建repository           
```
2 创建.gitignore 文件，并输入要规避的文件类型
3 <mark>stage</mark> 创建的 .gitignore文件
```bash
$ git add <文件名> 
```
4 <mark>commit</mark> 刚stage的 .gitignore文件

注意：
    *当commit命令执行之后，会弹出一个在安装时或手动修改的默认的git编辑器，比如notepad。 会要求输入 关于Commit 的信息。可以在第一行输入标题，然后空一行输入一段简短的描述。建议手动换行，长度参考commit信息界面用于解释描述的提示，否则git会自动换行，影响未来阅读*

5 在进入正式的生产过程中，建议现在其他的branch上进行工作，然后再更新回main branch

```bash
$ git branch # 可以列出当前repo有多少个branch
$ git branch <branch名称> # 可以创建一个branch
$ git checkout  <branch名称> #可以切换到指定的branch上

$ git checkout -b <branch名称> #创建一个新的branch并直接切换，相当于上面两部合成一步

$ git branch -d <branch名称> # 删除一个branch，但是如果分支有变化但是没有合并(合并到main或其他分支)，是不能删除的。 强行删除的话需要使用-D
```

<mark>每当使用`git branch`创建一个分支时，这个分支的进度是和当前的 main branch是一致的</mark>

6 在分支上做修改后，合并到其他分支。
注意：
    *对于git来说，重命名一个被track的文件相当于删除了这个我呢见并创建了一个新的文件。注意这点，以免在未来阅读历史信息的时候混淆*
<mark>在分支上add 和 commit 与在任何branch上都一样</mark>

**PS：Git的分支都是独立运作的并追踪变化的。在每次切换branch的时候，假如多干分支都在对同一个文件进行更新，这个文件的会在每次切换的时候被切换，如果用不同的类型名观察更明显**

在操作并的时候，如果是有Main branch和 用于开发其他新功能的分支branch， <mark>推荐先将 main branch 合并到功能性branch上，然后在未来再将功能分支合并回去</mark>。
*一般习惯上，如果分支被合并回了main，就可以删掉这个分支了*

```bash
# 首先切换到 要吸收其他分支的 分支上
$ git merge <被提取的分支名字> # 如果要将分支A合并到当前分支， A就是这里的名字
```

******************截至到这里所有基于本地利用Git维护代码的内容就差不多了**********************

# 基于Github平台使用Git维护代码

## 基础准备
注册账号
创建一个新的Repository

注意：一般情况下，如果已经先有了一个 local的repository 然后又想在Github上创建一个并让它们同步。
并且local的Repo中已经有一些文件比如 .gitignore 文件。在这种情况下创建Github仓库的时候不要点选 Add a README file 和 Add .gitignore
如果没有local Repo, 第一时间先创建Github Repo的话，可以先勾选这两项。

## 将远程repository 复制到或Clone到本地
可以直接在要存放仓库的目录下呼叫terminal然后执行clone命令,即可在该目录位置创建仓库
<mark>注意：
clone的仓库，本质上还是“一个仓库”，相当于原始仓库在另一个地方的映射。仓库的所有权还是原始拥有者。
这与Fork出来的仓库不同。</mark>

```bash
$ git clone <远程仓库地址> # 这里可以使用HTTP的形式，通过github 仓库界面上code菜单中的选项获得
                          # 仓库的名字和后缀名是 <仓库名>.git
```

## 在本地修改后，如果确认修改没有问题。可以将本地修改的内容与远程仓库同步
使用 pull 和 push组合: 
pull用来从远程仓库拉回文件， 当本地仓库已经超前远程仓库时，这个命令可以用来作为push之前的验证操作。
push用来将本地文件推到远程仓库，以达成同步。

```bash
$ git pull  # 完整的命令应该是 git pull origin <索要文件的仓库名> 
            # 但是如果已经clone过了，并且是本地main 对 远程 main的话，可以直接 git pull 即可。
```

```bash
$ git push # 同样的，如果已经clone了，并且是本地main对远程main， 直接 git push 即可
```

<mark>注意：
    当执行了clone之后，git会自动生成一个link将远程仓库和本地仓库的main branch彼此进行关联。
    所以当二者之间存在差异是，status会进行提示。
    但是如果工作在一个分支上，在进行status的时候就不会看到这个**差异**
    因为这个link并不是在远程main和本地分支上的。
    避免造成迷惑。
</mark>


# 一些应该保持的实践习惯 也算是 流程
1 创建仓库后第一件事，要先设置好 .gitignore 文件
2 在每次 stage 文件之后， 要先检查一下之后再去commit。如果发现有不需要commit的文件，要记得将它撤回staging的状态
3 在准备更新或者处理问题的时候，永远先 **git pull** 确保在仓库main branch最后的状态下进行 
4 然后，为了要更新或处理的问题创建一个新的branch,给这个branch起一个与目的相符的名字
5 在分支上完成工作后，先进行 将本地main merge到当前分支的操作，以确保本地main是最新的状态，并且合并到分支之后，当前分支是基于本地main的最新的工作进度
  然后再将当前分支 merge 回本地main，以确保main与相关问题的分支是同步的
6 然后再将本地仓库 push 到远程仓库( <mark>记得先pull一下确保是基于远程仓库最新状态的</mark>)
7 最后 *删掉* 刚刚用于更新或者处理问题的分支

# 如何合作开发同一个项目(比如开源项目，某个商业组织内的某个项目......)
当多个人需要在同一个项目上做修改时， 可以利用 fork的机制将仓库完整的copy出来。这与clone不同，fork出来的是一个归属于fork请求者的，拥有全部历史信息的，一模一样的仓库。
也就是说，在原拥有者允许的情况下，其他人可以拥有对这个项目做任何修改和更新的权力。**这些修改与更新完全不影响原始项目**，也就是说与原始项目仓库完全无关

fork项目之后，如何布置到本地等操作与正常创建一个新项目基本一致。
有一点可以变通的是：
    一般情况下，为了进行修改，会在clone到本地的仓库上创建一个分支跟新更新。通常会将分支的更新合并回本地的main branch上，
然后在将main push到远程。
    但是为了与他人合作，可以直接将这个branch push到远程。但是因为它是在本地创建的，并没有上游(upstream)的联系，类似远程main与本地main(当clone时git为我们在二者间做好了)
所以需要在远程创建一个呼应的branch，并进行关联。这个操作可以在本地直接做：

```bash
$ git push --set-upstream origin <一致的branch名称> # 回顾一下，origin就是那个clone地址
# 或者
$ git push -u origin <一致的branch名称> # -u参数是 --set-upstream 的快捷方式
```
<mark>PS:如何查看origin</mark>

```bash
$ git origin  #罗列一共多少个 origin就是那个clone地址
$ git origin show # 展示origin详细信息
```
当fork仓库的分支更新到fork仓库的远端后， 就可以发起 Pull Request，请求原项目拥有者，将自己的分支合并到原始项目的代码中。
如果原始仓库拥有者确认合并，他会选择合并选项并确认。这样原始项目就更新了。

后面可以删除branch 和 fork 或者继续维护，完全取决于合作需求.


# 如果在最近一次的commit中存在一些不符合预期的内容如何处理
如果是针对某一个文件，可以修改这个文件，然后正常stage
然后在commit的时候 加上参数 `--amend`

```bash
$ git commit --amend # 修改最近一次的commit,
# 这个命令在有任何被stage的文件时既更新文件也更新消息，在没有文件更新的时候可以只更新 commit 的消息描述，不需要先执行add 即可使用
```

<mark>要注意的是 * * 要在push到远程仓库之前完成这个工作，如果是在push到远程之后再进行amend就没有意义了。
因为很可能已经将有问题的内容传播给了其他写作者</mark>

# 将领先的分支回退到与其他分支相同(比如远程main)
```bash
$ git reset --hard origin/main
```

## 基于这个功能就可以恢复一些误将本地main提前commit的失误
比如想在一个分支上做更新，但是误在main上执行，commit之后才发现问题。这时想回退main的进度。
结合 `git branch newBranchName` 和 `git reset --hard origin/main`即可。

**当然，前提是还没有push到远程仓库**

1 首先创建一个新的branch，因为git的机制本次创建一个branch它的进度适合本地main一样的。
这样这个branch就保存了最新的进度。
2 然后执行`git reset --hard  远程仓库名称/分支名称`命令，就可以把本地的main回退回去

# 如何处理被push到远程仓库的commit

首先在本地使用 `git revert` undo要撤销的 commit
这个操作的本质也相当于一个commit，因为做了这个操作之后git会提示本地仓库领先远程仓库一个commit
```bash
$ git revert <SHA commit identifier> #注意 这个并不改变git history 这个只是undo某个commit
```
然后 `git push` 将这个操作push到远程仓库，这样远程仓库的对应的commit就被撤销了，但是保留了历史记录

# 如何撤销具体某一个文件内的改变
当对一个文件做了更改，在没有commit甚至stage之前，直接使用checkout命令查看这个文件就可以撤销最近一次的修改，恢复到git记录的最近的一个版本上。
<mark>注意：是git记录的，不是文件编辑器存储的</mark>

```bash
$ git checkout <filename>
```
# 如果在创建 ignore 文件之前就记录了不要的文件怎么办？
1 首先将 ignore file进行更新
2 然后使用 `git rm --cached <fileName>`

```bash
$ git rm -- cached <fileName> # 这个命令可以将不想让git跟踪的文件移除git仓库
                              # 但是不会从磁盘上删除，历史记录也会存在
```

# 如果整个仓库都乱了怎么办
在远程仓库没有被弄乱的前提下，直接创建一个新的路径上把远程仓库clone下来。
如果想删除之前仓库的记录，可以在本地把仓库路径内的.git文件夹删掉(<mark>通常是隐藏状态</mark>)


# 个人实战经验
## 在先建立本地放库的前提下，链接远程仓库存放学习笔记没成功
背景：
已经建立的本地仓库并使用，然后在Github上创建远程仓库，名称与本地仓库文件夹同名，但是空格被'-'替换。
模仿视频教程 通过 `git clone <http....git>`，但是发现并没有将两个仓库链接。

然后通过ChatGPT查到 需要调用 `git remote add origin <remote-repository-url> `命令实现**添加远程仓库到本地项目**

另外，在执行push的时候提示被拒绝原因是本地仓库并没有对应的upstream。 这一点是自己忘了
然后根据提示又添加了Upstream.
```bash
$ git push --set-upstream origin <一致的branch名称> # 回顾一下，origin就是那个clone地址
# 或者
$ git push -u origin <一致的branch名称> # -u参数是 --set-upstream 的快捷方式
```

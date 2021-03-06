# Pro Git 读书笔记

## 起步

### Git 简史

Git 基础：那么，简单地说，Git 究竟是怎样的一个系统呢？请注意接下来的内容非常重要，若你理解了 Git 的思想和基本工作原理，用起来就会知其所以然，游刃有余。在开始学习 Git 的时候，请努力分清你对其它版本管理系统的已有认识，如 Subversion 和 Perforce 等；这么做能帮助你使用工具时避免发生混淆。 Git 在保存和对待各种信息的时候与其它版本控制系统有很大差异，尽管操作起来的命令形式非常相近，理解这些差异将有助于防止你使用中的困惑。

* **直接记录快照，而非差异比较**：Git 和其它版本控制系统（包括 Subversion 和近似工具）的主要差别在于 Git 对待数据的方法。概念上来区分，其它大部分系统以文件变更列表的方式存储信息。这类系统（CVS、Subversion、Perforce、Bazaar 等等）将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。Git 不按照以上方式对待或保存数据。反之，Git 更像是把数据看作是对小型文件系统的一组快照。每次你提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个快照流。这是 Git 与几乎所有其它版本控制系统的重要区别。因此 Git 重新考虑了以前每一代版本控制系统延续下来的诸多方面。 Git 更像是一个小型的文件系统，提供了许多以此为基础构建的超强工具，而不只是一个简单的 VCS。
* **近乎所有操作都是本地执行**：在 Git 中的绝大多数操作都只需要访问本地文件和资源，一般不需要来自网络上其它计算机的信息。如果你习惯于所有操作都有网络延时开销的集中式版本控制系统，Git 在这方面会让你感到速度之神赐给了 Git 超凡的能量。因为你在本地磁盘上就有项目的完整历史，所以大部分操作看起来瞬间完成。举个例子，要浏览项目的历史，Git 不需外连到服务器去获取历史，然后再显示出来——它只需直接从本地数据库中读取。你能立即看到项目历史。如果你想查看当前版本与一个月前的版本之间引入的修改，Git 会查找到12个月前的文件做一次本地的差异计算，而不是由远程服务器处理或从远程服务器拉回旧版本文件再来本地处理。这也意味着你离线或者没有 VPN 时，几乎可以进行任何操作。如你在飞机或火车上想做些工作，你能愉快地提交，直到有网络连接时再上传。如你回家后 VPN 客户端不正常，你仍能工作。使用其它系统，做到如此是不可能或很费力的。比如，用 Perforce，你没有连接服务器时几乎不能做什么事；用 Subversion 和 CVS，你能修改文件，但不能向数据库提交修改（因为你的本地数据库离线了）。这看起来不是大问题，但是你可能会惊喜地发现它带来的巨大的不同。
* **Git 保证完整性**：Git 中所有数据在存储前都计算校验和，然后以校验和来引用。这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。这个功能建构在 Git 底层，是构成 Git 哲学不可或缺的部分。若你在传送过程中丢失信息或损坏文件，Git 就能发现。Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。这是一个由 40 个十六进制字符（0-9 和 a-f）组成的字符串，基于 Git 中文件的内容或目录结构计算出来。 SHA-1 哈希看起来是这样：24b9da6552252987aa493b52f8696cd6d3b00373。Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。
* **Git 一般只添加数据**：你执行的 Git 操作，几乎只往 Git 数据库中增加数据。很难让 Git 执行任何不可逆操作，或者让它以任何方式清除数据。同别的 VCS 一样，未提交更新时有可能丢失或弄乱修改的内容；但是一旦你提交快照到 Git 中，就难以再丢失数据，特别是如果你定期的推送数据库到其它仓库的话。这使得我们使用 Git 成为一个安心愉悦的过程，因为我们深知可以尽情做各种尝试，而没有把事情弄糟的危险。

### 获取帮助

若你使用 Git 时需要获取帮助，有三种方法可以找到 Git 命令的使用手册：

```shell
$ git help <verb> 
$ git <verb> --help 
$ man git-<verb>
# 例如，要想获得 config 命令的手册，执行
$ man git-config
```

## Git 基础

### 查看已暂存和未暂存的修改
要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 git diff：此命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --cached 命令。（Git 1.6.1 及更高版本 还允许使用 git diff --staged，效果是相同的，但更好记些。）请注意，git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件后，运行 git diff 后却什么也没有，就是这个原因。

### 撤消操作

* **取消暂存的文件**：git reset HEAD \<file\>...
* **撤消对文件的修改**：git checkout -- [file]    是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失。你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想 要那个文件了，否则不要使用这个命令。

### Git 别名

Git 并不会在你输入部分命令时自动推断出你想要的命令。 如果不想每次都输入完整的 Git 命令，可以通过 git config 文件来轻松地为每一个命令设置一个别名。 这里有一些例子你可以试试：

```shell
$ git config --global alias.co checkout 
$ git config --global alias.br branch 
$ git config --global alias.ci commit 
$ git config --global alias.st status
# 例如，为了解决取消暂存文件的易用性问题，可以向 Git 中 添加你自己的取消暂存别名：
$ git config --global alias.unstage 'reset HEAD --'
# 这会使下面的两个命令等价：
$ git unstage fileA 
$ git reset HEAD -- fileA
# 这样看起来更清楚一些。 通常也会添加一个 last 命令，像这样：
$ git config --global alias.last 'log -1 HEAD'
# 这样，可以轻松地看到最后一次提交。
```

## Git 分支

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来， 以免影响开发主线。 在很多版本控制系统中，这是一个略微低效的过程——常常需要完全创建一个源代码目录的 副本。对于大项目来说，这样的过程会耗费很多时间。

有人把 Git 的分支模型称为它的“必杀技特性”，也正因为这一特性，使得 Git 从众多版本控制系统中脱颖而 出。 为何 Git 的分支模型如此出众呢？ Git 处理分支的方式可谓是难以置信的轻量，创建新分支这一操作几乎能 在瞬间完成，并且在不同分支之间的切换操作也是一样便捷。 与许多其它版本控制系统不同，Git 鼓励在工作流 程中频繁地使用分支与合并，哪怕一天之内进行许多次。 理解和精通这一特性，你便会意识到 Git 是如此的强大 而又独特，并且从此真正改变你的开发方式。

### 分支开发工作流

* **长期分支**：因为 Git 使用简单的三方合并，所以就算在一段较长的时间内，反复把一个分支合并入另一个分支，也不是什 么难事。 也就是说，在整个项目开发周期的不同阶段，你可以同时拥有多个开放的分支；你可以定期地把某些 特性分支合并入其他分支中。许多使用 Git 的开发者都喜欢使用这种方式来工作，比如只在 master 分支上保留完全稳定的代码——有可能仅 仅是已经发布或即将发布的代码。 他们还有一些名为 develop 或者 next 的平行分支，被用来做后续开发或者 测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 master 分支了。 这 样，在确保这些已完成的特性分支（短期分支，比如之前的 iss53 分支）能够通过所有测试，并且不会引入更 多 bug 之后，就可以合并入主干分支中，等待下一次的发布。事实上我们刚才讨论的，是随着你的提交而不断右移的指针。 稳定分支的指针总是在提交历史中落后一大截， 而前沿分支的指针往往比较靠前。通常把他们想象成流水线（work silos）可能更好理解一点，那些经过测试考验的提交会被遴选到更加稳定的流 水线上去。你可以用这种方法维护不同层次的稳定性。 一些大型项目还有一个 proposed（建议） 或 pu: proposed updates（建议更新）分支，它可能因包含一些不成熟的内容而不能进入 next 或者 master 分支。 这么做的 目的是使你的分支具有不同级别的稳定性；当它们具有一定程度的稳定性后，再把它们合并入具有更高级别稳定 性的分支中。 再次强调一下，使用多个长期分支的方法并非必要，但是这么做通常很有帮助，尤其是当你在一 个非常庞大或者复杂的项目中工作时。
* **特性分支**：特性分支对任何规模的项目都适用。 特性分支是一种短期分支，它被用来实现单一特性或其相关工作。 也许你 从来没有在其他的版本控制系统（VCS）上这么做过，因为在那些版本控制系统中创建和合并分支通常很费劲。 然而，在 Git 中一天之内多次创建、使用、合并、删除分支都很常见。你已经在上一节中你创建的 iss53 和 hotfix 特性分支中看到过这种用法。 你在上一节用到的特性分支 （iss53 和 hotfix 分支）中提交了一些更新，并且在它们合并入主干分支之后，你又删除了它们。 这项技术 能使你快速并且完整地进行上下文切换（context-switch）——因为你的工作被分散到不同的流水线中，在不同 的流水线中每个分支都仅与其目标特性相关，因此，在做代码审查之类的工作的时候就能更加容易地看出你做了 哪些改动。 你可以把做出的改动在特性分支中保留几分钟、几天甚至几个月，等它们成熟之后再合并，而不用 在乎它们建立的顺序或工作进度。

### 变基

在 Git 中整合来自不同分支的修改主要有两种方法：merge 以及 rebase。

* **变基的风险**：呃，奇妙的变基也并非完美无缺，要用它得遵守一条准则：**不要对在你的仓库外有副本的分支执行变基。**如果你遵循这条金科玉律，就不会出差错。 否则，人民群众会仇恨你，你的朋友和家人也会嘲笑你，唾弃你。变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。 如果你已经将提 交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 git rebase 命令 重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合，如果接下来你 还要拉取并整合他们修改过的提交，事情就会变得一团糟。
* **变基 vs 合并**：至此，你已在实战中学习了变基和合并的用法，你一定会想问，到底哪种方式更好。 在回答这个问题之前，让 我们退后一步，想讨论一下提交历史到底意味着什么。有一种观点认为，仓库的提交历史即是 记录实际发生过什么。 它是针对历史的文档，本身就有价值，不能乱 改。 从这个角度看来，改变提交历史是一种亵渎，你使用 谎言 掩盖了实际发生过的事情。 如果由合并产生的提 交历史是一团糟怎么办？ 既然事实就是如此，那么这些痕迹就应该被保留下来，让后人能够查阅。另一种观点则正好相反，他们认为提交历史是 项目过程中发生的事。 没人会出版一本书的第一版草稿，软件维 护手册也是需要反复修订才能方便使用。 持这一观点的人会使用 rebase 及 filter-branch 等工具来编写故事，怎 么方便后来的读者就怎么写。现在，让我们回到之前的问题上来，到底合并还是变基好？希望你能明白，这并没有一个简单的答案。 Git 是一 个非常强大的工具，它允许你对提交历史做许多事情，但每个团队、每个项目对此的需求并不相同。 既然你已 经分别学习了两者的用法，相信你能够根据实际情况作出明智的选择。总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变 基操作，这样，你才能享受到两种方式带来的便利。

## 分布式 Git

### 分布式工作流程

同传统的集中式版本控制系统（CVCS）不同，Git 的分布式特性使得开发者间的协作变得更加灵活多样。 在集 中式系统中，每个开发者就像是连接在集线器上的节点，彼此的工作方式大体相像。 而在 Git 中，每个开发者同 时扮演着节点和集线器的角色——也就是说，每个开发者既可以将自己的代码贡献到其他的仓库中，同时也能维 护自己的公开仓库，让其他人可以在其基础上工作并贡献代码。 由此，Git 的分布式协作可以为你的项目和团队 衍生出种种不同的工作流程，接下来的章节会介绍几种利用了 Git 的这种灵活性的常见应用方式。 我们将讨论每 种方式的优点以及可能的缺点；你可以选择使用其中的某一种，或者将它们的特性混合搭配使用。

* **集中式工作流**：集中式系统中通常使用的是单点协作模型——集中式工作流。 一个中心集线器，或者说仓库，可以接受代码， 所有人将自己的工作与之同步。 若干个开发者则作为节点——也就是中心仓库的消费者——并且与其进行同步。这意味着如果两个开发者从中心仓库克隆代码下来，同时作了一些修改，那么只有第一个开发者可以顺利地把数 据推送回共享服务器。 第二个开发者在推送修改之前，必须先将第一个人的工作合并进来，这样才不会覆盖第 一个人的修改。 这和 Subversion （或任何 CVCS）中的概念一样，而且这个模式也可以很好地运用到 Git 中。如果在公司或者团队中，你已经习惯了使用这种集中式工作流程，完全可以继续采用这种简单的模式。 只需要 搭建好一个中心仓库，并给开发团队中的每个人推送数据的权限，就可以开展工作了。Git 不会让用户覆盖彼此 的修改。 例如 John 和 Jessica 同时开始工作。 John 完成了他的修改并推送到服务器。 接着 Jessica 尝试提交她自己的修改，却遭到服务器拒绝。 她被告知她的修改正通过非快进式（non-fastforward）的方式推送，只有将数据抓取下来并且合并后方能推送。 这种模式的工作流程的使用非常广泛，因为 大多数人对其很熟悉也很习惯。当然这并不局限于小团队。 利用 Git 的分支模型，通过同时在多个分支上工作的方式，即使是上百人的开发团队 也可以很好地在单个项目上协作。
* **集成管理者工作流**：Git 允许多个远程仓库存在，使得这样一种工作流成为可能：每个开发者拥有自己仓库的写权限和其他所有人仓 库的读权限。 这种情形下通常会有个代表“官方”项目的权威的仓库。 要为这个项目做贡献，你需要从该项目 克隆出一个自己的公开仓库，然后将自己的修改推送上去。 接着你可以请求官方仓库的维护者拉取更新合并到 主项目。 维护者可以将你的仓库作为远程仓库添加进来，在本地测试你的变更，将其合并入他们的分支并推送 回官方仓库。这是 GitHub 和 GitLab 等集线器式（hub-based）工具最常用的工作流程。人们可以容易地将某个项目派生成 为自己的公开仓库，向这个仓库推送自己的修改，并为每个人所见。 这么做最主要的优点之一是你可以持续地 工作，而主仓库的维护者可以随时拉取你的修改。 贡献者不必等待维护者处理完提交的更新——每一方都可以按 照自己的节奏工作。
* **司令官与副官工作流**：这其实是多仓库工作流程的变种。 一般拥有数百位协作开发者的超大型项目才会用到这样的工作方式，例如著 名的 Linux 内核项目。 被称为副官（lieutenant）的各个集成管理者分别负责集成项目中的特定部分。 所有这 些副官头上还有一位称为司令官（dictator）的总集成管理者负责统筹。 司令官维护的仓库作为参考仓库，为所有协作者提供他们需要拉取的项目代码。这种工作流程并不常用，只有当项目极为庞杂，或者需要多级别管理时，才会体现出优势。 利用这种方式，项 目总负责人（即司令官）可以把大量分散的集成工作委托给不同的小组负责人分别处理，然后在不同时刻将大块 的代码子集统筹起来，用于之后的整合。

## Git 工具

### 选择修订版本

* **引用日志**：当你在工作时， Git 会在后台保存一个引用日志（reflog），引用日志记录了最近几个月你的 HEAD 和分支引用 所指向的历史。你可以使用 git reflog 来查看引用日志，每当你的 HEAD 所指向的位置发生了变化，Git 就会将这个信息存储到引用日志这个历史记录里。 通过这些数 据，你可以很方便地获取之前的提交历史。 如果你想查看仓库中 HEAD 在五次前的所指向的提交，你可以使用 @{n} 来引用 reflog 中输出的提交记录：git show HEAD@{5}。
* **祖先引用**：祖先引用是另一种指明一个提交的方式。 如果你在引用的尾部加上一个 ^， Git 会将其解析为该引用的上一个提交。你可以使用 HEAD^ 来查看上一个提交，也就是 “HEAD 的父提交”：git show HEAD^。你也可以在 ^ 后面添加一个数字——例如 d921970^2 代表 “d921970 的第二父提交” 这个语法只适用于合并 （merge）的提交，因为合并提交会有多个父提交。 第一父提交是你合并时所在分支，而第二父提交是你所合 并的分支：git show d921970^2。另一种指明祖先提交的方法是 ~。 同样是指向第一父提交，因此 HEAD~ 和 HEAD^ 是等价的。 而区别在于你在 后面加数字的时候。 HEAD~2 代表“第一父提交的第一父提交”，也就是“祖父提交”—— Git 会根据你指定的 次数获取对应的第一父提交。也可以写成 HEAD^^^，也是第一父提交的第一父提交的第一父提交。你也可以组合使用这两个语法——你可以通过 HEAD~3^2 来取得之前引用的第二父提交（假设它是一个合并提 交）。

#### 提交区间

你已经学会如何单次的提交，现在来看看如何指明一定区间的提交。 当你有很多分支时，这对管理你的分支时 十分有用，你可以用提交区间来解决“这个分支还有哪些提交尚未合并到主分支？”的问题。

* **双点**：最常用的指明提交区间语法是双点。 这种语法可以让 Git 选出在一个分支中而不在另一个分支中的提交。 你想要查看 experiment 分支中还有哪些提交尚未被合并入 master 分支。 你可以使用 master..experiment 来让 Git 显示这些提交。也就是“在 experiment 分支中而不在 master 分支中的提交”：git log master..experiment。反过来，如果你想查看在 master 分支中而不在 experiment 分支中的提交，你只要交换分支名即可。 experiment..master 会显示在 master 分支中而不在 experiment 分支中的提交：git log experiment..master。这可以让你保持 experiment 分支跟随最新的进度以及查看你即将合并的内容。 另一个常用的场景是查看你即将推送到远端的内容：git log origin/master..HEAD。这个命令会输出在你当前分支中而不在远程 origin 中的提交。 如果你执行 git push 并且你的当前分支正在 跟踪 origin/master，由 git log origin/master..HEAD 所输出的提交就是会被传输到远端服务器的提 交。 如果你留空了其中的一边， Git 会默认为 HEAD。 例如， git log origin/master.. 将会输出与之前 例子相同的结果 —— Git 使用 HEAD 来代替留空的一边。
* **多点**：双点语法很好用，但有时候你可能需要两个以上的分支才能确定你所需要的修订，比如查看哪些提交是被包含在 某些分支中的一个，但是不在你当前的分支上。 Git 允许你在任意引用前加上 ^ 字符或者 --not 来指明你不希 望提交被包含其中的分支。 因此下列3个命令是等价的：

```shell
$ git log refA..refB 
$ git log ^refA refB 
$ git log refB --not refA
# 这个语法很好用，因为你可以在查询中指定超过两个的引用，这是双点语法无法实现的。 
# 比如，你想查看所有 被 refA 或 refB 包含的但是不被 refC 包含的提交，你可以输入下面中的任意一个命令。
$ git log refA refB ^refC 
$ git log refA refB --not refC
# 这就构成了一个十分强大的修订查询系统，你可以通过它来查看你的分支里包含了哪些东西。
```

* **三点**：最后一种主要的区间选择语法是三点，这个语法可以选择出被两个引用中的一个包含但又不被两者同时包含的提 交。 再看看之前双点例子中的提交历史。 如果你想看 master 或者 experiment 中包含的但不是两者共有的提 交，你可以执行：git log master...experiment。这种情形下，log 命令的一个常用参数是 --left-right，它会显示每个提交到底处于哪一侧的分支。 这会让输出数据更加清晰：git log --left-right master...experiment。有了这些工具，你就可以十分方便地查看你 Git 仓库中的提交。

#### 储藏与清理

有时，当你在项目的一部分上已经工作一段时间后，所有东西都进入了混乱的状态，而这时你想要切换到另一个 分支做一点别的事情。 问题是，你不想仅仅因为过会儿回到这一点而为做了一半的工作创建一次提交。 针对这 个问题的答案是 git stash 命令。储藏会处理工作目录的脏的状态——即跟踪文件的修改与暂存的改动——然后将未完成的修改保存到一个栈上， 而你可以在任何时候重新应用这些改动。

* **储藏工作**：为了演示，进入项目并改动几个文件，然后可能暂存其中的一个改动。现在想要切换分支，但是还不想要提交之前的工作；所以储藏修改。 将新的储藏推送到栈上，运行 git stash 或 git stash save。在这时，你能够轻易地切换分支并在其他地方工作；你的修改被存储在栈上。 要查看储藏的东西，可以使用 git stash list。在本例中，有两个之前做的储藏，所以你接触到了三个不同的储藏工作。 可以通过原来 stash 命令的帮助提示 中的命令将你刚刚储藏的工作重新应用：git stash apply。 如果想要应用其中一个更旧的储藏，可以通过 名字指定它，像这样：git stash apply stash@{2}。 如果不指定一个储藏，Git 认为指定的是最近的储藏：git stash apply。可以看到 Git 重新修改了当你保存储藏时撤消的文件。 在本例中，当尝试应用储藏时有一个干净的工作目录，并 且尝试将它应用在保存它时所在的分支；但是有一个干净的工作目录与应用在同一分支并不是成功应用储藏的充 分必要条件。 可以在一个分支上保存一个储藏，切换到另一个分支，然后尝试重新应用这些修改。 当应用储藏 时工作目录中也可以有修改与未提交的文件——如果有任何东西不能干净地应用，Git 会产生合并冲突。文件的改动被重新应用了，但是之前暂存的文件却没有重新暂存。 想要那样的话，必须使用 --index 选项来运 行 git stash apply 命令，来尝试重新应用暂存的修改。 如果已经那样做了，那么你将回到原来的位置：git stash apply --index。应用选项只会尝试应用暂存的工作——在堆栈上还有它。 可以运行 git stash drop 加上将要移除的储藏的名 字来移除它：git stash drop stash@{0}。也可以运行 git stash pop 来应用储藏然后立即从栈上扔掉它。
* **创造性的储藏**：有几个储藏的变种可能也很有用。 第一个非常流行的选项是 stash save 命令的 --keep-index 选项。 它告 诉 Git 不要储藏任何你通过 git add 命令已暂存的东西。 当你做了几个改动并只想提交其中的一部分，过一会儿再回来处理剩余改动时，这个功能会很有用。另一个经常使用储藏来做的事情是像储藏跟踪文件一样储藏未跟踪文件。 默认情况下，git stash 只会储藏已经在索引中的文件。 如果指定 --include-untracked 或 -u 标记，Git 也会储藏任何创建的未跟踪文件。最终，如果指定了 --patch 标记，Git 不会储藏所有修改过的任何东西，但是会交互式地提示哪些改动想要储 藏、哪些改动需要保存在工作目录中。
* **从储藏创建一个分支**：如果储藏了一些工作，将它留在那儿了一会儿，然后继续在储藏的分支上工作，在重新应用工作时可能会有问 题。 如果应用尝试修改刚刚修改的文件，你会得到一个合并冲突并不得不解决它。 如果想要一个轻松的方式来 再次测试储藏的改动，可以运行 git stash branch 创建一个新分支，检出储藏工作时所在的提交，重新在那 应用工作，然后在应用成功后扔掉储藏：git stash branch testchanges。这是在新分支轻松恢复储藏工作并继续工作的一个很不错的途径。
* **清理工作目录**：对于工作目录中一些工作或文件，你想做的也许不是储藏而是移除。 git clean 命令会帮你做这些事。有一些通用的原因比如说为了移除由合并或外部工具生成的东西，或是为了运行一个干净的构建而移除之前构建 的残留。你需要谨慎地使用这个命令，因为它被设计为从工作目录中移除未被追踪的文件。 如果你改变主意了，你也不 一定能找回来那些文件的内容。 一个更安全的选项是运行 git stash --all 来移除每一样东西并存放在栈 中。你可以使用 git clean 命令去除冗余文件或者清理工作目录。 使用 git clean -f -d 命令来移除工作目录 中所有未追踪的文件以及空的子目录。 -f 意味着 强制 或 “确定移除”。如果只是想要看看它会做什么，可以使用 -n 选项来运行命令，这意味着 “做一次演习然后告诉你 将要 移除什 么”：git clean -d -n。默认情况下，git clean 命令只会移除没有忽略的未跟踪文件。 任何与 .gitiignore 或其他忽略文件中的模 式匹配的文件都不会被移除。 如果你也想要移除那些文件，例如为了做一次完全干净的构建而移除所有由构建 生成的 .o 文件，可以给 clean 命令增加一个 -x 选项。如果不知道 git clean 命令将会做什么，在将 -n 改为 -f 来真正做之前总是先用 -n 来运行它做双重检查。 另 一个小心处理过程的方式是使用 -i 或 “interactive” 标记来运行它。这将会以交互模式运行 clean 命令。这种方式下可以分别地检查每一个文件或者交互地指定删除的模式。

#### 搜索

无论仓库里的代码量有多少，你经常需要查找一个函数是在哪里调用或者定义的，或者一个方法的变更历史。 Git 提供了两个有用的工具来快速地从它的数据库中浏览代码和提交。 我们来简单的看一下。

* **Git Grep**：Git 提供了一个 grep 命令，你可以很方便地从提交历史或者工作目录中查找一个字符串或者正则表达式。 我们 用 Git 本身源代码的查找作为例子。 默认情况下 Git 会查找你工作目录的文件。 你可以传入 -n 参数来输出 Git 所找到的匹配行行号：git grep -n gmtime_r。grep 命令有一些有趣的选项。例如，你可以使用 --count 选项来使 Git 输出概述的信息，仅仅包括哪些文件包含匹配以及每个文件包含了多少个匹配：git grep --count gmtime_r。如果你想看匹配的行是属于哪一个方法或者函数，你可以传入 -p 选项：git grep -p gmtime_r *.c。

* **Git 日志搜索**：或许你不想知道某一项在 哪里 ，而是想知道是什么 时候 存在或者引入的。 git log 命令有许多强大的工具可 以通过提交信息甚至是 diff 的内容来找到某个特定的提交。例如，如果我们想找到 ZLIB_BUF_MAX 常量是什么时候引入的，我们可以使用 -S 选项来显示新增和删除该字 符串的提交：git log -SZLIB_BUF_MAX --oneline。如果我们查看这些提交的 diff，我们可以看到在 ef49a7a 这个提交引入了常量，并且在 e01503b 这个提交中被修改了。如果你希望得到更精确的结果，你可以使用 -G 选项来使用正则表达式搜索。

* **行日志搜索**：行日志搜索是另一个相当高级并且有用的日志搜索功能。 这是一个最近新增的不太知名的功能，但却是十分有 用。 在 git log 后加上 -L 选项即可调用，它可以展示代码中一行或者一个函数的历史。

  例如，假设我们想查看 zlib.c 文件中 git_deflate_bound 函数的每一次变更，我们可以执行 git log -L :git_deflate_bound:zlib.c。 Git 会尝试找出这个函数的范围，然后查找历史记录，并且显示从函数创建 之后一系列变更对应的补丁：git log -L :git_deflate_bound:zlib.c。如果 Git 无法计算出如何匹配你代码中的函数或者方法，你可以提供一个正则表达式。 例如，这个命令和上面的 是等同的：git log -L '/unsigned long git_deflate_bound/',/^}/:zlib.c。 你也可以提供单行 或者一个范围的行号来获得相同的输出。

#### 重写历史

**核武器级选项：filter-branch**：有另一个历史改写的选项，如果想要通过脚本的方式改写大量提交的话可以使用它——例如，全局修改你的邮箱 地址或从每一个提交中移除一个文件。 这个命令是 filter-branch，它可以改写历史中大量的提交，除非你 的项目还没有公开并且其他人没有基于要改写的工作的提交做的工作，你不应当使用它。 然而，它可以很有 用。 你将会学习到几个常用的用途，这样就得到了它适合使用地方的想法。

* **从每一个提交移除一个文件**：这经常发生。 有人粗心地通过 git add . 提交了一个巨大的二进制文件，你想要从所有地方删除它。 可能偶 然地提交了一个包括一个密码的文件，然而你想要开源项目。 filter-branch 是一个可能会用来擦洗整个提 交历史的工具。 为了从整个提交历史中移除一个叫做 passwords.txt 的文件，可以使用 --tree-filter 选项 给 filter-branch：git filter-branch --tree-filter 'rm -f passwords.txt' HEAD。--tree-filter 选项在检出项目的每一个提交后运行指定的命令然后重新提交结果。 在本例中，你从每一个 快照中移除了一个叫作 passwords.txt 的文件，无论它是否存在。 如果想要移除所有偶然提交的编辑器备份文 件，可以运行类似 git filter-branch --tree-filter 'rm -f *~' HEAD 的命令。最后将可以看到 Git 重写树与提交然后移动分支指针。 通常一个好的想法是在一个测试分支中做这件事，然后当 你决定最终结果是真正想要的，可以硬重置 master 分支。 为了让 filter-branch 在所有分支上运行，可以 给命令传递 --all 选项。
* **使一个子目录做为新的根目录**：假设已经从另一个源代码控制系统中导入，并且有几个没意义的子目录（trunk、tags 等等）。 如果想要让 trunk 子目录作为每一个提交的新的项目根目录，filter-branch 也可以帮助你那么做：git filter-branch --subdirectory-filter trunk HEAD。现在新项目根目录是 trunk 子目录了。 Git 会自动移除所有不影响子目录的提交。
* **全局修改邮箱地址**：另一个常见的情形是在你开始工作时忘记运行 git config 来设置你的名字与邮箱地址，或者你想要开源一个 项目并且修改所有你的工作邮箱地址为你的个人邮箱地址。 任何情形下，你也可以通过 filter-branch 来一 次性修改多个提交中的邮箱地址。 需要小心的是只修改你自己的邮箱地址，所以你使用 --commit-filter：

```shell
$ git filter-branch --commit-filter '
				if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ]; 
				then
							GIT_AUTHOR_NAME="Scott Chacon";
							GIT_AUTHOR_EMAIL="schacon@example.com";
							git commit-tree "$@"; 
        else
							git commit-tree "$@"; 
        fi' HEAD
```

这会遍历并重写每一个提交来包含你的新邮箱地址。 因为提交包含了它们父提交的 SHA-1 校验和，这个命令会 修改你的历史中的每一个提交的 SHA-1 校验和，而不仅仅只是那些匹配邮箱地址的提交。

#### 使用 Git 调试

Git 也提供了两个工具来辅助你调试项目中的问题。 由于 Git 被设计成适用于几乎所有类型的项目，这些工具是 比较通用的，但它们可以在出现问题的时候帮助你找到 bug 或者错误。

* **文件标注**：如果你在追踪代码中的一个 bug，并且想知道是什么时候以及为何会引入，文件标注通常是最好用的工具。 它 展示了文件中每一行最后一次修改的提交。 所以，如果你在代码中看到一个有问题的方法，你可以使用 git blame 标注这个文件，查看这个方法每一行的最后修改时间以及是被谁修改的。 这个例子使用 -L 选项来限制输 出范围在第12至22行：git blame -L 12,22 simplegit.rb。请注意，第一个字段是最后一次修改该行的提交的部分 SHA-1 值。 接下来两个字段的值是从提交中提取出来 的——作者的名字以及提交的时间——所以你就可以很轻易地找到是谁在什么时候修改了那一行。 接下来就是行 号和文件内容。 注意一下 ^4832fe2 这个提交的那些行，这些指的是这个文件第一次提交的那些行。 这个提交 是这个文件第一次加入到这个项目时的提交，并且这些行从未被修改过。 这会带来小小的困惑，因为你已经至 少看到三种 Git 使用 ^ 来修饰一个提交的 SHA-1 值的不同含义，但这里确实就是这个意思。另一件比较酷的事情是 Git 不会显式地记录文件的重命名。 它会记录快照，然后在事后尝试计算出重命名的动作。 这其中有一个很有意思的特性就是你可以让 Git 找出所有 的代码移动。 如果你在 git blame 后面加上一个 -C，Git 会分析你正在标注的文件，并且尝试找出文件中从别 的地方复制过来的代码片段的原始出处。 比如，你将 GITServerHandler.m 这个文件拆分为数个文件，其中 一个文件是 GITPackUpload.m。 对 GITPackUpload.m 执行带 -C 参数的 blame 命令，你就可以看到代码块 的原始出处：git blame -C -L 141,153 GITPackUpload.m。这个功能很有用。 通常来说，你会认为复制代码过来的那个提交是最原始的提交，因为那是你第一次在这个文 件中修改了这几行。 但 Git 会告诉你，你第一次写这几行代码的那个提交才是原始提交，即使这是在另外一个文 件里写的。
* **二分查找**：当你知道问题是在哪里引入的情况下文件标注可以帮助你查找问题。 如果你不知道哪里出了问题，并且自从上 次可以正常运行到现在已经有数十个或者上百个提交，这个时候你可以使用 git bisect 来帮助查找。 bisect 命令会对你的提交历史进行二分查找来帮助你尽快找到是哪一个提交引入了问题。假设你刚刚在线上环境部署了你的代码，接着收到一些 bug 反馈，但这些 bug 在你之前的开发环境里没有出现 过，这让你百思不得其解。 你重新查看了你的代码，发现这个问题是可以被重现的，但是你不知道哪里出了问 题。 你可以用二分法来找到这个问题。 首先执行 git bisect start 来启动，接着执行 git bisect bad 来告诉系统当前你所在的提交是有问题的。 然后你必须告诉 bisect 已知的最后一次正常状态是哪次提交，使用 git bisect good [good_commit]：

```shell
$ git bisect start 
$ git bisect bad 
$ git bisect good v1.0 
Bisecting: 6 revisions left to test after this 
[ecb6e1bc347ccecc5f9350d878ce677feb13d3b2] error handling on repo
# Git 发现在你标记为正常的提交（v1.0）和当前的错误版本之间有大约12次提交，于是 Git 检出中间的那个提交。
# 现在你可以执行测试，看看在这个提交下问题是不是还是存在。 
# 如果还存在，说明问题是在这个提交之前 引入的；
# 如果问题不存在，说明问题是在这个提交之后引入的。 
# 假设测试结果是没有问题的，你可以通过 git bisect good 来告诉 Git，然后继续寻找。
$ git bisect good 
Bisecting: 3 revisions left to test after this 
[b047b02ea83310a70fd603dc8cd7a6cd13d15c04] secure this thing
# 现在你在另一个提交上了，这个提交是刚刚那个测试通过的提交和有问题的提交的中点。 
# 你再一次执行测试， 发现这个提交下是有问题的，因此你可以通过 git bisect bad 告诉 Git：
$ git bisect bad 
Bisecting: 1 revisions left to test after this 
[f71ce38690acf49c1f3c9bea38e09d82a5ce6014] drop exceptions table
# 这个提交是正常的，现在 Git 拥有的信息已经可以确定引入问题的位置在哪里。 
# 它会告诉你第一个错误提交的 SHA-1 值并显示一些提交说明，以及哪些文件在那次提交里修改过，
# 这样你可以找出引入 bug 的根源：
$ git bisect good
# 当你完成这些操作之后，你应该执行 git bisect reset 重置你的 HEAD 指针到最开始的位置，
# 否则你会停留在一个很奇怪的状态：
$ git bisect reset
# 这是一个可以帮助你在几分钟内从数百个提交中找到 bug 的强大工具。 
# 事实上，如果你有一个脚本在项目是正常的情况下返回 0，在不正常的情况下返回非 0，你可以使 git bisect 自动化这些操作。
# 首先，你设定好项目正常以及不正常所在提交的二分查找范围。 
# 你可以通过 bisect start 命令的参数来设定 这两个提交，第一个参数是项目不正常的提交，第二个参数是项目正常的提交：
$ git bisect start HEAD v1.0 
$ git bisect run test-error.sh
# Git 会自动在每个被检出的提交里执行 test-error.sh 直到找到第一个项目不正常的提交。 
# 你也可以执行 make 或者 make tests 或者其他东西来进行自动化测试。
```

#### 凭证存储

如果你使用的是 SSH 方式连接远端，并且设置了一个没有口令的密钥，这样就可以在不输入用户名和密码的 情况下安全地传输数据。 然而，这对 HTTP 协议来说是不可能的 —— 每一个连接都是需要用户名和密码的。 这 在使用双重认证的情况下会更麻烦，因为你需要输入一个随机生成并且毫无规律的 token 作为密码。

幸运的是，Git 拥有一个凭证系统来处理这个事情。 下面有一些 Git 的选项：

* 默认所有都不缓存。 每一次连接都会询问你的用户名和密码。
* “cache” 模式会将凭证存放在内存中一段时间。 密码永远不会被存储在磁盘中，并且在15分钟后从内存 中清除。
* “store” 模式会将凭证用明文的形式存放在磁盘中，并且永不过期。 这意味着除非你修改了你在 Git 服务 器上的密码，否则你永远不需要再次输入你的凭证信息。 这种方式的缺点是你的密码是用明文的方式存放 在你的 home 目录下。
* 如果你使用的是 Mac，Git 还有一种 “osxkeychain” 模式，它会将凭证缓存到你系统用户的钥匙串中。 这种方式将凭证存放在磁盘中，并且永不过期，但是是被加密的，这种加密方式与存放 HTTPS 凭证以及 Safari 的自动填写是相同的。
* 如果你使用的是 Windows，你可以安装一个叫做 “winstore” 的辅助工具。 这和上面说的 “osxkeychain” 十分类似，但是是使用 Windows Credential Store 来控制敏感信息。 可以在 https://gitcredentialstore.codeplex.com 下载。

你可以设置 Git 的配置来选择上述的一种方式：git config --global credential.helper cache

部分辅助工具有一些选项。 “store” 模式可以接受一个 --file \<path\> 参数，可以自定义存放密码的文件路 径（默认是 ~/.git-credentials ）。 “cache” 模式有 --timeout \<seconds\> 参数，可以设置后台进 程的存活时间（默认是 “900”，也就是 15 分钟）。 下面是一个配置 “store” 模式自定义路径的例子：git config --global credential.helper store --file ~/.my-credentials Git 甚至允许你配置多个辅助工具。 当查找特定服务器的凭证时，Git 会按顺序查询，并且在找到第一个回答时 停止查询。 当保存凭证时，Git 会将用户名和密码发送给 所有 配置列表中的辅助工具，它们会按自己的方式处 理用户名和密码。 如果你在闪存上有一个凭证文件，但又希望在该闪存被拔出的情况下使用内存缓存来保存用 户名密码，.gitconfig 配置文件如下：

```shell
[credential]
    helper = store --file /mnt/thumbdrive/.git-credentials 
    helper = cache --timeout 30000
```

## 自定义 Git

### 配置 Git

* **core.editor**：默认情况下，Git 会调用环境变量（$VISUAL 或 $EDITOR）设置的任意文本编辑器，如果没有设置，会调用 vi 来创建和编辑你的提交以及标签信息。 你可以使用 core.editor 选项来修改默认的编辑器：git config --global core.editor emacs 现在，无论你定义了什么终端编辑器，Git 都会调用 Emacs 编辑信息。
* **commit.template**：如果把此项指定为你的系统上某个文件的路径，当你提交的时候， Git 会使用该文件的内容作为提交的默认信 息。要想让 Git 把它作为运行 git commit 时显示在你的编辑器中的默认信息， 如下设置 commit.template：git config --global commit.template ~/.gitmessage.txt 然后当你提交时，编辑器中就会显示如下的提交信息占位符，如果你的团队对提交信息有格式要求，可以在系统上创建一个文件，并配置 Git 把它作为默认的模板，这样可以更加容易地使提交信息遵循格式。
* **core.pager**：该配置项指定 Git 运行诸如 log 和 diff 等命令所使用的分页器。 你可以把它设置成用 more 或者任何你喜欢 的分页器（默认用的是 less），当然也可以设置成空字符串，关闭该选项：git config --global core.pager '' 这样不管命令的输出量多少，Git 都会在一页显示所有内容。
* **user.signingkey**：如果你要创建经签署的含附注的标签，那么把你的 GPG 签署密钥设置为配置项会更 好。 如下设置你的密钥 ID： git config --global user.signingkey \<gpg-key-id\> 现在，你每次运行 git tag 命令时，即可直接签署标签，而无需定义密钥：git tag -s \<tag-name\>
* **core.excludesfile**：正如 忽略文件 所述，你可以在你的项目的 .gitignore 文件里面规定无需纳入 Git 管理的文件的模板，这样 它们既不会出现在未跟踪列表，也不会在你运行 git add 后被暂存。不过有些时候，你想要在你所有的版本库中忽略掉某一类文件。 如果你的操作系统是 OS X，很可能就是指 .DS_Store。 如果你把 Emacs 或 Vim 作为首选的编辑器，你肯定知道以 ~ 结尾的临时文件。这个配置允许你设置类似于全局生效的 .gitignore 文件。 如果你按照下面的内容创建一个 ~/.gitignore_global 文件，然后运行 git config --global core.excludesfile ~/.gitignore_global，Git 将把那些文件永远地拒之门外。
* **help.autocorrect**：假如你打错了一条命令，Git 会尝试猜测你的意图，但是它不会越俎代庖。 如果你把 help.autocorrect 设置成 1，那么只要有一个命 令被模糊匹配到了，Git 会自动运行该命令。help.autocorrect 接受一个代表十分之一秒的整数。 所以如果你把它设置为 50：git config --global help.autocorrect 50， Git 将在自动执行命令前给你 5 秒的时间改变主意。

### 格式化与多余的空白字符

格式化与多余的空白字符是许多开发人员在协作时，特别是在跨平台情况下，不时会遇到的令人头疼的琐碎的 问题。 由于编辑器的不同或者文件行尾的换行符在 Windows 下被替换了，一些细微的空格变化会不经意地混入 提交的补丁或其它协作成果中。 不用怕，Git 提供了一些配置项来帮助你解决这些问题。

**core.autocrlf**

假如你正在 Windows 上写程序，而你的同伴用的是其他系统（或相反），你可能会遇到 CRLF 问题。 这是因 为 Windows 使用回车（CR）和换行（LF）两个字符来结束一行，而 Mac 和 Linux 只使用换行（LF）一个字 符。 虽然这是小问题，但它会极大地扰乱跨平台协作。许多 Windows 上的编辑器会悄悄把行尾的换行字符转换 成回车和换行，或在用户按下 Enter 键时，插入回车和换行两个字符。

Git 可以在你提交时自动地把回车和换行转换成换行，而在检出代码时把换行转换成回车和换行。 你可以用 core.autocrlf 来打开此项功能。 如果是在 Windows 系统上，把它设置成 true，这样在检出代码时，换行 会被转换成回车和换行：

```shell
$ git config --global core.autocrlf true
```

如果使用以换行作为行结束符的 Linux 或 Mac，你不需要 Git 在检出文件时进行自动的转换；然而当一个以回车 加换行作为行结束符的文件不小心被引入时，你肯定想让 Git 修正。 你可以把 core.autocrlf 设置成 input 来告诉 Git 在提交时把回车和换行转换成换行，检出时不转换：

```shell
$ git config --global core.autocrlf input
```

这样在 Windows 上的检出文件中会保留回车和换行，而在 Mac 和 Linux 上，以及版本库中会保留换行。

如果你是 Windows 程序员，且正在开发仅运行在 Windows 上的项目，可以设置 false 取消此功能，把回车保 留在版本库中：

```shell
$ git config --global core.autocrlf false
```

**core.whitespace**

Git 预先设置了一些选项来探测和修正多余空白字符问题。 它提供了六种处理多余空白字符的主要选项 —— 其中 三个默认开启，另外三个默认关闭，不过你可以自由地设置它们。

默认被打开的三个选项是：blank-at-eol，查找行尾的空格；blank-at-eof，盯住文件底部的空 行；space-before-tab，警惕行头 tab 前面的空格。

默认被关闭的三个选项是：indent-with-non-tab，揪出以空格而非 tab 开头的行（你可以用 tabwidth 选 项控制它）；tab-in-indent，监视在行头表示缩进的 tab；cr-at-eol，告诉 Git 忽略行尾的回车。

通过设置 core.whitespace，你可以让 Git 按照你的意图来打开或关闭以逗号分割的选项。 要想关闭某个选 项，你可以在输入设置选项时不指定它或在它前面加个 -。 例如，如果你想要打开除 cr-at-eol 之外的所有选 项：

```shell
$ git config --global core.whitespace \ trailing-space,space-before-tab,indent-with-non-tab
```

当你运行 git diff 命令并尝试给输出着色时，Git 将探测到这些问题，因此你在提交前就能修复它们。 用 git apply 打补丁时你也会从中受益。 如果正准备应用的补丁存有特定的空白问题，你可以让 Git 在应用补丁时发 出警告：

```shell
$ git apply --whitespace=warn <patch>
```

或者让 Git 在打上补丁前自动修正此问题：

```shell
$ git apply --whitespace=fix <patch>
```

这些选项也能运用于 git rebase。 如果提交了有空白问题的文件，但还没推送到上游，你可以运行 git rebase --whitespace=fix 来让 Git 在重写补丁时自动修正它们。

### 服务器端配置

Git 服务器端的配置项相对来说并不多，但仍有一些饶有生趣的选项值得你一看。 

**receive.fsckObjects**

Git 能够确认每个对象的有效性以及 SHA-1 检验和是否保持一致。 但 Git 不会在每次推送时都这么做。这个操作 很耗时间，很有可能会拖慢提交的过程，特别是当库或推送的文件很大的情况下。 如果想在每次推送时都要求 Git 检查一致性，设置 receive.fsckObjects 为 true 来强迫它这么做：

```shell
$ git config --system receive.fsckObjects true
```

现在 Git 会在每次推送生效前检查库的完整性，确保没有被有问题的客户端引入破坏性数据。

**receive.denyNonFastForwards**

如果你变基已经被推送的提交，继而再推送，又或者推送一个提交到远程分支，而这个远程分支当前指向的提交 不在该提交的历史中，这样的推送会被拒绝。 这通常是个很好的策略，但有时在变基的过程中，你确信自己需 要更新远程分支，可以在 push 命令后加 -f 标志来强制更新（force-update）。

要禁用这样的强制更新推送（force-pushes），可以设置 receive.denyNonFastForwards：

```shell
$ git config --system receive.denyNonFastForwards true
```

稍后我们会提到，用服务器端的接收钩子也能达到同样的目的。 那种方法可以做到更细致的控制，例如禁止某 一类用户做非快进（non-fast-forwards）推送。

**receive.denyDeletes**

有一些方法可以绕过 denyNonFastForwards 策略。其中一种是先删除某个分支，再连同新的引用一起推送回 该分支。 把 receive.denyDeletes 设置为 true 可以把这个漏洞补上：

```shell
$ git config --system receive.denyDeletes true
```

这样会禁止通过推送删除分支和标签 — 没有用户可以这么做。 要删除远程分支，必须从服务器手动删除引用文 件。 通过用户访问控制列表（ACL）也能够在用户级的粒度上实现同样的功能，你将在 使用强制策略的一个例 子 一节学到具体的做法。

### 合并策略

通过 Git 属性，你还能对项目中的特定文件指定不同的合并策略。 一个非常有用的选项就是，告诉 Git 当特定 文件发生冲突时不要尝试合并它们，而是直接使用你这边的内容。

考虑如下场景：项目中有一个分叉的或者定制过的特性分支，你希望该分支上的更改能合并回你的主干分支，同 时需要忽略其中某些文件。此时这个合并策略就能派上用场。 假设你有一个数据库设置文件 database.xml， 在两个分支中它是不同的，而你想合并另一个分支到你的分支上，又不想弄乱该数据库文件。 你可以设置属性 如下：

```shell
database.xml merge=ours
```

然后定义一个虚拟的合并策略，叫做 ours：

```shell
$ git config --global merge.ours.driver true
```

如果你合并了另一个分支，database.xml 文件不会有合并冲突，相反会显示如下信息：

```shell
$ git merge topic Auto-merging database.xml Merge made by recursive.
```

这里，database.xml 保持了主干分支中的原始版本。

## Git 内部原理

从根本上来讲 Git 是一个内容寻址 （content-addressable）文件系统，并在此之上提供了一个版本控制系统的用户界面。 早期的 Git（主要是 1.5 之前的版本）的用户界面要比现在复杂的多，因为它更侧重于作为一个文件系统，而不 是一个打磨过的版本控制系统。 不时会有一些陈词滥调抱怨早期那个晦涩复杂的 Git 用户界面；不过最近几年来，它已经被改进到不输于任何其他版本控制系统地清晰易用了。

### 维护与数据恢复

有的时候，你需要对仓库进行清理——使它的结构变得更紧凑，或是对导入的仓库进行清理，或是恢复丢失的内 容。 这个小节将会介绍这些情况中的一部分。

**维护**

Git 会不定时地自动运行一个叫做 “auto gc” 的命令。 大多数时候，这个命令并不会产生效果。 然而，如果有 太多松散对象（不在包文件中的对象）或者太多包文件，Git 会运行一个完整的 git gc 命令。 “gc” 代表垃 圾回收，这个命令会做以下事情：收集所有松散对象并将它们放置到包文件中，将多个包文件合并为一个大的包 文件，移除与任何提交都不相关的陈旧对象。

可以像下面一样手动执行自动垃圾回收：

```shell
$ git gc --auto
```

就像上面提到的，这个命令通常并不会产生效果。 大约需要 7000 个以上的松散对象或超过 50 个的包文件才能 让 Git 启动一次真正的 gc 命令。 你可以通过修改 gc.auto 与 gc.autopacklimit 的设置来改动这些数值。

gc 将会做的另一件事是打包你的引用到一个单独的文件。

## 附录 C: Git 命令

有两个命令使用得最多了，从第一次调用 Git 到每天的日常微调及参考，这个两个命令就是： config 和 help 命令。

**git config**

Git 做的很多工作都有一个默认方式。 对于绝大多数工作而言，你可以改变 Git 的默认方式，或者根据你的偏好 来设置。 这些设置涵盖了所有的事，从告诉 Git 你的名字，到指定偏好的终端颜色，以及你使用的编辑器。 此 命令会从几个特定的配置文件中读取和写入配置值，以便你可以从全局或者针对特定的仓库来进行设置。 本书的所有章节几乎都有用到 git config 命令。 在 初次运行 Git 前的配置 一节中，在开始使用 Git 之前，我们用它来指定我们的名字，邮箱地址和编辑器偏好。 在 Git 别名 一节中我们展示了如何创建可以展开为长选项序列的短命令，以便你不用每次都输入它们。 在 变基 一节中，执行 git pull 命令时，使用此命令来将 --rebase 作为默认选项。 在 凭证存储 一节中，我们使用它来为你的 HTTP 密码设置一个默认的存储区域。 在 关键字展开 一节中我们展示了如何设置在 Git 的内容添加和减少时使用的 smudge 过滤器 和 clean 过滤器。 最后，基本上 配置 Git 整个章节都是针对此命令的。

**git help**

git help 命令用来显示任何命令的 Git 自带文档。 但是我们仅会在此附录中提到大部分最常用的命令，对于每 一个命令的完整的可选项及标志列表，你可以随时运行 git help \<command\> 命令来了解。

我们在 获取帮助 一节中介绍了 git help 命令，同时在 配置服务器 一节中给你展示了如何使用它来查找更多 关于 git shell 的信息。

### 获取与创建项目

有几种方式获取一个 Git 仓库。 一种是从网络上或者其他地方拷贝一个现有的仓库，另一种就是在一个目录中创 建一个新的仓库。

**git init**

你只需要简单地运行 git init 就可以将一个目录转变成一个 Git 仓库，这样你就可以开始对它进行版本管理 了。 我们一开始在 获取 Git 仓库 一节中介绍了如何创建一个新的仓库来开始工作。 在 远程分支 一节中我们简单的讨论了如何改变默认分支。 在 把裸仓库放到服务器上 一节中我们使用此命令来为一个服务器创建一个空的祼仓库。 最后，我们在 底层命令和高层命令 一节中介绍了此命令背后工作的原理的一些细节。

**git clone**

git clone 实际上是一个封装了其他几个命令的命令。 它创建了一个新目录，切换到新的目录，然后 git init 来初始化一个空的 Git 仓库， 然后为你指定的 URL 添加一个（默认名称为 origin 的）远程仓库（git remote add），再针对远程仓库执行 git fetch，最后通过 git checkout 将远程仓库的最新提交检出到 本地的工作目录。 git clone 命令在本书中多次用到，这里只列举几个有意思的地方。 在 克隆现有的仓库 一节中我们通过几个示例详细介绍了此命令。 在 在服务器上搭建 Git 一节中，我们使用了 --bare 选项来创建一个没有任何工作目录的 Git 仓库副本。 在 打包 一节中我们使用它来解包一个打包好的 Git 仓库。 最后，在 克隆含有子模块的项目 一节中我们学习了使用 --recursive 选项来让克隆一个带有子模块的仓库变 得简单。 虽然在本书的其他地方都有用到此命令，但是上面这些用法是特例，或者使用方式有点特别。

### 快照基础

对于基本的暂存内容及提交到你的历史记录中的工作流，只有少数基本的命令。

**git add**

git add 命令将内容从工作目录添加到暂存区（或称为索引（index）区），以备下次提交。 当 git commit 命令执行时，默认情况下它只会检查暂存区域，因此 git add 是用来确定下一次提交时快照的样子的。 这个命令对于 Git 来说特别的重要，所以在本书中被无数次的提及和使用。 我们将快速的过一遍一些可以看到的 独特的用法。 我们在 跟踪新文件 一节中介绍并详细解释了 git add 命令。 然后，我们在 遇到冲突时的分支合并 一节中提到了如何使用它来解决合并冲突。

接下来，我们在 交互式暂存 一章中使用它来交互式的暂存一个已修改文件的特定部分。 最后，在 树对象 一节中我们在一个低层次中模拟了它的用法，以便你可以了解在这背后发生了什么。

**git status**

git status 命令将为你展示工作区及暂存区域中不同状态的文件。 这其中包含了已修改但未暂存，或已经暂 存但没有提交的文件。 一般在它显示形式中，会给你展示一些关于如何在这些暂存区域之间移动文件的提示。 首先，我们在 检查当前文件状态 一节中介绍了 status 的基本及简单的形式。 虽然我们在全书中都有用到它， 但是绝大部分的你能用 git status 做的事情都在这一章讲到了。

**git diff**

当需要查看任意两棵树的差异时你可以使用 git diff 命令。 此命令可以查看你工作环境与你的暂存区的差异 （git diff 默认的做法），你暂存区域与你最后提交之间的差异（git diff --staged），或者比较两个 提交记录的差异（git diff master branchB）。

首先，我们在 查看已暂存和未暂存的修改 一章中研究了 git diff 的基本用法，在此节中我们展示了如何查看 哪些变化已经暂存了，哪些没有。 在 提交准则 一节中,我们在提交前使用 --check 选项来检查可能存在的空白字符问题。 在 确定引入了哪些东西 一节中,了解了使用 git diff A...B 语法来更有效地比较不同分支之间的差异。 在 高级合并 一节中我们使用 -b 选项来过滤掉空白字符的差异，及通过 --theirs、--ours 和 --base 选项来 比较不同暂存区冲突文件的差异。 最后，在 开始使用子模块 一节中,我们使用此命令合 --submodule 选项来有效地比较子模块的变化。

**git difftool**

当你不想使用内置的 git diff 命令时。git difftool 可以用来简单地启动一个外部工具来为你展示两棵树 之间的差异。

我们只在 查看已暂存和未暂存的修改 一节中简单的提到了此命令。

**git commit**

git commit 命令将所有通过 git add 暂存的文件内容在数据库中创建一个持久的快照，然后将当前分支上的 分支指针移到其之上。

首先，我们在 提交更新 一节中涉及了此命令的基本用法。 我们演示了如何在日常的工作流程中通过使用 -a 标 志来跳过 git add 这一步，及如何使用 -m 标志通过命令行而不启动一个编辑器来传递提交信息。 在 撤消操作 一节中我们介绍了使用 --amend 选项来重做最后的提交。 在 分支简介，我们探讨了 git commit 的更多细节，及工作原理。

在 签署提交 一节中我们探讨了如何使用 -S 标志来为提交签名加密。 最后，在 提交对象 一节中，我们了解了 git commit 在背后做了什么，及它是如何实现的。

**git reset**

git reset 命令主要用来根据你传递给动作的参数来执行撤销操作。 它可以移动 HEAD 指针并且可选的改变 index 或者暂存区，如果你使用 --hard 参数的话你甚至可以改变工作区。 如果错误地为这个命令附加后面的 参数，你可能会丢失你的工作，所以在使用前你要确定你已经完全理解了它。 首先，我们在 取消暂存的文件 一节中介绍了 git reset 简单高效的用法，用来对执行过 git add 命令的文件 取消暂存。 在 重置揭密 一节中我们详细介绍了此命令，几乎整节都在解释此命令。 在 中断一次合并 一节中，我们使用 git reset --hard 来取消一个合并，同时我们也使用了 git merge --abort 命令，它是 git reset 的一个简单的封装。

**git rm**

git rm 是 Git 用来从工作区，或者暂存区移除文件的命令。 在为下一次提交暂存一个移除操作上，它与 git add 有一点类似。 我们在 移除文件 一节中提到了 git rm 的一些细节，包括递归地移除文件，和使用 --cached 选项来只移除暂 存区域的文件但是保留工作区的文件。 在本书的 移除对象 一节中，介绍了 git rm 仅有的几种不同用法，如在执行 git filter-branch 中使用和 解释了 --ignore-unmatch 选项。 这对脚本来说很有用。

**git mv**

git mv 命令是一个便利命令，用于移到一个文件并且在新文件上执行`git add`命令及在老文件上执行`git rm`命令。

我们只是在 移动文件 一节中简单地提到了此命令。

**git clean**

git clean 是一个用来从工作区中移除不想要的文件的命令。 可以是编译的临时文件或者合并冲突的文件。

在 清理工作目录 一节中我们介绍了你可能会使用 clean 命令的大量选项及场景。

### 分支与合并

Git 有几个实现大部的分支及合并功能的实用命令。

**git branch**

git branch 命令实际上是某种程度上的分支管理工具。 它可以列出你所有的分支、创建新分支、删除分支及 重命名分支。 Git 分支 一节主要是为 branch 命令来设计的，它贯穿了整个章节。 首先，我们在 分支创建 一节中介绍了它， 然后我们在 分支管理 一节中介绍了它的其它大部分特性（列举及删除）。 在 跟踪分支 一节中，我们使用 git branch -u 选项来设置一个跟踪分支。 最后，我们在 Git 引用 一节中讲到了它在背后做一什么。

**git checkout**

git checkout 命令用来切换分支，或者检出内容到工作目录。

我们是在 分支切换 一节中第一次认识了命令及 git branch 命令。 在 跟踪分支 一节中我们了解了如何使用 --track 标志来开始跟踪分支。 在 检出冲突 一节中，我们用此命令和 --conflict=diff3 来重新介绍文件冲突。 在 重置揭密 一节中，我们进一步了解了其细节及与 git reset 的关系。 最后，我们在 HEAD 引用 一节中介绍了此命令的一些实现细节。

**git merge**

git merge 工具用来合并一个或者多个分支到你已经检出的分支中。 然后它将当前分支指针移动到合并结果 上。 我们首先在 新建分支 一节中介绍了 git merge 命令。 虽然它在本书的各种地方都有用到，但是 merge 命令只 有几个变种，一般只是 git merge <branch> 带上一个你想合并进来的一个分支名称。 我们在 派生的公开项目 的后面介绍了如何做一个 squashed merge （指 Git 合并时将其当作一个新的提交而不 是记录你合并时的分支的历史记录。） 在 高级合并 一节中，我们介绍了合并的过程及命令，包含 -Xignore-space-change 命令及 --abort 选项 来中止一个有问题的提交。 在 签署提交 一节中我们学习了如何在合并前验证签名，如果你项目正在使用 GPG 签名的话。 最后，我们在 子树合并 一节中学习了子树合并。

**git mergetool**

当你在 Git 的合并中遇到问题时，可以使用 git mergetool 来启动一个外部的合并帮助工具。 我们在 遇到冲突时的分支合并 中快速介绍了一下它，然后在 外部的合并与比较工具 一节中介绍了如何实现你自 己的外部合并工具的细节。

**git log**

git log 命令用来展示一个项目的可达历史记录，从最近的提交快照起。 默认情况下，它只显示你当前所在分 支的历史记录，但是可以显示不同的甚至多个头记录或分支以供遍历。 此命令通常也用来在提交记录级别显示 两个或多个分支之间的差异。

在本书的每一章几乎都有用到此命令来描述一个项目的历史。

在 查看提交历史 一节中我们介绍了此命令，并深入做了研究。 研究了包括 -p 和 --stat 选项来了解每一个提 交引入的变更，及使用`--pretty` 和 --online 选项来查看简洁的历史记录。

在 分支创建 一节中我们使用它加 --decorate 选项来简单的可视化我们分支的指针所在，同时我们使用 --graph 选项来查看分叉的历史记录是怎么样的。

在 私有小型团队 和 提交区间 章节中，我们介绍了在使用 git log 命令时用 branchA..branchB 的语法来查 看一个分支相对于另一个分支, 哪一些提交是唯一的。 在 提交区间 一节中我们作了更多介绍。

在 <_merge_log>> 和 三点 章节中，我们介绍了 branchA...branchB 格式和 --left-right 语法来查看哪 些仅其中一个分支。 在 合并日志 一节中我们还研究了如何使用 --merge 选项来帮助合并冲突调试，同样也使 用 --cc 选项来查看在你历史记录中的合并提交的冲突。

在 引用日志 一节中我们使用此工具和 -g 选项 而不是遍历分支来查看 Git 的 reflog。

在 搜索 一节中我们研究了`-S` 及 -L 选项来进行来在代码的历史变更中进行相当优雅地搜索，如一个函数的历 史。

在 签署提交 一节中，我们了解了如何使用 --show-signature 来为每一个提交的 git log 输出中，添加一 个判断是否已经合法的签名的一个验证。

**git stash**

git stash 命令用来临时地保存一些还没有提交的工作，以便在分支上不需要提交未完成工作就可以清理工作 目录。

储藏与清理 一整个章节基本就是在讲这个命令。

**git tag**

git tag 命令用来为代码历史记录中的某一个点指定一个永久的书签。 一般来说它用于发布相关事项。

我们在 打标签 一节中介绍了此命令及相关细节，并在 为发布打标签 一节实践了此命令。

我也在 签署工作 一节中介绍了如何使用 -s 标志创建一个 GPG 签名的标签，然后使用 -v 选项来验证。

### 项目分享与更新

在 Git 中没有多少访问网络的命令，几乎所以的命令都是在操作本地的数据库。 当你想要分享你的工作，或者从 其他地方拉取变更时，这有几个处理远程仓库的命令。

**git fetch**

git fetch 命令与一个远程的仓库交互，并且将远程仓库中有但是在当前仓库的没有的所有信息拉取下来然后 存储在你本地数据库中。 我们开始在 从远程仓库中抓取与拉取 一节中介绍了此命令，然后我们在 远程分支 中看到了几个使用示例。 我们在 向一个项目贡献 一节中有几个示例中也都有使用此命令。 在 合并请求引用 我们用它来抓取一个在默认空间之外指定的引用，在 打包 中，我们了解了怎么从一个包中获取 内容。 在 引用规格 章节中我们设置了高度自定义的 refspec 以便 git fetch 可以做一些跟默认不同的事情。

**git pull**

git pull 命令基本上就是 git fetch 和 git merge 命令的组合体，Git 从你指定的远程仓库中抓取内容， 然后马上尝试将其合并进你所在的分支中。

我们在 从远程仓库中抓取与拉取 一节中快速介绍了此命令，然后在 查看某个远程仓库 一节中了解了如果你运行 此命令的话，什么将会合并。 我们也在 用变基解决变基 一节中了解了如何使用此命令来来处理变基的难题。 在 检出冲突 一节中我们展示了使用此命令如何通过一个 URL 来一次性的拉取变更。 最后，我们在 签署提交 一节中我们快速的介绍了你可以使用 --verify-signatures 选项来验证你正在拉取 下来的经过 GPG 签名的提交。

**git push**

git push 命令用来与另一个仓库通信，计算你本地数据库与远程仓库的差异，然后将差异推送到另一个仓库 中。 它需要有另一个仓库的写权限，因此这通常是需要验证的。 我们开始在 推送到远程仓库 一节中介绍了 git push 命令。 在这一节中主要介绍了推送一个分支到远程仓库的 基本用法。 在 推送 一节中，我们深入了解了如何推送指定分支，在 跟踪分支 一节中我们了解了如何设置一个 默认的推送的跟踪分支。 在 删除远程分支 一节中我们使用 --delete 标志和 git push 命令来在删除一个在 服务器上的分支。 在 向一个项目贡献 一整节中，我们看到了几个使用 git push 在多个远程仓库分享分支中的工作的示例。 在 共享标签 一节中，我们知道了如何使用此命令加 --tags 选项来分享你打的标签。 在 发布子模块改动 一节中，我们使用 --recurse-submodules 选项来检查是否我们所有的子模块的工作都已 经在推送子项目之前已经推送出去了，当使用子模块时这真的很有帮助。 在 其它客户端钩子 中我们简单的提到了 pre-push 挂钩（hook），它是一个可以用来设置成在一个推送完成之 前运行的脚本，以检查推送是否被允许。 最后，在 引用规格推送 一节中，我们知道了使用完整的 refspec 来推送，而不是通常使用的简写形式。 这对我

们精确的指定要分享出去的工作很有帮助。

**git remote**

git remote 命令是一个是你远程仓库记录的管理工具。 它允许你将一个长的 URL 保存成一个简写的句柄，例 如 origin ，这样你就可以不用每次都输入他们了。 你可以有多个这样的句柄，git remote 可以用来添加， 修改，及删除它们。 此命令在 远程仓库的使用 一节中做了详细的介绍，包括列举、添加、移除、重命名功能。 几乎在此书的后续章节中都有使用此命令，但是一般是以 git remote add <name> <url> 这样的标准格 式。

**git archive**

git archive 命令用来创建项目一个指定快照的归档文件。

我们在 准备一次发布 一节中，使用 git archive 命令来创建一个项目的归档文件用于分享。

**git submodule**

git submodule 命令用来管理一个仓库的其他外部仓库。 它可以被用在库或者其他类型的共享资源上。 submodule 命令有几个子命令, 如（add、update、sync 等等）用来管理这些资源。 只在 子模块 章节中提到和详细介绍了此命令。

### 检查与比较

**git show**

git show 命令可以以一种简单的人类可读的方式来显示一个 Git 对象。 你一般使用此命令来显示一个标签或一 个提交的信息。 我们在 附注标签 一节中使用此命令来显示带注解标签的信息。 然后，我们在 选择修订版本 一节中，用了很多次来显示不同的版本选择将解析出来的提交。 我们使用 git show 做的最有意思的事情是在 手动文件再合并 一节中用来在合并冲突的多个暂存区域中提取指 定文件的内容。

**git shortlog**

git shortlog 是一个用来归纳 git log 的输出的命令。 它可以接受很多与 git log 相同的选项，但是此命 令并不会列出所有的提交，而是展示一个根据作者分组的提交记录的概括性信息

我们在 制作提交简报 一节中展示了如何使用此命令来创建一个漂亮的 changelog 文件。

**git describe**

git describe 命令用来接受任何可以解析成一个提交的东西，然后生成一个人类可读的字符串且不可变。 这 是一种获得一个提交的描述的方式，它跟一个提交的 SHA-1 值一样是无歧义，但是更具可读性。

我们在 生成一个构建号 及 准备一次发布 章节中使用 git describe 命令来获得一个字符串来命名我们发布的 文件。

### 调试

Git 有一些命令可以用来帮你调试你代码中的问题。 包括找出是什么时候，是谁引入的变更。

**git bisect**

git bisect 工具是一个非常有用的调试工具，它通过自动进行一个二分查找来找到哪一个特定的提交是导致 bug 或者问题的第一个提交。

仅在 二分查找 一节中完整的介绍了此命令。

**git blame**

git blame 命令标注任何文件的行，指出文件的每一行的最后的变更的提交及谁是那一个提交的作者。 当你要 找那个人去询问关于这块特殊代码的信息时这会很有用。

只有 文件标注 一节有中提到此命令。

**git grep**

git grep 命令可以帮助在源代码中，甚至是你项目的老版本中的任意文件中查找任何字符串或者正则表达式。

只有 Git Grep 的章节中与提到此命令。

### 补丁

Git 中的一些命令是以引入的变更即提交这样的概念为中心的，这样一系列的提交，就是一系列的补丁。 这些命 令以这样的方式来管理你的分支。

**git cherry-pick**

git cherry-pick 命令用来获得在单个提交中引入的变更，然后尝试将作为一个新的提交引入到你当前分支 上。 从一个分支单独一个或者两个提交而不是合并整个分支的所有变更是非常有用的。

在 变基与拣选工作流 一节中描述和演示了 Cherry picking

**git rebase**

git rebase 命令基本是是一个自动化的 cherry-pick 命令。 它计算出一系列的提交，然后再以它们在其他 地方以同样的顺序一个一个的 cherry-picks 出它们。

在 变基 一章中详细提到了此命令，包括与已经公开的分支的变基所涉及的协作问题。 在 替换 中我们在一个分离历史记录到两个单独的仓库的示例中实践了此命令，同时使用了 --onto 选项。 在 Rerere 一节中，我们研究了在变基时遇到的合并冲突的问题。 在 修改多个提交信息 一节中，我们也结合 -i 选项将其用于交互式的脚本模式。

**git revert**

git revert 命令本质上就是一个逆向的 git cherry-pick 操作。 它将你提交中的变更的以完全相反的方式 的应用到一个新创建的提交中，本质上就是撤销或者倒转。 我们在 还原提交 一节中使用此命令来撤销一个合并提交。

### 邮件

很多 Git 项目，包括 Git 本身，基本是通过邮件列表来维护的。 从方便地生成邮件补丁到从一个邮箱中应用这些 补丁,Git 都有工具来让这些操作变得简单。

**git apply**

git apply 命令应用一个通过 git diff 或者甚至使用 GNU diff 命令创建的补丁。 它跟补丁命令做了差不多 的工作，但还是有一些小小的差别。 我们在 应用来自邮件的补丁 一节中演示了它的使用及什么环境下你可能会用到它。

**git am**

git am 命令用来应用来自邮箱的补丁。特别是那些被 mbox 格式化过的。 这对于通过邮件接受补丁并将他们 轻松地应用到你的项目中很有用。 我们在 使用 am 命令应用补丁 命令中提到了它的用法及工作流，包括使用 --resolved、-i 及 -3 选项。 我们在 电子邮件工作流钩子 也提到了几条 hooks，你可以用来辅助与 git am 相关工作流。 在 邮件通知 一节中我们也将用此命令来应用 格式化的 GitHub 的推送请求的变更。

**git format-patch**

git format-patch 命令用来以 mbox 的格式来生成一系列的补丁以便你可以发送到一个邮件列表中。

我们在 通过邮件的公开项目 一节中研究了一个使用 git format-patch 工具为一个项目做贡献的示例。

**git imap-send**

git imap-send 将一个由 git format-patch 生成的邮箱上传至 IMAP 草稿文件夹。 我们在 通过邮件的公 开项目 一节中见过一个通过使用 git imap-send 工具向一个项目发送补丁进行贡献的例子。

**git send-email**

git send-mail 命令用来通过邮件发送那些使用 git format-patch 生成的补丁。

我们在 通过邮件的公开项目 一节中研究了一个使用 git send-email 工具发送补丁来为一个项目做贡献的示 例。

**git request-pull**

git request-pull 命令只是简单的用来生成一个可通过邮件发送给某个人的示例信息体。 如果你在公共服务 器上有一个分支，并且想让别人知道如何集成这些变更，而不用通过邮件发送补丁，你就可以执行此命令的输出 发送给这个你想拉取变更的人。

我们在 派生的公开项目 一节中演示了如何使用 git request-pull 来生成一个推送消息。

### 外部系统

Git 有一些可以与其他的版本控制系统集成的命令。

**git svn**

git svn 可以使 Git 作为一个客户端来与 Subversion 版本控制系统通信。 这意味着你可以使用 Git 来检出内 容，或者提交到 Subversion 服务器。

Git 与 Subversion 一章深入讲解了此命令。

**git fast-import**

对于其他版本控制系统或者从其他任何的格式导入，你可以使用 git fast-import 快速地将其他格式映射到 Git 可以轻松记录的格式。

在 一个自定义的导入器 一节中深入讲解了此命令。

### 管理

如果你正在管理一个 Git 仓库，或者需要通过一个复杂的方法来修复某些东西，Git 提供了一些管理命令来帮助 你。

**git gc**

git gc 命令在你的仓库中执行 “garbage collection”，删除数据库中不需要的文件和将其他文件打包成一种 更有效的格式。

此命令一般在背后为你工作，虽然你可以手动执行它-如果你想的话。 我们在维护 一节中研究此命令的几个示 例。

**git fsck**

git fsck 命令用来检查内部数据库的问题或者不一致性。

我们只在 数据恢复 这一节中快速使用了一次此命令来搜索所有的悬空对象（dangling object）。

**git reflog**

git reflog 命令分析你所有分支的头指针的日志来查找出你在重写历史上可能丢失的提交。

我们主要在 引用日志 一节中提到了此命令，并在展示了一般用法，及如何使用 git log -g 来通过 git log 的输出来查看同样的信息。 我们同样在 数据恢复 一节中研究了一个恢复丢失的分支的实例。

**git filter-branch**

git filter-branch 命令用来根据某些规则来重写大量的提交记录，例如从任何地方删除文件，或者通过过 滤一个仓库中的一个单独的子目录以提取出一个项目。 在 从每一个提交移除一个文件 一节中，我们解释了此命令，并探究了其他几个选项，例如 --commitfilter，--subdirectory-filter 及 --tree-filter 。 在 Git-p4 和 TFS 的章节中我们使用它来修复已经导入的外部仓库。

### 底层命令

在本书中我们也遇到了不少底层的命令。 

我们遇到的第一个底层命令是在 合并请求引用 中的 ls-remote 命令。

我们用它来查看服务端的原始引用。 我们在 手动文件再合并、 Rerere 及 索引 章节中使用 ls-files 来查看暂存区的更原始的样子。 

我们同样在 分支引用 一节中提到了 rev-parse 命令，它可以接受任意字符串，并将其转成一个对象的 SHA-1 值。 

我们在 Git 内部原理 一章中对大部分的底层命令进行了介绍，这差不多正是这一章的重点所在。 我们尽量避免 了在本书的其他部分使用这些命令。

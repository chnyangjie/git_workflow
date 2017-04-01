#  工作方式

功能分支工作流仍然用中央仓库，并且`master`分支还是代表了正式项目的历史。
但不是直接提交本地历史到各自的本地`master`分支，开发者每次在开始新功能前先创建一个新分支。
功能分支应该有个有描述性的名字，比如`animated-menu-items`或`issue-#1061`，这样可以让分支有个清楚且高聚焦的用途。

在`master`分支和功能分支之间，`Git`是没有技术上的区别，所以开发者可以用和集中式工作流中完全一样的方式编辑、暂存和提交修改到功能分支上。

另外，功能分支也可以（且应该）`push`到中央仓库中。这样不修改正式代码就可以和其它开发者分享提交的功能。
由于`master`是仅有的一个『特殊』分支，在中央仓库上存多个功能分支不会有任何问题。当然，这样做也可以很方便地备份各自的本地提交。

## Pull Requests

功能分支除了可以隔离功能的开发，也使得通过[`Pull Requests`](pull-request.md)讨论变更成为可能。
一旦某个开发完成一个功能，不是立即合并到`master`，而是`push`到中央仓库的功能分支上并发起一个`Pull Request`请求去合并修改到`master`。
在修改成为主干代码前，这让其它的开发者有机会先去`Review`变更。

`Code Review`是`Pull Requests`的一个主要的收益，但`Pull Requests`实际上用作讨论代码的常用手段。
你可以把`Pull Requests`作为专门给某个分支的讨论。这意味着可以在更早的开发过程中就可以进行`Code Review`。
比如，一个开发者开发功能需要帮助时，要做的就是发起一个`Pull Request`，相关的人就会自动收到通知，在相关的提交旁边能看到需要帮助解决的问题。

一旦`Pull Request`被接受了，发布功能要做的就和集中式工作流就很像了。
首先，确定本地的`master`分支和上游的`master`分支是同步的。然后合并功能分支到本地`master`分支并`push`已经更新的本地`master`分支到中央仓库。

仓库管理的产品解决方案像[`Bitbucket`](http://bitbucket.org/)或[`Stash`](http://www.atlassian.com/stash)，可以良好地支持`Pull Requests`。可以看看`Stash`的[`Pull Requests`文档](https://confluence.atlassian.com/display/STASH/Using+pull+requests+in+Stash)。
# Git Outline

* 基础概念
* 工作流程
* 参考资料

## 基础概念
* working tree（工作目录）， stage/index（暂存区）， repository（版本库）
* stage， commit， push
* rebase， merge， pull
* branch， master， HEAD

### 工作目录， 暂存区， 版本库
![aa](https://jwiegley.github.io/git-from-the-bottom-up/images/lifecycle.png)

### stage， commit， push
* stage， 开发完功能的一小部分， 临时存储（存在本地 index，其他同事无法看到）
* commit， 开发完一个完整的功能，添加说明日志并提交（存在本地 repository，其他同事无法看到）
* push， 本地测试并确认开发的功能正确无误，推送到远端仓库（存在远端 repository， 其他同事可以看到）

### rebase， merge， pull
* rebase， 顾名思义， re-base， 基于指定的一个 commit （后续称为 base） 查询到该 base 与当前工作分支的共同祖先节点（后续称之为 ancestor），然后将当前分支最新一个 commit 直至该 ancestor 的所有 commit 重新应用到 base 上形成新的 commit 并将当前分支重新指向最新生成的 commit。   
关于 rebase 的详细解释及图例请参阅 `Git from the Bottom Up` 章节 [Branching and the power of rebase](https://jwiegley.github.io/git-from-the-bottom-up/1-Repository/7-branching-and-the-power-of-rebase.html)
* merge， 不解释
* pull， fetch & rebase，不建议使用 pull 操作，建议先 fetch 查看变更项再决定是否 rebase
* **建议：本地分支与跟踪的远端分支同步时，使用 rebase 代替 merge。但若同时需要合并其他分支时，请先 rebase 跟踪的远端分支后再 merge 其他分支，然后再 push 回远端分支。** 
* **注意: rebase/merge/pull 操作都有可能导致冲突，执行 rebase/merge/pull 时请千万千万注意看提示是否执行成功、是否有冲突！不要不看提示就再次 commit 甚至 push ！**

### branch， master
* branch, 只是指向 commit 的一个可读别名（指针/引用），所以在 git 项目中开分支是非常廉价便捷的一个操作，仅仅只是在本地 repository 中添加一个指针而已。新开发一个小功能时都新开一个分支进行开发。
* master, 约定俗称的一个默认分支名，通常用于表示当前开发分支或线上发布分支。

## 工作流程
* 瀑布流程
* 敏捷流程

### 瀑布流程
* master 分支用于线上发布，不允许个人提交修改。
* develop 分支用于 nightly build，不允许个人提交修改。
* `hotfix/$hotfixName` 分支用于线上 bug 紧急修复。
* 开发新功能时，从 develop 分支开出 `feature/$featureName` 新分支进行开发。在新分支上进行 commit 及 push。记得及时更新 develop 上的最新代码（在新分支上执行 `git rebase develop `，有冲突的话需要手工解决后 `git rebase --continue`）。
* 完成新功能开发后在 gitlab 发起 pull request，由项目管理者进行 code review 及 CI（持续集成系统）自动化测试。确认无误后合并到 develop 分支并删除 `feature/$featureName` 分支。
* 新版本完成所有功能开发并测试确认无误后（未上线的 bug 修复当做新功能开发），master 合并 develop 分支并部署上线。
* 紧急修复线上 bug 时，从 master 分支开出 `hotfix/$hotfixName` 新分支进行开发。完成修复后，在 gitlab 发起 pull request（合并到 master 及 develop 两个分支），由项目管理者进行 code review，CI 在 `hotfix/$hotfixName` 分支上进行回归测试。确认无误后合并到 master 及 develop 两个分支。若 master 合并成功而 develop 分支产生冲突，需要项目管理者在 develop 分支上手工解决冲突。

### 敏捷流程
* master 分支用于线上发布，不允许个人提交修改。
* 开发新功能时，从 master 分支开出新分支进行开发。在新分支上进行 commit 及 push。记得及时更新 master 上的最新代码（在新分支上执行 `git rebase master`，有冲突的话需要手工解决后 git rebase --continue）。
* 完成新功能开发后在 gitlab 发起 pull request，由项目管理者进行 code review 及 CI（持续集成系统）自动化测试。确认无误后合并到 master 并部署上线。
* 紧急修复线上 bug 当做新功能开发处理。

## 参考资料
[1] [为什么要先 git add 才能 git commit？](https://www.zhihu.com/question/19946553)  
[2] [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/)  
[3] [基于git的源代码管理模型——git flow](http://www.ituring.com.cn/article/56870)  
[4] [Git-flow 使用笔记](https://fann.im/blog/2012/03/12/git-flow-notes/)  
[5] [GitHub Flow](http://scottchacon.com/2011/08/31/github-flow.html)  


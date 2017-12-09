Git
----------

官网：

    https://git-scm.com

安装：

    cli：https://git-scm.com/downloads
    gui：https://git-scm.com/downloads/guis

资料：

    [入门]
    http://rogerdudler.github.io/git-guide/index.zh.html

    [全面学习]
    https://github.com/geeeeeeeeek/git-recipes/wiki


工作涉及涉及到的操作：

    基本配置：

        使用 git 是需要进行一些基本的个人配置的。例如 账户（username，email）与 一些偏好默认设置

        git config

        --help   帮助
        --global 全局设置 （所有项目可以通用的配置，** 不应该包含账户设置 **）
        --edit   编辑
        --list   查看所有设置
        --add    添加设置


        // 首先设置账户

        git config --add user.name <Your name>
        git config --add user.email <Your.email@pixara.cn>

        // push 模式
        git config --add push.default simple    # 设置当前的push模式，推荐 simple，禁止使用 matching

        // 设置你的默认编辑器 （可以使用全局配置）

        git config --global --add core.editor vi         # 设置 vi 为默认编辑器 当然可以设置其他

        // merge 工具 (推荐使用 p4merge （可以使用全局配置）)

        git config --global --edit

        ```
          [merge]
                tool = p4merge

          [mergetool]
                trustExitCode = false
                keepTemporaries = false
                keepBackup = false
                prompt = false

          [diff]
                external = p4diff

          [mergetool "p4merge"]
                cmd = p4merge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"
        ```


        * 非命令行模式下（gui）下gui工具会有自己的配置，也请手动配置



    远程仓库交互操作：

        // 克隆一个项目（仓库）到本地
        git clone http://host/xx.git

        解释：
        git 是属于分布式的版本控制系统。 每一个仓库都是完整的。克隆到本地的也不例外。
        所以仓库只有上游与下游的关系，默认上游仓库为 origin，也可以手动添加。

        与 svn checkout 区别：
        - svn checkout 只是检出工作区，并不是完整的仓库，但是只有一个上游，只有 commit 才是真正的提交。
        - git clone 克隆一个完整的仓库到本地。可以直接在本地进行提交操作。但是要注意与上游仓库的兼容，因为最终这些提交都要合并到上游仓库。上游仓库与本地仓库类似于主从关系。


        // 从上游仓库拉取分支（同步上游分支到本地）
        git fetch 从上游拉取全部分支 但不合并（merge）
        git fetch origin master 拉取上游master分支 （不合并）

        fetch 和 pull 关系：
             git pull == git fetch && git merge

        // 推送提交到远程仓库
        git push
        （-f --force 选项 **禁止使用** 这会强制覆盖远程提交记录）



    本地仓库操作：

        本地仓库有三个概念 （工作区 workspace，暂存区 stage，本地仓库repo）

        工作区 就是我们可见的文件目录，在此我们对文件进行修改，添加或删除操作
        暂存区 可以提交到仓库的改动


        当我们想要的改动完成之后 需要提交改动到仓库时 （svn 的 `svn commit` ）
        在 git 中此过程有两个阶段

        1. 将改动添加到暂存区（暂存工作区的改动）
            工作区处理完之后 可以将改动添加到暂存区
        2. 提交
            提交暂存区的改动到仓库，此步完成将提交为正式将你的修改提交并生成提交记录


        // 1. 将工作区中的修改添加到暂存区（stage）

        git add

        git add .                  添加当前目录下的所有文件（递归）到暂存区
        git add -u                 可以把所有已经在repo的文件的变更标记添加，而不会把临时生成的文件也添加进来
        git add file1 file2 [...]  添加多个指定文件到暂存区
        git add prefix_*           可以使用unix风格文件匹配符

        // 2. 提交暂存区的改动到仓库

        git commit -m '在此写入你的提交信息（单行）。必填 并且具有明确的提交信息'
        git commit  交互式提交

    查看状态：

        git status // 查看当前工作区 暂存区的状态 是否有需要提交的改动

        git log // 查看提交历史

        git show // 查看上一个提交修改的内容
        git show COMMITID 查看指定的提交


    分支
        git checkout feature-1     切换到 feature-1 分支
        git checkout -b new-branch 检出当前分支到一个新分支


一些小tip
---------
    0.
    尽量减少无意义的提交。
    比如每次修改一行代码来测试看下效果，
    比如同一个bug修改，js文件和php文件分开提交，
    这样的情况应该尽可能避免。

    1.
    开发时可以从本地 feature 公共分支fork出新分支，在新分支开发，
    开发完一部分之后再合并到feature分支。

    2.
    开发过程中难免会出现拼写错误，语法错误问题。
    那么可能会为了单独修正这个进行一次 commit，
    如果这些问题比较多，那么将会产生较多的冗余提交，影响时间线。
    这时可以通过 git rebase 来将这些提交消除掉 * fixup *

    通过 fixup 与 rebase autosquash 来自动将不必要的bug commit 来抹掉
    提交时加入 --fixup 参数可自动将此提交作为某次提交的修正版本写入提交信息内。
    ` git commit --fixup <commit> `

    交互式 自动合并 fixup 提交 <commit> 为有意义提交的上一次提交
    ` git rebase -i --autosquash <commit> `

    ************** 不要在公共分支上 git rebase ***************
    引用：
    http://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html

    3.
    如果想扔掉工作区或暂存区的改动，返回到上一次提交后的状态 可以用 git reset 命令
    ` git reset [--soft|--mixed|--hard] HEAD `

        --soft – 缓存区和工作目录都不会被改变
        --mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响
        --hard – 缓存区和工作目录都同步到你指定的提交

    4.
    如果不小心进行了有问题的提交，比如某个逻辑提交之后发现走不通，想取消这次提交
    那么可以用 git revert 命令进行回滚
    ` git revert COMMITID `



注意事项
---------

    * 慎用 git rebase，  不可 rebase 远程分支。如果rebase一个远程分支 将会产生混乱
    * 不可用 git push --force



Gitlab - 项目托管工具
-------

    作为项目托管工具，可以管理项目代码以及对工作流程进行一定的规范

    1. 使用 issue 产生代码变更需求，作为开发依据，并且可以作为工作内容体现
    2. 代码合并，merge request（mr）来实现代码的管理
    3. 代码审核，合并前可以进行代码审核 来保证代码质量以及功能不出现重大bug
    4. 还可以实现持续集成，测试，发布



规范
-------

    （一）git commit 规范：

        1. 参看：http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html

        重点在于规则。工具使用可选

        2. 工具安装以及使用：https://gist.github.com/sursir/d8185c21ccdafd415c5e768a657b2777

        3. changelog 应该与issue与需求挂钩，而不应该与提交记录挂钩。

        4. changelog 规范请看 <https://github.com/sursir/Work-docs/blob/master/CHANGELOG-rule.md>


    （二）关于 issue 的使用
        1. 每个需求 每个bug都需要经过 issue 这样可以将工作记录记录下来。
        2. 相关人员应该跟进issue状态

        3. issue 创建之后必须有结束


    ** 请切实遵守规范 **



工作流
--------

    三个主要分支
      * master      开发分支，
      * pre-product 预发布分支 用作测试与发布协调，
      * product     线上分支，

      - 向三个主分支提交与合并都需要提mr 限制提交
      - 提交遵循上游优先的原则 （master -> pre-product -> product） 尽量避免反向合并
      - merge 与 cherry pick 结合使用

    其他为非永久的次要分支 功能分支
    比如：
      * bugfix-a
      * bugfix-b
      * feature-a

    gitlab 作为项目托管工具

    当开发一个新功能（feature）时，流程如下：
    - 作为开始：在gitlab提 issue
    - 管理在 issue 中指定某些成员去开发 并且从 master fork 一个新的功能分支
    - 成员各自将功能分支拉取到本地进行开发
    - 协同开发并提交（带 changelog）
    - 开发完成后 在 issue 中发起 mr（merge request）并且通知管理
    - 管理收到 mr 对提交进行代码审查
    - - 通过则接受 mr
    - - 不通过则at成员进行更改 直到通过
    - 最终通过代码审查 合并回 master 分支。
    - 等待**提测** 保持功能分支存在直到 pre-product 提测通过
    - 开发流程至此结束

    当修补一个非紧急bug（bugfix）时，流程同开发一个新功能（feature）


    当测试功能及非紧急bug时（feature，bugfix），流程如下：
    - 按提测时间 合并到 pre-product 分支，
    - 按 changelog 进行测试
    - 测试不通过，从最初的功能分支开始 进行bugfix（无需重开issue与新开分支，可以在当前issue中体现）直到测试通过
    - 等待**发布** 删除功能分支
    - 测试流程至此结束


    当发布功能及非紧急bug时（feature，bugfix），流程如下：
    （根据 changelog 进行提测与发布）（注意提测的顺序 代码修改可能的依赖）
    （每次提测之后如果再要加测试内容 要新开一个提测）
    （发布只发布提测完整通过的提交，可以等待发布（注意依赖）也可以一次发布多个提测内容）
    - 根据 changelog 进行发布
    - 发布中的注意事项


    当修补一个紧急bug（hotfix）时，
        [在其他分支代码已与线上分支冲突出现冲突时，（全面改动的情况）]
            在主分支中，那么
                1. 可以从未冲突的最下游（pre-product > product）分支fork分支进行修改 完成后再mr，
                2. 同时从主分支fork代码同步修改，完成后mr。
            在功能分支中，那么
                1. 同修复非紧急bug（bugfix）
                2. 同时从功能分支中修改
        [其他]
            同修复非紧急bug（bugfix）

    当测试一个紧急bug（hotfix）时，
        - 优先提测

    当发布一个紧急bug（hotfix）时，
        - 优先发布




远端仓库 gitlab-flow
----------

```
--
  -----------------------------------------------------------------------------------------------------
  |
  |          --                      feature-xx
  |                                   -------*------*---------*-------*-*--   (merge request)
  |          --                      /      bugfix-1                       \
  |                                 /      -----*--------*-------- (mr)     \
  |                                /     ／                        \         \ (merge && add changelog)    (合并到开发主干)
  |          -- master --------------------------------------------**--------**----------------
  |                                                                            \ (mrege && changelog test) (提测)
  |  origin/ -- pre-product  --------------------------------------------------**--------------
  |                                                                              \ (merge && deploy)       (部署)
  |          -- product  --------------------------------------------------------**------------
  |                                                                               |
  |                                                                               |
  |                                                                              (deploy)
  |
  -------------------------------------------------------------------------------------------------------
--
```

本地开发
--------
```
--
  ---------------------------------------------------------------------------------------------------
  |
  |                                  (pull)     (fetch && merge)
  |  local/feature-discount (Xie Fan)  ---*----*----------------------------------
  |                                   /       /      \ (push)
  |                                  /       /        \
  |  origin/feature-discount  ------------------------**-----------------**--------------------
  |                                  \                    \             /
  |                                   \                    \           / (push)
  |  local/feature-discount (Yang Xu)  ----*-----*----------*-----*-----------------
  |                                  (pull)                  (fetch && merge)
  |
  ---------------------------------------------------------------------------------------------------
--
```









Git的学习教程
参考了廖雪峰的Git教程：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

目录结构：
  1.安装Git
    Linux上安装：
    Mac OS上安装：
    Windows上安装：
  2.创建版本库
    即创建repository，步骤：
      1）创建空目录，并进入
        $ mkdir learngit
        $ cd learngit
      2）通过git init命令把这个目录变成Git可以管理的仓库
        $ git init
         （可以发现当前目录下多了一个.git的隐藏目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件）
      3）把文件添加到版本库
        $ git add readme.txt
        用命令git commit告诉Git，把文件提交到仓库：
          $ git commit -m "wrote a readme file"
          (其中-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录）
          可以多次add不同的文件，一次提交commit
          
  3.版本回退
    HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
    穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
    要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本
  4.管理修改
    如果对一个文件要进行两次修改，可使用以下步骤：
    第一次修改 -> git add -> 第二次修改 -> git add -> git commit
  5.删除文件
    一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
      $ rm test.txt
    这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了
    现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
      $ git rm test.txt
      $ git commit -m "remove test.txt"
    另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
      $ git checkout -- test.txt

  6.远程仓库
      要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
      关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
      此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
      分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步。
      另外，要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
      
  5.分支管理
  6.标签管理
  7.使用GitHub
  8.使用码云
  9.自定义Git

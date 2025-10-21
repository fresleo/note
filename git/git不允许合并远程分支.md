Git 不允许直接合并远程分支

一键丢弃所有本地修改并同步远程

    git fetch origin
    git reset --hard origin/main
    git clean -fd

解释：

-   `git fetch origin`：拉取最新远程分支数据（但不合并）。
    
-   `git reset --hard origin/main`：强制将当前分支重置为远程 `main` 的状态，本地所有修改（包括未提交的）都会被丢弃。
    
-   `git clean -fd`：删除所有未被 git 跟踪的文件和文件夹（例如新建但未 `git add` 的文件）。


丢弃工作区的修改：
git  reset  --hard  HEAD
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NjQxNjU2NjAsLTU4ODY4NjE2M119
-->
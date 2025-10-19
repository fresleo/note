---


---

<h1 id="git操作">git操作</h1>
<p>提前准备<br>
检查ssh是否在运行 ：eval “$(ssh-agent -s)”<br>
2添加私钥到ssh代理 ssh-add /root/.ssh/你的私钥</p>
<p>推送</p>
<ol>
<li>
<p>检查当前分支的状态 git status</p>
</li>
<li>
<p>添加更改到暂存区 git add .   或者，你可以单独添加特定的文件：git add filename1 filename2</p>
</li>
</ol>
<p>如果你想提交所有修改的文件<br>
3. 提交更改到本地仓库，提交暂存区的更改到本地仓库，并写一个有意义的提交消息：<br>
git commit -m “Your commit message”</p>
<ol start="4">
<li>
<p>拉取远程仓库的最新更改：git pull origin main</p>
</li>
<li>
<p>推送更改到远程仓库：git push origin main</p>
</li>
</ol>
<p>无需您每次都输入密码（必须要有这一步） ssh-add  |ssh-add /path/to/your/private/key</p>
<p>命令行<br>
查看具体变动内容： git diff</p>
<p>查看暂存区中的变动 git diff --staged</p>
<p>还原未暂存的更改（本地修改未添加到暂存区）<br>
git restore .</p>
<p>步骤 1：贮藏当前更改<br>
首先，在你当前的工作目录中执行 git stash 命令来保存未提交的更改。</p>
<p>git stash</p>
<p>步骤 2：拉取最新的远程代码<br>
接下来，你可以安全地拉取远程仓库的更改：</p>
<p>git pull origin main</p>
<p>拉取完成后，可以恢复之前使用 git stash 贮藏的更改。使用以下命令来弹出并恢复最顶层的贮藏：</p>
<p>git stash pop</p>
<p>这个命令会将贮藏中的更改恢复到工作目录，并从 stash 栈中删除它。你也可以使用 git stash apply 来恢复更改，但不会从栈中删除贮藏项</p>
<p>git stash apply</p>
<p>如果有冲突，git stash pop 和 git stash apply 都会出现冲突的提示，你需要手动解决冲突。<br>
你可以查看当前所有的贮藏项（如果有多个）通过 git stash list</p>
<p>git stash list</p>
<p>如果你希望恢复某个特定的贮藏项，可以通过指定索引来应用它：</p>
<p>git stash apply stash@{1}</p>
<p>步骤 1：查看当前的更改</p>
<p>git status</p>
<p>步骤 2：将更改添加到暂存区</p>
<p>git add .</p>
<p>或者，添加指定文件：</p>
<p>git add &lt;文件名&gt;</p>
<p>步骤 3：提交更改</p>
<p>git commit -m “提交信息”</p>
<p>步骤 4：推送到远程仓库</p>
<p>git push origin &lt;分支名&gt;</p>
<p>git log</p>


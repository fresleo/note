---


---

<p>创建文件夹<br>
mkdir frontend backend nginx ssl</p>
<p>更新系统包列表<br>
首先，打开终端并更新本地的软件包列表：</p>
<p>sudo apt update</p>
<p>安装Git<br>
使用apt命令安装Git：</p>
<p>sudo apt install git</p>
<p>验证Git安装<br>
安装完成后，可以通过以下命令验证Git是否正确安装：</p>
<p>git --version</p>
<ol>
<li>安装 PuTTYgen（如果未安装）<br>
在 Debian 系统上，你可以通过以下命令安装 putty-tools（包含 PuTTYgen）：</li>
</ol>
<p>sudo apt update<br>
sudo apt install putty-tools</p>
<p>使用 puttygen 转换 .ppk 文件为 OpenSSH 格式<br>
运行以下命令，将 .ppk 文件转换为 OpenSSH 格式：</p>
<p>puttygen githubypws.ppk -O private-openssh -o githubypws_openssh</p>
<p>启动 ssh-agent 并自动添加私钥</p>
<p>打开 .bashrc 或 .zshrc 文件：</p>
<p>nano ~/.bashrc</p>
<p>或者如果你使用的是 Zsh：</p>
<p>nano ~/.zshrc</p>
<p>在文件末尾添加以下代码：<br>
eval “$(ssh-agent -s)” &gt; /dev/null 2&gt;&amp;1<br>
ssh-add -l &gt; /dev/null || ssh-add ~/.ssh/id_rsa</p>
<p>这段代码做了以下几件事：<br>
eval “$(ssh-agent -s)” 启动 ssh-agent 进程。<br>
ssh-add -l &gt; /dev/null || ssh-add ~/.ssh/id_rsa 会检查当前是否已经有密钥被加载。如果没有，自动加载指定的密钥文件（这里假设你的私钥是 ~/.ssh/id_rsa，如果路径不同，请修改为相应的路径）</p>
<p>确保 ssh-agent 进程在每次启动时持久化<br>
如果你希望 ssh-agent 在每次启动会话时自动加载并保持有效，确保 ~/.bash_profile 或 ~/.profile 文件中也包含 ssh-agent 的启动命令。</p>
<p>打开 ~/.bash_profile 或 ~/.profile：</p>
<p>nano ~/.bash_profile</p>
<p>添加以下内容，确保 SSH 代理在每次登录时启动：</p>
<p>if [ -z “<span class="katex--inline"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi>S</mi><mi>S</mi><msub><mi>H</mi><mi>A</mi></msub><mi>U</mi><mi>T</mi><msub><mi>H</mi><mi>S</mi></msub><mi>O</mi><mi>C</mi><mi>K</mi><mi mathvariant="normal">"</mi><mo stretchy="false">]</mo><mo separator="true">;</mo><mi>t</mi><mi>h</mi><mi>e</mi><mi>n</mi><mi>e</mi><mi>v</mi><mi>a</mi><mi>l</mi><mi mathvariant="normal">"</mi></mrow><annotation encoding="application/x-tex">SSH_AUTH_SOCK" ]; then
    eval "</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height: 1em; vertical-align: -0.25em;"></span><span class="mord mathnormal" style="margin-right: 0.05764em;">SS</span><span class="mord"><span class="mord mathnormal" style="margin-right: 0.08125em;">H</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.328331em;"><span class="" style="top: -2.55em; margin-left: -0.08125em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mathnormal mtight">A</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"><span class=""></span></span></span></span></span></span><span class="mord mathnormal" style="margin-right: 0.10903em;">U</span><span class="mord mathnormal" style="margin-right: 0.13889em;">T</span><span class="mord"><span class="mord mathnormal" style="margin-right: 0.08125em;">H</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.328331em;"><span class="" style="top: -2.55em; margin-left: -0.08125em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mathnormal mtight" style="margin-right: 0.05764em;">S</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"><span class=""></span></span></span></span></span></span><span class="mord mathnormal" style="margin-right: 0.07153em;">OC</span><span class="mord mathnormal" style="margin-right: 0.07153em;">K</span><span class="mord">"</span><span class="mclose">]</span><span class="mpunct">;</span><span class="mspace" style="margin-right: 0.166667em;"></span><span class="mord mathnormal">t</span><span class="mord mathnormal">h</span><span class="mord mathnormal">e</span><span class="mord mathnormal">n</span><span class="mord mathnormal">e</span><span class="mord mathnormal" style="margin-right: 0.03588em;">v</span><span class="mord mathnormal">a</span><span class="mord mathnormal" style="margin-right: 0.01968em;">l</span><span class="mord">"</span></span></span></span></span>(ssh-agent -s)”<br>
fi</p>
<p>保存并退出，然后运行：<br>
source ~/.bash_profile</p>
<p>将 SSH 密钥文件添加到 SSH 配置文件中，如果没有config文件，那么创建一个</p>
<p>nano ~/.ssh/config</p>
<p>在文件中添加以下内容，确保在连接 Git 服务器时使用正确的私钥：</p>
<p>Host <a href="http://github.com">github.com</a><br>
User git<br>
Hostname <a href="http://github.com">github.com</a><br>
IdentityFile ~/.ssh/id_rsa<br>
UseKeychain yes  # macOS 用户可用<br>
AddKeysToAgent yes  # 自动将密钥添加到 ssh-agent</p>
<p>IdentityFile 指定了你要使用的私钥文件（请替换为实际的私钥路径）。<br>
AddKeysToAgent yes 告诉 SSH 在需要时自动将密钥添加到 SSH 代理。</p>
<p>2.2 检查 SSH 配置是否生效<br>
确保配置正确后，你可以通过以下命令测试 SSH 连接是否使用了正确的密钥：</p>
<p>ssh -T <a href="mailto:git@github.com">git@github.com</a></p>
<p>SSH 私钥权限太“宽松”了<br>
修正权限<br>
chmod 600 ~/.ssh/id_rsa</p>


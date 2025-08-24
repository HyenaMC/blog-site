# HyenaMC官方网站PR手册

写在最前面：如果看不懂请不要俺寻思，直接问kpA或者在线的其他獾狸，谢谢！

关于Markdown语言的基本语法，请参照[这个10分钟教程](https://markdown.com.cn/basic-syntax/)。

关于Fuwari框架的扩展功能，请参照[官方文档](https://github.com/saicaca/fuwari/blob/main/src/content/posts/markdown-extended.md)（同仓库其他文件还有嵌入视频等）。

关于博客框架的其他概况，还可以参照[这位大佬的文章](https://www.2x.nz/posts/fuwari/)。

### 图片资源预备

由于Netlify CDN基于流量计费，服务器现已启用基于Backblaze B2和Cloudflare Worker的独立图床系统，如果需要在文章中插入图片，请访问[OpenList](https://files.teamhyena.org/b2/img)进行查找，然后**对复制出来的图像链接进行重写：**

1. 首先，取你需要使用的图片的文件名，如a3d674c2628b364e63fc9ca309eafee7.webp

2. 将其放到https://mc-img.teamhyena.org/后面

3. 你得到了图床链接，如[https://mc-img.teamhyena.org/a3d674c2628b364e63fc9ca309eafee7.webp](https://mc-img.teamhyena.org/a3d674c2628b364e63fc9ca309eafee7.webp)

4. 测试可以打开后你的图片就准备好了！

如果你需要上传新图片，请把图片用格式工厂等软件**转换成质量90左右的webp格式**（以MC直接截图的PNG为例，通常可以在没有显著区别的情况下减少90%的大小），在OpenList里直接上传，然后使用上面的流程。

### 文章撰写与上传

你需要安装[Github Desktop](https://github.com/apps/desktop)（如果因为Q打不开请使用T）和[MarkText](https://mark-text.en.lo4d.com/windows)编辑器。

在[博客托管仓库](https://github.com/HyenaMC/blog-site)中（也就是本仓库），点击右上角的“Code <>”按钮，选择Open With Github Desktop，然后指定一个专用的本地位置将其克隆到你的计算机上的一个文件夹。然后，使用Marktext左上角的File菜单打开仓库文件夹。

在Github Desktop窗口上方的工具栏，点击Current Branch，点击New Branch，输入一个可以标志你自己，不会被混淆为其他人的名字（比如你的游戏ID），点击创建，确认Current Branch已经变成了你的名字。（如果你之前已经创建过自己的分支，直接选择，然后**点击右上角Fetch Origin**即可）

![](https://mc-img.teamhyena.org/88cd9a600ddc496bf6a4ebea0650aab8.webp)

全程请注意这几个要点：

1. **不要动文章最上面基本信息的基本格式**，填表即可，也**不要动最下面那一串代码**，那是内嵌评论区。

2. **不要直接把图片粘贴进文章**，那会让CDN流量爆炸，在文章里输入：
   
   ```markdown
   <img >
   ```
   
   MarkText会自动把它变成一个工具栏，点一下即可输入上面准备好的图床链接，如果点击**确认后框变成了你的图片**，就说明成功了！

3. **确认你真的在你自己的branch进行编辑！不要直接编辑主分支！**

然后，按照以下指南操作：

- 如果你想要修改一个已有的页面，你应该可以在src/content/posts/下找到你要修改的文章，将其主文件（.md格式）打开，直接编辑，保存，然后直接往下看

- 如果你想要发一个新的帖子，在**仓库文件夹根目录**下右键打开终端、命令提示符或Powershell（如果都没有，按住Shift右键），输入以下命令回车，在src/content/posts/下找到你的文章，正常填写文章上方的基本信息，然后用Markdown格式编辑文章：
  
  ```powershell
  pnpm new-post <此处替换成文件名（英语横杠的标题大意）>
  ```

如果你想要查看你的编辑的效果，在**仓库文件夹根目录**下执行：

```powershell
pnpm dev
```

然后用浏览器访问localhost:4321，等待数秒即可查看你的更改情况！（你可以一边编辑一边查看，保存的时候服务端会自动刷新你的页面来推送最新情况）你输入这条命令的终端会被长期占用来输出日志，如果你需要执行其他命令，你可以打开另一个终端，或者（仅建议在完成测试后）Ctrl-C将其关闭。

最后，确认你没有不小心搞砸任何固定格式，**保存你编辑好的文件**，打开Github Desktop，这个时候窗口中应该已经出现了你进行过的所有编辑（如果没有就是你没保存）。

1. 在左侧的列表勾选你想要应用的编辑

2. 在左下角输入你的编辑的主题和梗概

3. 点击Commit to <你的分支名字>（最好不要是main！）

![](https://mc-img.teamhyena.org/900eec638728050a3645e968a441e834.webp)

然后开始上传你的更改：

![](https://mc-img.teamhyena.org/32c59f3fe67f60033e0be628e3408a62.webp)

1. 点击窗口上方Push to Origin

2. 窗口中间应该会出现一个Preview Pull Request按钮，点一下，复核你要合入主分支的更改，确认，浏览器会自动打开你的PR的页面

3. PR的标题和概述**会被发送到群里**，所以尽量让它们能概括你做出的所有修改

4. 你会看到有很多圈在转，等待它们全部完成（变成绿勾），如果出现红叉，**不要合入**，确认你没有错误使用任何东西或破坏格式，实在不知道为什么就通知kpA来检查

![](https://mc-img.teamhyena.org/2fd78bde3186b1e9e741e478789ce4df.webp)

最后，如果Netlify成功构建了预览页面，你可以点击Deploy Preview后面的链接检查你的更改是否真的如你所愿。

Merge Pull Request，QQ群里应该会出现一条更新信息，然后网站即被更新（可能需要等待1分钟左右）！

## 📄 网站框架许可证

Fuwari框架在MIT许可证下发布（注：该许可证不对本站中可能出现的内容传染）

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fsaicaca%2Ffuwari.svg?type=large&issueType=license)](https://app.fossa.com/projects/git%2Bgithub.com%2Fsaicaca%2Ffuwari?ref=badge_large&issueType=license)

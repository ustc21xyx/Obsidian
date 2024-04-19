## 前言

我将笔记从印象笔记迁移至 Obsidian 后已经一段时间了，使用体验良好。但印象笔记的网页剪裁功能十分便利，我迁移完成后没有来得及找到替代品。

这是在我进行搜索，尝试后，得到的最佳解决方案。

## 问题描述

网页浏览时，可以一键将选中的网页内容裁剪，自动新建笔记，收集。

对应使用体验就是 印象笔记 网络裁剪 浏览器插件。

## 解决方案

Markdownload 浏览器插件 -> Obsidian 社区插件 Advanced Obsidian URI（Local Images Plus 可选） -> 新建笔记

Markdownload 是一个简单用于将网页保存为 md 文件的插件，开始时并不是专门为 Obsidian 开发。之后为了 Obsidian 进行了额外的开发，适配。Advanced Obsidian URI 是一个社区插件，通过打开一些 URI 来控制 Obsidian 中的许多不同功能，经常用于自定义用户自己的自动化工作流程。

这也是一个低耦合的两个部件组合为一个新功能的实际案例。

### 1\. 浏览器 Markdownload 插件

从你的浏览器的插件商店获取 Markdownload 插件并启用。

我的浏览器是 edge，其他浏览器需要你根据自己情况进行调整。

![](https://cdn.sspai.com/2024/01/14/2f84ab51f5022672951ed1b0d3ff7575.png?imageView2/2/format/webp)

对 Markdownload 按自己需要进行配置

![](https://cdn.sspai.com/2024/01/14/21d5cdcdc260a7c5f55a04f6ca7bc7e4.png?imageView2/2/format/webp)

-   配置存放裁剪内容的 Vault
-   配置存放裁剪内容的文件夹名称
    -   开启 Obsidian 集成开关
    -   保证在 Obsidian 中存在对应的 Valut
    -   我按照时间建立文件夹进行分类，插件作者在配置页面提供了其他的替换占位符，可以根据自己的需要进行配置使用。

![](https://cdn.sspai.com/2024/01/14/601f5c5f140d1272b3730db1f5c98feb.png?imageView2/2/format/webp)

-   配置笔记 template（可选）

按需调整模板，同样是利用占位符，默认的模板信息对我已经足够

![](https://cdn.sspai.com/2024/01/14/c140539fec3d83256116d951afa14cf6.png?imageView2/2/format/webp)

-   配置插件快捷键（可选）

![](https://cdn.sspai.com/2024/01/14/b10fc0b00c1c2ce7cf46d40a805e760b.png?imageView2/2/format/webp)

### 2\. Obsidian Advanced Obsidian URI 插件

从 Obsidian 社区插件商店获取 Advanced Obsidian URI 插件，安装，启用，即可。

![](https://cdn.sspai.com/2024/01/14/2b380dc23bdd5321e80770d2c16059be.png?imageView2/2/format/webp)

## 3\. Obsidian Local Images Plus 插件

将网络剪裁中的图片文件自动转化为本地图片文件，永久保存。直接裁剪笔记中的图片仍然是网络外链的形式，如果在服务器上失效，本地也会失效。

![](https://cdn.sspai.com/2024/01/14/7f977a00035f0913ca587a024ae05ff7.png?imageView2/2/format/webp)

## reference

\[MarkDownload github\]([deathau/markdownload: A Firefox and Google Chrome extension to clip websites and download them into a readable markdown file. (github.com)](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fdeathau%2Fmarkdownload%3Ftab%3Dreadme-ov-file))

Markdownload 插件的 github 地址，有不同浏览器对应的下载地址

[MarkDownload - Markdown Web Clipper](https://sspai.com/link?target=https%3A%2F%2Fforum.obsidian.md%2Ft%2Fmarkdownload-markdown-web-clipper%2F173)  
[Configuring the Web Clipper MarkDownload for a seamless workflow with Obsidian](https://sspai.com/link?target=https%3A%2F%2Fforum.obsidian.md%2Ft%2Fconfiguring-the-web-clipper-markdownload-for-a-seamless-workflow-with-obsidian%2F62441%2F1)

两个社区讨论串，如果遇到问题可以尝试来这里找答案。

2\. Obsidian Advanced Obsidian URI 插件

3\. Obsidian Local Images Plus 插件

[![nyaaar](https://cdn.sspai.com/ui/otter_avatar_placeholder.png?imageMogr2/auto-orient/thumbnail/!84x84r/gravity/center/crop/84x84/format/webp/ignore-error/1)](https://sspai.com/u/var71qvc/updates)

[![](https://cdn.sspai.com/2023/2/7/article/c8656602-8fa9-e7d4-2c78-767c454bfce8.jpg?imageMogr2/auto-orient/thumbnail/!1096x252r/gravity/center/crop/1096x252/format/webp/ignore-error/1)](https://sspai.com/a/XJRq3n)
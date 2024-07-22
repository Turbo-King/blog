# Turbo-King 的 Blog

## DO NOT FORK!

> **此项目内容为个人博客。**
>
> **未经许可请勿直接使用本项目代码作为除技术学习以外任何用途。**

## 介绍

本项目基于 [Hugo](https://www.gohugo.org) 进行搭建，`Hugo`是由 Go 语言实现的静态网站生成器。具有简单、易用、高效、高扩展、快速部署等特点。

`Hugo`以速度快著称，号称是世界上最快的网站生成框架。

> **The world’s fastest framework for building websites**

<br>

## 选择原因

> 这里有一段来自 [1Password](https://1password.com/zh-cn) 的对`hugo`的评价，很有代表性
>
> 内容翻译自https://gohugo.io/showcase/1password-support/

在1Password，我们过去每个月都会经历一个不同的文档平台：博客引擎，电子书，维基，用Ruby和JavaScript编写的网站生成器。 每个方式都有自己的不足。然后我们找到了hugo。我们做了最后一次切换，我们很高兴我们做到了。

**并非所有静态站点生成器都是相同的**

找到一个能让您的客户，作者，设计师和DevOps团队满意的工具并非易事，但我们通过Hugo进行了管理：

- **Hugo是静态的**。我们是一家安全公司，所以我们坚持静态网站并尽可能地使用它们。我们觉得客户在HTML文件上比在需要强化的复杂服务器上更安全。
- **Hugo是基于Go的**。在1Password我们喜欢Go编程语言，我们很高兴得知Hugo使用了我们的设计师和前端开发人员已经掌握的相同的Go模板语法。
- **Hugo很快**。我们以前的静态站点生成器花费将近一分钟来编译我们的（那时还小得多）站点。开发人员可能已经习惯了这一点，但对于希望看到工作实时预览的作家来说，这并不合适。Hugo在几毫秒内完成了同样的工作，到现在，转瞬之间就可以编译五种语言编写的400多个页面。
- **Hugo很灵活**。感谢Hugo的内容和布局系统，我们能够保留现有的文件和文件夹结构，并在几天内移植整个生产站点。然后我们可以创建以前不可能的新内容类型，比如这些时髦的陈列柜。
- **Hugo非常适合作家**。我们的文档团队已经对Markdown和Git感到满意，并且可以开始为Hugo创建内容而无需停顿。一旦我们添加了短代码，我们的作者就可以通过少量的新语法来使用类似平台盒(platform boxes)的功能来装饰文章。
- **Hugo有一个惊人的开发者社区**。Hugo更新频繁，充满了功能和修复。在我们开发网站的多语言版本时，我们提交了所需功能的PR，并通过@bep和其他人的流程得到了帮助。
- **Hugo易于部署**。 Hugo拥有适量的配置选项，可以适应我们的构建系统而不会太复杂。

<br>

**个人选择原因主要体现在以下几个方面**

1. **静态文件生成**：生成纯静态文件，可以任意找地方托管，无任何特殊依赖。免费的github pages，廉价的虚拟主机，廉价的VPS，都可以轻松完成网站托管。
2. **markdown编写**：这是至关重要的一点，在习惯了markdown高效而专注的写作体验之后，markdown的支持成为硬性要求。
3. **使用方便**：无论是偏公共性质的技术博客，还是偏私人性质的学习笔记，从内容创作或者学习心得记录的角度，都是“内容为王”！因此，一个样式简单大方的网站，在一次性的安装调试完成之后，后续就应该可以轻松快捷的进行内容创作。
4. **速度！**：仅以上述三点而言，hexo和gitbook都表现的很好，两者的功能和主题都非常的丰富。但是，hugo在渲染速度上明显高出太多，尤其是对比gitbook，几乎有几十上百倍的性能差距。当站点内容比较多，比如积攒了几年的博客，或者大量细节展开的学习笔记，gitbook的渲染速度就开始从秒级到十几秒再到分钟级，而变得越来越不可接受。Hugo的速度，是我最终放弃hexo和gitbook的最关键的原因。
5. **Go语言**：这是讨人喜欢的一点，当然这会因人而异，nodejs的同学可能更愿意用gitbook。仅从个人感受上说，我对基于go的hugo好感更多。
6. **喜欢的主题**：对于hexo、gitbook、hugo，都拥有数以千计的各式主题。而要在众多主题中找到自己的心头好，有时需要一点运气。有特别喜欢的主题，是一个关键的决策依据。

<br>

## 安装使用

### [安装Hugo](https://skyao.io/learning-hugo/docs/installation/installation/)

Hugo在多个操作系统下的安装

### [快速开始](https://skyao.io/learning-hugo/docs/installation/quickstart/)

详细介绍通过Hugo快速开始搭建一个新的站点

### [开启HTTPS](https://skyao.io/learning-hugo/docs/installation/https/)

如何为Hugo增加HTTPS支持

### [搜索引擎优化（待更新）](https://skyao.io/learning-hugo/docs/installation/seo/)

为了让Hugo网站更好的被搜索引擎收录，需要进行搜索引擎优化

### [增加更多语言的语法高亮](https://skyao.io/learning-hugo/docs/installation/code-highlight/)

默认支持的编程语言有限，可以在需要时增加语言的支持，使之也可以做到语法高亮







© Since 2021 Turbo-King. All rights reserverd.

---
title: vim折腾记
categories:
  - vim
tags: vim
---

最近折腾了一圈 vim，在这里记录下折腾了啥。

### 为啥折腾 vim？

~~为了装 b~~

最开始是受到了@PegasusWang 刺激。用 vim 编写代码实在是太帅了。于是入坑开始了。

在支之后就是找了好多 vim 文章看。工作不忙的时候，代码都会使用 vim 来编写。现在算入门了吧，感觉在逐渐变好。

现在 IDE 功能确实非常完备，没啥必要去折腾这个，就算不用 IDE，前端还有 vscode 这个神器，我个人不太习惯使用
IDE，一直在使用 vscode，IDE
封装了太多功能，一旦出问题我就抓瞎了...可能是我比较菜把。

### 为啥自己配置 vim？

其实最开始是直接使用 @[ PegasusWang ](https://github.com/PegasusWang/vim-config) 的 vim
配置，可是他是后端程序员，而我是前端... 在之后有陆续尝试了 [ SpaceVim ](https://github.com/SpaceVim/SpaceVim) 的配置，[
ThinkVim ](https://github.com/hardcoreplayers/ThinkVim)
的配置等等，但是 vim 的基础知识的确实，导致使用他人的 vim 配置，出现的问题我没有办法修复，而且人家的配置非常成熟，学习他人的快捷键操作也是非常痛苦的过程。

于是就希望学习下 vim 知识，决定自己搞一个自己的 vim 配置。so...

### 折腾的成果

[ 链接 ](https://github.com/yongqi-zhang/nvim)

这个只能算是入门的入门，都是抄大神的配置，东拼西凑，配置了专属于我的 vim-配置。下面说一说特别值得说的。

#### Shougo/dein.vim

为插件提供懒加载的能力，可以有效提高 vim 开启速度，最常用的就是打开特定文件，加载相应的插件。

#### neoclide/coc.nvim

为 vim 带来了等同于 vscode 的代码补全能力，这也是保证 vim 体验的基石。启明大神威武。而且 coc
插件自成体系。里面有非常非常多的高质量插件。我的配置中。git 改动提示。代码语法错误等都是 coc 插件提供。

#### Shougo/defx.nvim

为 vim 带来了文件管理工具。体验与其他软件的文件管理工具一致。这个插件相比前辈的亮点是异步特性。PS:
这些配置也是抄袭上面那些大神的配置。

#### fzf.vim

fzf 本身是命令行搜索工具，go 语言编写，以速度快著称。fzf.vim 将 fzf
引进 vim，在我的配置中，项目文件搜索，项目全局搜索，tag 搜索，buffer 搜索，都是又此插件提供。

---

通过上面这些插件，使得 vim 有不输于现代编辑器的体验，加上 vim 本身的全键盘操作模式。使 vim 可以在今天依然有一席之地。

此外还有好多插件没有说到的，比如 ctag 为 vim 带来 tag 搜索和列表的基础能力。easymotion 为 vim
带来了另一种高效的定位光标位置的方式。vim-startify 为 vim 带来了优秀的开屏等等。正是拥有众多的插件。才有现在的 vim。

我对 vim 的期待并没有像 IDE 那样大而全，而是希望 vim 像 vscode
一样，保持一个良好的编辑器状态。同时，插件在提供一些体验优秀的编辑器增强功能。个人觉着之前使用编辑器的，切换到 vim，体验应该不会差太多。

还是感谢启明大神提供的 coc 插件。在之前，vim 的代码补全，特别是前端方向的代码补全，资料真是特别少。而 coc
几乎解决了这一切。。。

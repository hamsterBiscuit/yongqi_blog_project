---
title: 2021年vim配置
categories:
  - vim
tags: vim neovim
---

最近折腾了vim配置，查了很多资料，鉴于vim在中文论坛不是特别有话题度，而且网络上面的配置年头也有些久远，
所以在这记录下目前（2021年）的vim配置

- vim版本：nvim 0.5 Following HEAD.

mac 下安装：
```bash
brew install --HEAD luajit
brew install --HEAD neovim
```

### 为什么选择开发分支的nvim

master 分支的nvim相比稳定版本的nvim和vim，要多出很多功能。稳定性也不错。

#### 支持使用 `lua` 编写配置文件

<!-- more -->

相比`vimscript`，`lua`是一个脚本语言，只要我们使用过c语系语言，那看到lua会比看到vimsript更为友好。

关于如何使用`lua`配置nvim，国内已经有大神翻译编写了[中文文档](https://github.com/glepnir/nvim-lua-guide-zh)
文档不长，实际上，把现在的配置翻译为`lua`，可能只是需要几个方法即可

#### tree-sitter

增量语法生成树。
性能要比正则匹配的高亮要好，也拥有更好的拓展性，比如支持缩紧，跳转，文本对象等
有了这个，你可以卸载之前的语言高亮插件了
（ps：但是我工作使用Vue文件解析，目前还没有支持）

#### LSP支持

内置了lsp，果然在`vscode`生态下，vim的编程体验越来越好啦。

目前已经有了类似 [coc.nvim](https://github.com/neoclide/coc.nvim) 的实现

内置的lsp会有更好的性能，和更纯粹的体验

### 正在使用的插件

OK，说完了开发版本的nvim，再来看看当前在使用的插件

- [packer.nvim](https://github.com/wbthomason/packer.nvim)

`lua`编写的包管理工具，基本该有的功能都有了，配置也很方便

- [akinsho/nvim-bufferline.lua](https://github.com/akinsho/nvim-bufferline.lua)

基于`lua`编写的 `buffer`栏插件

- [glepnir/galaxyline.nvim](https://github.com/glepnir/galaxyline.nvim)

基于`lua`编写的 状态栏插件

- [glepnir/dashboard-nvim](https://github.com/glepnir/dashboard-nvim)

非常轻量的仪表盘插件

- [rhysd/accelerated-jk](https://github.com/rhysd/accelerated-jk)

加快jk的插件

- [hrsh7th/vim-eft](https://github.com/hrsh7th/vim-eft)

增强`f|t`操作，在摁下`f|t`时，会高亮可能需要跳转的字母

- [tomtom/tcomment_vim](https://github.com/tomtom/tcomment_vim)

非常轻量的注释插件

- [psliwka/vim-smoothie](https://github.com/psliwka/vim-smoothie)

将 `<C-d><C-f>` 这类变为滚动翻页效果插件

- [kana/vim-operator-user](https://github.com/kana/vim-operator-user)
- [rhysd/vim-operator-surround](https://github.com/rhysd/vim-operator-surround)

轻量的增删改引号的插件

- [skywind3000/vim-terminal-help](https://github.com/skywind3000/vim-terminal-help)
- [skywind3000/asynctasks.vim](https://github.com/skywind3000/asynctasks.vim)
- [skywind3000/asyncrun.vim](https://github.com/skywind3000/asyncrun.vim)

这三个放在一起介绍了，

`vim-terminal-help` 优化了内置`term`的使用体验

`asyncrun`利用了异步机制，可以异步执行命令

`asynctasks`实现了类似vscode的任务系统

- [glepnir/indent-guides.nvim](https://github.com/glepnir/indent-guides.nvim)

基于lua的缩进线插件

- [itchyny/vim-cursorword](https://github.com/glepnir/itchyny/vim-cursorword)

提供了光标下划线和高亮

- [norcalli/nvim-colorizer.lua](https://github.com/norcalli/nvim-colorizer.lua)

荧光笔插件，写css啊，很有用

- [nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)

搜索插件

- [glepnir/zephyr-nvim](https://github.com/glepnir/zephyr-nvim)

主题

- [nvim-treesitter/nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)

语法高亮插件

- [kyazdani42/nvim-tree.lua](https://github.com/kyazdani42/nvim-tree.lua)

语法高亮插件

- [neovim/nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)

lsp配置插件，可以帮助你快速配置lsp

- [nvim-lua/completion-nvim](https://github.com/nvim-lua/completion-nvim)
- [hrsh7th/vim-vsnip](https://github.com/hrsh7th/vim-vsnip)

基于lsp的补全插件,

`vsnip` 代码片段插件，可以自定义代码片段，甚至可以直接使用vscode的代码片段插件！！！

- [mhartington/formatter.nvim](https://github.com/mhartington/formatter.nvim)

格式化插件

- [liuchengxu/vista.vim](https://github.com/mhartington/liuchengxu/vista.vim)

tags栏插件，用的不多

- [mhinz/vim-signify](https://github.com/mhinz/vim-signify)

git 状态显示插件

- [editorconfig/editorconfig-vim](https://github.com/editorconfig/editorconfig-vim)

编辑器配置插件。

- [iamcco/markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim)

同步预览MD

- [posva/vim-vue](https://github.com/posva/vim-vue)

vue 文件语法高亮

- [mattn/emmet-vim](https://github.com/mattn/emmet-vim)

emmet 写html用的

 - [alvan/vim-closetag](https://github.com/alvan/vim-closetag)

在html环境下，会自动闭合标签

可以看到，插件这里，能换成lua实现的，都换成了lua实现，这里有点强迫症了。
就体验来说，并不是每一个都好于vim写的插件的。

### 为什么用vim

纯键盘coding很爽。

我是一个vim小白，能配置出这么一套配置，肯定是借鉴了各位大佬的配置，其中，借鉴的最多的就是这个大佬的[配置](https://github.com/glepnir/nvim)，一位中国的大神，目前也是neovim的核心开发者，欢迎大家关注.

最后，列出有关这里提到的一些链接

[awesome-neovim](https://github.com/glepnir/awesome-neovim)
[中文速查表](https://github.com/skywind3000/awesome-cheatsheets)
[neovim](https://github.com/neovim/neovim)

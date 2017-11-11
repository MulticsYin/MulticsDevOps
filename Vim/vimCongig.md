# 我的vim配置

[配置文件](https://github.com/MulticsYin/MulticsDevOps/blob/master/Configuration_file/vimrc)  

图片仅仅为演示所用，相信现实中不会有人这么写代码吧？

如图：  
![](https://github.com/MulticsYin/MulticsDevOps/blob/master/picture/sereen00.png)


# 快捷键说明

1. NERDTree有按横向纵向布局模式打开文件的快捷，直接看help信息即可。
2. tab切换使用ngt即可。例如切换到第二个tab页，输入2gt。
3. 支持pycharm中的代码块缩进操作（使用tab、s-Tab）。
4. 支持全局搜索替换（多个文件搜索替换）。

## 自定义快捷键

| 命令 | 说明 |
| :--: | :--: |
| sv \<filename\> | 打开一个文件，纵向分割布局（新文件会在当前文件下方界面打开）|
| vs \<filename\> | 横向分割布局（新文件会在当前文件右侧界面打开）|
| Ctrl-h | 切换到左侧的分割窗口 |
| Ctrl-j | 切换到下方的分割窗口 |
| Ctrl-l | 切换到右侧的分割窗口 |
| Ctrl-k | 切换到上方的分割窗口 |
| Alt-h | 减小当前窗口的宽度 |
| Alt-j | 减小当前窗口的高度 |
| Alt-l | 增加当前窗口的高度 |
| Alt-k | 增加当前窗口的宽度 |
| Ctrl-g | 跳转到函数定义或者声明 |
| Ctrl-y, | emmet自动补全快捷 |
| -- | -- |
| F2 | 打开or关闭行号，同时打开or关闭gitgutter（文件变化提示） |
| F3 | 打开or关闭复制支持 |
| F4 | 折叠or展开代码（默认打开文件不折叠） |
| F5 | 打开or关闭目录树 |
| F6 | 打开or关闭语法检查（大文件时影响性能） |
| F7 | flake8 check |
| F8 | Glog，展示文件的git history |
| F9 | 配合Glog，查看文件前一个版本 |
| f10 | 配合Glog，查看文件后一个版本 |
| -- | -- |
| space | 折叠/展开代码 |
| -- | -- |
| Shift-i | 目录是否显示隐藏文件（NERDTree）。便于git开发，默认永远不显示.git。 |

## 跳转

| 命令 | 说明 |
| :--: | :--: |
| Ctrl-o | jump back to where you where before invoking the command 和 Ctrl-g、ctrl-i 配合使用 |
| ctrl-i | jump forward |
| ctrl-^ | 跳转到上一个编辑的文件 |
| -- | -- |
| zz | move current line to the middle of the screen |
| zt | move current line to the top of the screen |
| zb | move current line to the bottom of the screen |

## 搜索

| 命令 | 说明 |
| :--: | :--: |
| :/pattern\<CR\> | 搜索所有包含pattern的单词（向上搜索） |
| :?pattern\<CR\> | 搜索所有包含pattern的单词（向下搜索） |
| n | 朝同一方向搜索 |
| N | 反方向搜索 |
| :/ pattern\<CR\> | 单词前加空格，精确匹配 |
| :/^pattern\<CR\> | 搜索仅在行首出现 |
| :/pattern$\<CR\> | 搜索仅在行末出现 |
| \\^ \\$ | 特殊字符的搜索加反斜杠 |

## 搜索替换

[http://vim.wikia.com/wiki/Search_and_replace](http://vim.wikia.com/wiki/Search_and_replace)
[http://vim.wikia.com/wiki/Search_and_replace_in_multiple_buffers](http://vim.wikia.com/wiki/Search_and_replace_in_multiple_buffers)

| 命令 | 说明 |
| :-- | :--: |
| :s/foo/bar/g | Change each 'foo' to 'bar' in the current line. |
| :%s/foo/bar/g | Change each 'foo' to 'bar' in all the lines. |
| :5,12s/foo/bar/g | Change each 'foo' to 'bar' for all lines from line 5 to line 12 (inclusive). |
| :'a,'bs/foo/bar/g | Change each 'foo' to 'bar' for all lines from mark a to mark b inclusive (see Note below). |
| :'<,'>s/foo/bar/g | When compiled with +visual, change each 'foo' to 'bar' for all lines within a visual selection. Vim automatically appends the visual selection range ('<,'>) for any ex command when you select an area and enter :. Also, see Note below. |
| :.,$s/foo/bar/g | Change each 'foo' to 'bar' for all lines from the current line (.) to the last line ($) inclusive. |
| :.,+2s/foo/bar/g | Change each 'foo' to 'bar' for the current line (.) and the two next lines (+2). |
| :g/^baz/s/foo/bar/g | Change each 'foo' to 'bar' in each line starting with 'baz'. |
| -- | -- |
| :arg \*.py | All \*.py files in current directory. |
| :argadd \*.md | And all  \*.md files. |
| :arg | Optional: Display the current arglist. |
| :argdo %s/pattern/replace/ge \| update | Search and replace in all files in arglist. |


## 删除

| 命令 | 说明 |
| :--: | :--: |
| x | 删除当前光标处的字符 |
| X | 删除光标左边的字符 |
| D | 删除从当前光标到本行末尾的字符 |
| J | 删除两行之间的换行符 (亦可用于合并两行）|
| dmove | 删除从当前光标到move所给位置的字符 |
| dd | 删除当前行 |
| :line**d** | 删除指定行 |
| :line,line**d** | 删除指定范围内的行 |


## [返回目录](https://github.com/MulticsYin/MulticsDevOps)

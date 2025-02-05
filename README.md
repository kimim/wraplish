# Wraplish

Wraplish 是一个在 Unicode 与英文之间加上空格的 Emacs 插件。

这个插件类似于 [pangu-spacing](https: //github.com/coldnew/pangu-spacing) 的效果， 主要的区别是 Wraplish 使用 Python 多线程技术来分析 Emacs 的文本， 避免 Elisp 代码分析大文本时产生过多的 GC 对象卡住 Emacs。

## 原理

Wraplish 的原理如下：

1. 每次编辑 Buffer 内容后， Elisp 端发送 Diff 序列到 Wraplish 的 Python 进程
2. Python 端根据 Diff 序列重建 Buffer 的内容， 并分析出需要插入空格的位置列表返回 Emacs
3. Emacs 再根据空格位置列表， 找到对应的位置， 从后向前填充空格

## 安装

1. 安装 Emacs 28 及以上版本
2. 安装 Python 依赖: `pip3 install epc sexpdata six`
3. 用 `git clone` 下载此仓库， 并替换下面配置中的 load-path 路径
4. 把下面代码加入到你的配置文件 ~/.emacs 中：

```elisp
(add-to-list 'load-path "<path-to-wraplish>")

(require 'wraplish)

(dolist (hook (list 'markdown-mode-hook))
  (add-hook hook #'(lambda () (wraplish-mode 1))))
```

## 选项
* `wraplish-add-space-after-chinese-punctuation`: 在中文标点后面添加空格， 默认不开启, 我们阅读很多中文纸质书籍的时候， 你会发现， 这些书籍为了提高阅读的流畅性， 他们除了中英文单词之间添加空格外， 它们还会在中文标点后面增加空格， 推荐大家在自己的中文文章中开启这个选项
* `wraplish-add-space-before-markdown-link`: 如果 Markdown 链接的首字母是英文， 并且 Markdown 链接左边是 Unicode， 在 Markdown 链接之前添加空格， 默认开启

## 反馈问题

关于一些常用问题， 请先阅读
[Wiki](https: //github.com/manateelazycat/wraplish/wiki)

请用命令 `emacs -q` 并只添加 Wraplish 配置做一个对比测试， 如果 `emacs -q` 可以正常工作， 请检查你个人的配置文件。

如果 `emacs -q` 环境下问题依旧， 请到[这里](https: //github.com/manateelazycat/wraplish/issues/new)反馈, 并附带 `*wraplish*` 窗口的内容给我们提交 issue， 那里面有很多线索 可以帮助我们排查问题。

- 如果你遇到崩溃的问题, 请用下面的方式来收集崩溃信息:

  1. 先安装 gdb 并打开选项 `(setq wraplish-enable-debug t)`
  2. 使用命令 `wraplish-restart-process` 重启 WRAPLISH 进程
  3. 在下次崩溃时发送 `*wraplish*` 的内容

- 如果你遇到其他问题， 请用下面的方式来收集信息
  1. 打开选项 `(setq wraplish-enable-log t)`
  2. 使用命令 `wraplish-restart-process` 重启 WRAPLISH 进程
  3. 发送`*wraplish*`中的内容

## 贡献者

<a href = "https: //github.com/manateelazycat/wraplish/graphs/contributors">
  <img src = "https: //contrib.rocks/image?repo=manateelazycat/wraplish"/>
</a>

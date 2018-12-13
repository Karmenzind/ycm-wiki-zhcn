插件介绍
========

**Vim代码补全引擎YouCompleteMe**

[![Gitter room](https://img.shields.io/gitter/room/Valloric/YouCompleteMe.svg)](https://gitter.im/Valloric/YouCompleteMe)
[![Linux build status](https://img.shields.io/travis/Valloric/YouCompleteMe/master.svg?label=Linux)](https://travis-ci.org/Valloric/YouCompleteMe)
[![macOS build status](https://img.shields.io/circleci/project/github/Valloric/YouCompleteMe/master.svg?label=macOS)](https://circleci.com/gh/Valloric/YouCompleteMe)
[![Windows build status](https://img.shields.io/appveyor/ci/Valloric/YouCompleteMe/master.svg?label=Windows)](https://ci.appveyor.com/project/Valloric/YouCompleteMe)
[![Coverage status](https://img.shields.io/codecov/c/github/Valloric/YouCompleteMe/master.svg)](https://codecov.io/gh/Valloric/YouCompleteMe)


帮助、建议及支持
---------------------

如果在使用 YCM 过程中遇到问题，需要帮助、建议和支持：

请先仔细阅读与你操作系统对应的[installation instructions](#installation)。
推荐使用我们提供的`install.py`脚本进行安装。

然后根据你使用的sematic completer查看[用户指南](#user-guide)。如果你使用C/C++/Objective-C/Objective-C++/CUDA，请 _务必_ 阅读[这一节](#c-family-semantic-completion).

最后，查看[问答列表](#faq)。

如果依然无法解决问题，看这里[contacts](#contact)。

请 **不要** 去freenode的#vim寻求帮助。
请根据[此处提供的联系方式](#contact)，直接联系YouCompleteMe项目维护者。


简介
--------


YouCompleteMe_ 是个快速、即时响应并支持模糊搜索的 Vim_ 代码补全引擎。它包含了如下几个补全引擎：

.. _YouCompleteMe:
.. _Vim:


- 一个基于标识符的引擎，支持所有编程语言；
- 一个基于[Clang][] 的引擎，为 C/C++/Objective-C/Objective-C++/CUDA （*后续简称“C系语言”*）提供 native sematic code 补全；
- 一个基于Jedi 的引擎，用于补全 Python 2 和 3；
- 一个基于OmniSharp 的引擎，用于补全 C# ；
- 一个结合了 Gocode 和 Godef 的语义引擎，用于 Go 补全；
- 一个基于 TSServer 的引擎，用于 JavaScript 和 TypeScript 补全；
- 一个基于 racer 的 Rust 补全引擎；
- 一个基于 jdt.ls 的实验中的 Java 补全引擎；
- 一个基于 omnifunc 的引擎，使用 Vim 的 omnicompelete 系统为许多其他预言（Ruby、PHP等）提供补全支持


![YouCompleteMe GIF demo](http://i.imgur.com/0OP4ood.gif)

动图 demo 说明：

首先注意，demo 中全程 **无需借助键盘快捷键** 去触发补全列表。用户输入时，补全建议会自动出现。如果找不到所需的补全项或需要继续输入，则继续打字即可，引擎不会造成干扰。

当看到有用的补全项，按下 TAB 去选择，这个字符串就被插入到了文本中。持续按 TAB 可以在补全列表中循环选择。

如果提供的补全建议不够相关，用户可以输入更多字符，过滤掉不想要的补全项。

一个重点： **补全项的过滤并非基于所输入字符串前缀** （尽管起到了一定作用）。输入需要满足补全项的 `子序列 <https://en.wikipedia.org/wiki/Subsequence>`_ 匹配。换句话说，输入的字符顺序要和它们出现在补全项中的顺序一致。所以 ``abc`` 是 ``xaybgc`` 的子序列，但不是 ``xbyxaxxc`` 的。过滤后，一个复杂的排序系统会对补全列表进行排序，相关度最高的选项会出现在菜单顶部（所以通常你只需要按一次 TAB）。

得益于基于标识符的补全引擎，**上述几点支持任何编程语言**。引擎会收集当前文件和你访问过的其他文件（以及 tags 文件）中的所有标识符，当你输入的时候对它们进行搜索（标识符会根据文件类型分组）。

demo 也展示了实际使用中的的语义引擎。当用户在输入模式下打出 ``.`` 、 ``->`` 或者 ``::`` （此处为 C++ ；其他的语言有不同的触发方式），语义引擎随即触发（也可以用快捷键来触发；见下文）。

最后，你可以看到 YCM 针对 C 系文件的诊断展示特性（出现在左端的小红X；受 Syntastic_ 启发）。 Clang 编译你的文件时检测到警告和错误，会以多种方式展现出来。你不需要通过保存文件或者快捷键去触发它，它会在后台自然“发生”。

大体上， YCM 可以淘汰下列 Vim 插件，包含了它们所有的特性，有过之而无不及：

- clang_complete
- AutoComplPop
- Supertab
- neocomplcache

**以及……**

YCM 还对许多语言提供了 `基于语义的仿IDE特性`__ ：

__ `特性速览`_

- 查找标识符的声明、定义和使用等；
- 展示类、变量、函数等的类型信息；
- 语言方法、成员等的文档。
- 修复常见代码错误，比如确实的分号、拼写错误等；
- 基于语义的跨文件变量重命名；
- 代码格式化；
- 去除无用引入（import）、给引入排序等；

特性因文件类型而异，所以确保阅读 `特性速览`_ 和 `completer 子命令全列表`_ 以找到适用于你心爱语言的结果。

你也会发现， YCM 还拥有文件路径 completer（尝试在文件中输入 ``./`` ）和集成了 `UltiSnips`_ 的 completer 。

.. _UltiSnips: 

------

安装
----

Mac OS X
~~~~~~~~~


此处（使用 ``install.py`` ）是安装 YCM 最快捷的方式，但可能并不适用于所有人。如果以下教程对你不奏效，移步 `完整安装指导`_ 。

安装 MacVim_ 最新版。对，MacVim， **最新版** 。

.. _MacVim:

如果你不用 GUI 版本，建议选择 MacVim.app 包（ ``MacVim.app/Contents/MacOS/Vim`` ）中的 Vim 二进制文件。为确保生效，从下载好的 MacVim_ 中正确复制 ``mvim`` 脚本到你的二进制文件目录（例如 ``/usr/local/bin/mvim`` ）然后创建链接：

.. code-block:: bash

    ln -s /usr/local/bin/mvim vim


用 Vundle_ 安装 YCM 。

.. _Vundle:

**谨记：** YCM 插件包含编译成分。如果你用 Vundle **更新** 了 YCM 而 ycm_core 库的 API 发生变化（很少发生）， YCM 会提醒你重新编译。那么你需要重新走一遍安装流程。

**注意：** 如果你需要 C 系语言补全，你 **必须** 安装最新版 Xcode 搭配最新版 Command Line Tools （首次运行 ``clang`` 时会自动安装，或者运行 ``xcode-select --install`` 来手动安装 ）。

安装 CMake 。首选 HomeBrew_ ，这里有[stand-alone CMake installer][cmake-download].  

.. _HomeBrew:

`如果` 你已经通过 HomeBrew 安装了 Python 和/或 MacVim ，查看 `FAQ` 。

编译 **包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --clang-completer


编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py


以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。

.. HERE


完整安装指导
------------

特性速览
--------

completer 子命令全列表
------------------------

blabla

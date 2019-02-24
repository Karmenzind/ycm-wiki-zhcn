README 文档
==========

**Vim代码补全引擎YouCompleteMe**

|Gitter room|_ |Linux build status|_ |macOS build status|_ |Windows build status|_ |Coverage status|_


帮助、建议及支持
---------------------

如果在使用 YCM 过程中遇到问题，需要帮助、建议和支持：

请先仔细阅读与你操作系统对应的 `安装指南`__ 。

__ `安装`_

推荐使用我们提供的`install.py`脚本进行安装。

然后根据你使用的sematic completer查看 `用户指南`__ 。如果你使用C/C++/Objective-C/Objective-C++/CUDA，请 _务必_ 阅读 `这一节`__ 。

__ `用户指南`_
__ `C系语言语义补全`_

最后，查看 `FAQ`__ 。

__ `FAQ`_

如果依然无法解决问题， `看着里`__ 。

__ `联系方式`_

请 **不要** 去freenode的#vim寻求帮助。
请根据 `此处提供的联系方式`__ ，直接联系YouCompleteMe项目维护者。

__ `联系方式`_

简介
--------

YouCompleteMe_ 是个快速、即时响应并支持模糊搜索的 Vim_ 代码补全引擎。它包含了如下几个补全引擎：

.. _YouCompleteMe:
.. _Vim:


- 一个基于标识符的引擎，支持所有编程语言；
- 一个基于 Clang_ 的引擎，为 C/C++/Objective-C/Objective-C++/CUDA （*后续简称“C系语言”*）提供 native sematic code 补全；
- 一个基于 clangd_ 的 **实验阶段的** C系语言补全引擎；
- 一个基于Jedi 的引擎，用于补全 Python 2 和 3；
- 一个基于OmniSharp 的引擎，用于补全 C# ；
- 一个结合了 Gocode 和 Godef 的语义引擎，用于 Go 补全；
- 一个基于 TSServer 的引擎，用于 JavaScript 和 TypeScript 补全；
- 一个基于 racer 的 Rust 补全引擎；
- 一个基于 jdt.ls 的实验中的 Java 补全引擎；
- 一个基于 omnifunc 的引擎，使用 Vim 的 omnicompelete 系统为许多其他预言（Ruby、PHP等）提供补全支持


|YouCompleteMe GIF demo|

.. |YouCompleteMe GIF demo| image:: http://i.imgur.com/0OP4ood.gif


`如果动图未显示，点此跳转 <http://i.imgur.com/0OP4ood.gif>`_


..    alt: YouCompleteMe GIF demo

动图 demo 说明：

首先注意，demo 中全程 **无需借助键盘快捷键** 去触发补全列表。用户输入时，补全建议会自动出现。如果找不到所需的补全项或需要继续输入，则继续打字即可，引擎不会造成干扰。

当看到有用的补全项，按下 TAB 去选择，这个字符串就被插入到了文本中。持续按 TAB 可以在补全列表中循环选择。

如果提供的补全建议不够相关，用户可以输入更多字符，过滤掉不想要的补全项。

一个重点： **补全项的过滤并非基于所输入字符串前缀** （尽管起到了一定作用）。输入需要满足补全项的 `子序列 <https://en.wikipedia.org/wiki/Subsequence>`_ 匹配。换句话说，输入的字符顺序要和它们出现在补全项中的顺序一致。所以 ``abc`` 是 ``xaybgc`` 的子序列，但不是 ``xbyxaxxc`` 的。过滤后，一个复杂的排序系统会对补全列表进行排序，相关度最高的选项会出现在菜单顶部（所以通常你只需要按一次 TAB）。

得益于基于标识符的补全引擎，**上述几点支持任何编程语言**。引擎会收集当前文件和你访问过的其他文件（以及 tags 文件）中的所有标识符，当你输入的时候对它们进行搜索（标识符会根据文件类型分组）。

demo 也展示了实际使用中的的语义引擎。当用户在输入模式下打出 ``.`` 、 ``->`` 或者 ``::`` （此处为 C++ ；其他的语言有不同的触发方式），语义引擎随即触发（也可以用快捷键来触发；见下文）。

最后，你可以看到 YCM 针对 C 系文件的诊断展示特性（出现在左端的小红X；受 Syntastic_ 启发）。 Clang 编译你的文件时检测到警告和错误，会以多种方式展现出来。你不需要通过保存文件或者快捷键去触发它，它会在后台自然“发生”。

大体上， YCM 可以淘汰下列 Vim 插件，因为 YCM 包含了它们所有的特性，而且做得更多更好：

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

安装
-----

Mac OS X
~~~~~~~~~


此处（使用 ``install.py`` ）是安装 YCM 最快捷的方式，但可能并不适用于所有人。如果以下教程对你不奏效，移步 `完整安装指导`_ 。

安装 MacVim_ 最新版。对，MacVim， **最新版** 。

.. _MacVim:

如果你不用 GUI 版本，建议选择 MacVim.app 包（ ``MacVim.app/Contents/MacOS/Vim`` ）中的 Vim 二进制文件。为确保生效，从下载好的 MacVim_ 中正确复制 ``mvim`` 脚本到你的二进制文件目录（例如 ``/usr/local/bin/mvim`` ）然后创建链接：

.. code-block:: bash

    ln -s /usr/local/bin/mvim vim

用 `Vundle <vundle_>`_ 安装 YCM 。

**谨记：** YCM 插件包含编译组件。如果你用 Vundle **更新** 了 YCM 而 ycm_core 库的 API 发生变化（很少发生）， YCM 会提醒你重新编译。那么你需要重新走一遍安装流程。

**注意：** 如果你需要 C 系语言补全，你 **必须** 安装最新版 Xcode 搭配最新版 Command Line Tools （首次运行 ``clang`` 时会自动安装，或者运行 ``xcode-select --install`` 来手动安装 ）。

安装 CMake 。首选 HomeBrew_ ，这里有[stand-alone CMake installer][cmake-download].  

.. _HomeBrew:

`如果` 你已经通过 HomeBrew 安装了 Python 和/或 MacVim ，查看 `FAQ` 。

编译 **包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --clang-completer


编译 **包含** 基于 **实验阶段的 clangd** 的C系语言语义支持：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --clangd-completer

注意，你可以安装同时带有 **libclang** 和 **clangd** 的 YCM ， **clangd** 会成为首选，除非你在 ``vimrc`` 中加上：

.. code-block:: vim

    let g:ycm_use_clangd = "Never"

编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py


以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `Rust <rust-install_>`_ ，并在执行 ``install.py`` 时添加 ``--rust-completer`` 。
- Java 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

如果要一次性编译所有特性，则加上 ``--all`` 参数。如果要安装所有语言特性，确保在 ``PATH`` 路径下安装了 ``xbuild``, ``go``, ``tsserver``, ``node``,
``npm`` , ``rustc``, 和 ``cargo`` 然后直接运行：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --all

搞定。查阅 `用户指南` 了解 YCM 的用法。不要忘记，如果你需要 C 系语言的补全引擎正常工作，则需要对 YCM 提供你的项目的 compilation flags 。这些都可以在用户指南中找到。

YCM 拥有健全的默认配置，但你可能依然想要看看可选的配置细节。基于谨慎考虑，一些有趣的配置默认为关闭状态，而你可能想要开启它们。

Linux 64-bit
~~~~~~~~~~~~

此处（使用 ``install.py`` ）是安装 YCM 最快捷的方式，但可能并不适用于所有人。如果以下教程对你不奏效，移步 `完整安装指导`_ 。

确定你已经安装了附带 Python 2 或 3 支持的 Vim 7.4.1578 。 Fedora 27 或更高版本上的 Vim 包以及 Ubuntu 16.04 或更高版本上预装的 Vim 版本都已经足够新。你可以用 ``vim --version`` 来查看所安装 Vim 的版本。如果版本太旧，你可能需要 `从源码编译 Vim <vim-build_>`_ （不用担心，很简单）。

用 `Vundle <vundle_>`_ 安装 YCM 。

**谨记：** YCM 插件包含编译组件。如果你用 Vundle **更新** 了 YCM 而 ycm_core 库的 API 发生变化（很少发生）， YCM 会提醒你重新编译。那么你需要重新走一遍安装流程。

安装开发工具包， Cmake 和 Python 头文件：

- Fedora 27 和更高版本：

.. code-block:: bash

    sudo dnf install cmake gcc-c++ make python3-devel

- Ubuntu 14.04:

.. code-block:: bash

    sudo apt install build-essential cmake3 python3-dev

- Ubuntu 16.04 和更高版本：

.. code-block:: bash

    sudo apt install build-essential cmake python3-dev

编译 **包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    python3 install.py --clang-completer

编译 **包含** 基于 **实验阶段的 clangd** 的C系语言语义支持：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --clangd-completer

注意，你可以安装同时带有 **libclang** 和 **clangd** 的 YCM ， **clangd** 会成为首选，除非你在 ``vimrc`` 中加上：

.. code-block:: vim

    let g:ycm_use_clangd = "Never"

编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    python3 install.py

以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `Rust <rust-install_>`_ ，并在执行 ``install.py`` 时添加 ``--rust-completer`` 。
- Java 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

如果要一次性编译所有特性，则加上 ``--all`` 参数。注意，这个 flag **不** 安装 **clangd** ，你需要手动添加 ``--clangd-completer`` 。如果要安装所有语言特性，确保在 ``PATH`` 路径下安装了 ``xbuild``, ``go``, ``tsserver``, ``node``,
``npm`` , ``rustc``, 和 ``cargo`` 然后直接运行：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    python3 ./install.py --all

搞定。查阅 `用户指南` 了解 YCM 的用法。不要忘记，如果你需要 C 系语言的补全引擎正常工作，则需要对 YCM 提供你的项目的 compilation flags 。这些都可以在用户指南中找到。

YCM 拥有健全的默认配置，但你可能依然想要看看可选的配置细节。基于谨慎考虑，一些有趣的配置默认为关闭状态，而你可能想要开启它们。

Windows
~~~~~~~~~

此处（使用 ``install.py`` ）是安装 YCM 最快捷的方式，但可能并不适用于所有人。如果以下教程对你不奏效，移步 `完整安装指导`_ 。

**重要：** 我们假设你在使用 ``cmd.exe`` 命令行而且你知道怎么把可执行文件添加到 PATH 环境变量。

确定你已经安装了附带 Python 2 或 3 支持的版本不低于 7.4.1578 的 Vim 。你可以在 Vim 中用 ``:version`` 来查看所安装 Vim 的版本和 Python 支持情况。 Python 2 对应包含 ``+python/dyn`` ， Python 3 对应包含 ``+python3/dyn`` 。要留意 Vim 的架构是32 还是 64-bit ，这对后续选择 Python 安装包很重要。我们推荐使用 64-bit 的客户端。这里提供了可供下载的 `日常更新的支持 Python 2 和 3 的 32-bit 及 64-bit 的 Vim 备份 <vim-win-download_>`_ 。

在 vimrc_ 中加上这一行：

.. code-block:: bash

    set encoding=utf-8

YCM 需要这一项。注意，这并不能阻止你编辑非 UTF-8 编码的文件，你可以在 ``:e`` 命令中指定 `++enc`_ 参数。

用 `Vundle <vundle_>`_ 安装 YCM 。

**谨记：** YCM 插件包含编译组件。如果你用 Vundle **更新** 了 YCM 而 ycm_core 库的 API 发生变化（很少发生）， YCM 会提醒你重新编译。那么你需要重新走一遍安装流程。

下载安装如下软件：

- `Python 2 或 Python 3 <python-win-download_>`_ 。确定依据你的 Vim 架构来选择版本， `Windows x86` 对应 32-bit Vim ， `Windows x86-64` 对应 64-bit Vim 。我们推荐安装 Python 3 。另外 你安装的 Python 版本必须和 Vim 所寻找的 Python 版本相匹配。输入 ``:version`` 查看页面地步的编译器 flags 列表。找到类似 ``-DDYNAMIC_PYTHON_DLL=\"python27.dll\"`` 和 ``-DDYNAMIC_PYTHON3_DLL=\"python35.dll\"`` 的 flags 。前者说明 Vim 在寻找 Python 2.7 ，后者说明 Vim 在寻找 Python 3.5 。你需要安装其中之一，并正确匹配版本号。
- `CMake <cmake-download_>`_ 。添加 CMake 可执行文件到 PATH 环境变量。
- `Visual Studio <visual-studio-download_>`_ 。下载社区版。安装过程中，在 `Workloads` 中选择 `Desktop development with C++` 。

编译 **包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: cmd

    cd %USERPROFILE%/vimfiles/bundle/YouCompleteMe
    python install.py --clang-completer

编译 **包含** 基于 **实验阶段的 clangd** 的C系语言语义支持：

.. code-block:: bash

    cd %USERPROFILE%/vimfiles/bundle/YouCompleteMe
    python install.py --clangd-completer

注意，你可以安装同时带有 **libclang** 和 **clangd** 的 YCM ， **clangd** 会成为首选，除非你在 ``vimrc`` 中加上：

.. code-block:: vim

    let g:ycm_use_clangd = "Never"

编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: cmd

    cd %USERPROFILE%/vimfiles/bundle/YouCompleteMe
    python install.py

以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `Rust <rust-install_>`_ ，并在执行 ``install.py`` 时添加 ``--rust-completer`` 。
- Java 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

如果要一次性编译所有特性，则加上 ``--all`` 参数。注意，这个 flag **不** 安装 **clangd** ，你需要手动添加 ``--clangd-completer`` 。如果要安装所有语言特性，确保在 ``PATH`` 路径下安装了 ``msbuild``, ``go``, ``tsserver``, ``node``,
``npm`` 和 ``cargo`` 然后直接运行：

.. code-block:: cmd

    cd %USERPROFILE%/vimfiles/bundle/YouCompleteMe
    python install.py --all

你可以用 ``--msvc`` 参数来指定 Microsoft Visual C++ (MSVC) 。 YCM 官方支持 MSVC 14 (Visual Studio 2015) 和 15 (2017) 。

搞定。查阅 `用户指南` 了解 YCM 的用法。不要忘记，如果你需要 C 系语言的补全引擎正常工作，则需要对 YCM 提供你的项目的 compilation flags 。这些都可以在用户指南中找到。

YCM 拥有健全的默认配置，但你可能依然想要看看可选的配置细节。基于谨慎考虑，一些有趣的配置默认为关闭状态，而你可能想要开启它们。


FreeBSD/OpenBSD
~~~~~~~~~~~~~~~~

此处（使用 ``install.py`` ）是安装 YCM 最快捷的方式，但可能并不适用于所有人。如果以下教程对你不奏效，移步 `完整安装指导`_ 。

**注意：** YCM 官方并没有正式支持 OpenBSD / FreeBSD 。

确定你已经安装了附带 Python 2 或 3 支持的版本不低于 7.4.1578 的 Vim 。

OpenBSD 5.5 及之后的版本都自带了最近版本的 Vim 。你可以在 Vim 中用 ``:version`` 来查看所安装 Vim 的版本。

FreeBSD 11.x 需要安装 cmake ：

.. code-block:: cmd

    pkg install cmake

用 `Vundle <vundle_>`_ 安装 YCM 。

**谨记：** YCM 插件包含编译组件。如果你用 Vundle **更新** 了 YCM 而 ycm_core 库的 API 发生变化（很少发生）， YCM 会提醒你重新编译。那么你需要重新走一遍安装流程。

编译 **包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --clang-completer

编译 **包含** 基于 **实验阶段的 clangd** 的C系语言语义支持：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --clangd-completer

注意，你可以安装同时带有 **libclang** 和 **clangd** 的 YCM ， **clangd** 会成为首选，除非你在 ``vimrc`` 中加上：

.. code-block:: vim

    let g:ycm_use_clangd = "Never"

编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py

如果系统中没有 ``python`` 可执行文件，或者默认的 ``python`` 不是编译需要的版本，则需要明确指定 python 解释器：

.. code-block:: bash

    python3 install.py --clang-completer

以下为可用的附加语言支持选项：

- C# 支持：安装 Mono 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `Rust <rust-install_>`_ ，并在执行 ``install.py`` 时添加 ``--rust-completer`` 。
- Java 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

如果要一次性编译所有特性，则加上 ``--all`` 参数。注意，这个 flag **不** 安装 **clangd** ，你需要手动添加 ``--clangd-completer`` 。如果要安装所有语言特性，确保在 ``PATH`` 路径下安装了 ``xbuild``, ``go``, ``tsserver``, ``node``,
``npm`` , ``rustc``, 和 ``cargo`` 然后直接运行：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py --all

搞定。查阅 `用户指南` 了解 YCM 的用法。不要忘记，如果你需要 C 系语言的补全引擎正常工作，则需要对 YCM 提供你的项目的 compilation flags 。这些都可以在用户指南中找到。

YCM 拥有健全的默认配置，但你可能依然想要看看可选的配置细节。基于谨慎考虑，一些有趣的配置默认为关闭状态，而你可能想要开启它们。

完整安装指导
~~~~~~~~~~~~~~~~

这里提供了让 YCM 在 Unix 和 Windows 系统上运行起来的必要步骤。

**Windows 用户注意：** 我们假设你在使用 ``cmd.exe`` 命令行，而且所需的可执行文件已经加入了 PATH 环境变量。不要直接复制这里的命令，用 ``%USERPROFILE%`` 替换其中的 ``~`` 并且使用正确的 Vim 根目录（默认在 ``vimfiles`` 而不是 ``.vim`` ）。

遇到任何问题，查看 *FAQ* 。

**谨记：** YCM 插件包含编译组件。如果你用 Vundle **更新** 了 YCM 而 ycm_core 库的 API 发生变化（很少发生）， YCM 会提醒你重新编译。那么你需要重新走一遍安装流程。

**请谨遵指导，逐字阅读。**

1. 
    **确定你的 Vim 版本 *至少是 7.4.1578* 并且支持 Python 2 或 Python 3 脚本**。

    进入 Vim ，输入 ``:version`` 。查看输出结果的前2-3行，里面应该包括 ``Vi IMproved X.Y`` ， X.Y 是 vim 的主要版本。如果你的版本大于 7.4 ，那么你已经准备就绪。如果你的版本是 7.4 ，找到下面的 ``Included patched: 1-Z`` ， Z 需要是大于或等于 1578 的数字。

    如果你的 Vim 版本不够新，你需要 `从源码编译Vim <vim-build_>`_ （不要担心，很简单）。

    搞定 Vim 7.4.1578+ 版本之后，在 Vim 中输入 ``:echo has('python') || has('python3')`` ，输出结果应该为 1 。如果是 0 ，那么就需要安装支持 Python 的 Vim 版本。

    对于 Windows ，还需要检查 Vim 的架构是 32 还是 64-bit 。这一点很重要，因为这个架构需要和 Python 以及 YCM 库架构匹配。我们推荐使用 64-bit 的 Vim 。

2.
    用 `Vundle <vundle_>`_ （或者 `Pathogen <Pathogen>`_ ，但用 Vundle 更好） 安装 YCM 。对于 Vundle ，需要添加 ``Plugin 'Valloric/YouCompleteMe'`` 到 `vimrc <vimrc_>`_ 。

    如果你不用 Vundle 来安装 YCM ，确保在checkout之后执行 ``git submodule update --init --recursive`` 来获取 YCM 的依赖。

3. 
    *这一步 仅仅 针对需要 C 系语言语义补全支持的情况，否则无关紧要。*

    **下载最新版本的libclang** 。 Clang 是一个用来编译 C 系语言的开源编译器，它提供了可以驱动 YCM 相关语义补全引擎的 ``libclang`` 库。 YCM 支持 libclang 的 7.0.0 或更高版本。

    除了 ``libclang`` ， YCM 还支持 **实验阶段的** 基于 clangd_ 的 completer。你可以从 `llvm.org releases <clang-download_>`_ 下载最新版本的 clangd_ 。按照第4步告诉 YCM 去那里找到 clangd 可执行文件。请注意， YCM 支持 7.0.0 或更高版本的 clangd_ 。

    你可以使用系统自带的 libclang 或 clangd ，但是是在 *确保版本是 7.0.0 或更高的前提下* ，否则就不要用。即使版本合适，我们也推荐尽可能使用 `llvm.org上的官方编译版本 <clang-download_>`_ 。确保下载匹配你操作系统的压缩包。

    我们 **强烈建议避免使用** 系统自带的 libclang 或 clangd ，为保证万无一失，使用上游提供的预编译 libclang 。

4. 
    **编译 YCM 所需的 ycm_core 库** 。这个库是 YCM 用于获取快速补全的 C++ 引擎。

    你需要安装 ``cmake`` ，以生成所需的 makefile 。 Linux 用户可以使用包管理器（Ubuntu用 ``sudo apt-get install cmake`` ），其他用户可以从 cmake 项目网站 `下载安装 <cmake-download_>`_ 。 Mac 用户也可以通过 `HomeBrew <brew_>`_ 执行 `brew install cmake` 来安装。

    在 Unix 系统上，你需要确保安装 Python 头文件。 Debian 系的 Linux 发行版使用 ``sudo apt-get install python-dev python3-dev`` 命令。在 Mac 上这些文件是现成的。

    在 Windows 系统上，你需要下载安装 `Python 2 或 Python 3 <python-win-download_>`_ 。根据你的 Vim 架构来选择对应版本。你还需要 Microsoft Visual C++ (MSVC) 来编译 YCM 。你可以通过安装 `Visual Stidio <visual-studio-download_>`_ 来获取它。 MSVC 14 （Visual Studio 2015） 和 15 （2017）是官方支持的。

    此处我们假设你用 Vundle 安装了 YCM ，意味着顶层 YCM 路径在 ``~/.vim/bundle/YouCompleteMe`` 。

    创建一个新目录用于存放编译文件。执行以下命令：

    .. code-block:: bash

        cd ~
        mkdir ycm_build
        cd ycm_build

    现在我们需要生成 makefile 。如果你 **不** 在意 C 系语言支持，也不打算使用 **实验阶段的** 基于 ``clangd`` 的 completer ，在 ``ycm_build`` 目录下执行：

    .. code-block:: bash

        cmake -G "<generator>" . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp
    
    其中 ``<generator>`` 在 Unix 系统上是 ``Unix Makefiles`` ，在 Windows 上则是以下之一：

    - ``Visual Studio 14 Win64``
    - ``Visual Studio 15 Win64``

    如果你的 Vim 架构是 32 位，就去掉 ``Win64`` 。

    如果想使用系统版本的 boost 库，你可以给 cmake 加上 ``-DUSE_SYSTEM_BOOST=ON`` 。在一些系统上，如果（cmake）捆绑的 boost 不能开箱即用，这个参数会比较有用。

    **注意：** 我们 **强烈建议不要使用** 系统版本的 boost ，减少不必要的风险。

    如果你 **在意** C 系语言的语义支持，并且想使用 libclang 作为驱动而非 **实验阶段** 的基于 ``clangd`` 的 completer， 那么你执行 ``cmake`` 的命令会变得更复杂一点。我们假设你按照第3步从 llvm.org 下载了 LLVM+Clang 的二进制包，而且把压缩文件解压到了 ``~/ycm_temp/llvm_root_dir`` （里面包含 ``bin`` ， ``lib`` ， ``include`` 等目录）。在 Windows 系统上，你可以使用 `7-zip <7-zip_>`_ 来解压 LLVM+Clang 的安装文件。

    **注意：** 这 *仅仅* 针对 *下载* 的 LLVM 二进制包生效，而非自己编译的 LLVM ！如果使用的是自己编译的 LLVM ，查看下文的 ``EXTERNAL_LIBCLANG_PATH`` 。

    在 ``ycm_build`` 路径下执行：

    .. code-block:: bash

        cmake -G "<generator>" -DPATH_TO_LLVM_ROOT=~/ycm_temp/llvm_root_dir . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp

    其中， ``<generator`` 的替换如前所述。

    配置文件生成之后，使用如下命令来编译库文件：

    .. code-block:: bash

        cmake --build . --target ycm_core --config Release
    
    其中， ``--config Release`` 是给 Windows 系统用的，在 Unix 系统上會被忽略。

    如果你想使用系统版本的 libclang ，在 cmake 命令中用 ``-DUSE_SYSTEM_LIBCLANG=ON`` 这个 flag 来 **替换** ``-DPATH_TO_LLVM_ROOT=...`` 。

    **注意：** 我们 **强烈建议不要使用** 系统版本的 libclang ，请选择上游发布的二进制文件，以减少不必要的风险。

    你也可以强行使用自定义的 libclang 库，传入 ``-DEXTERNAL_LIBCLANG_PATH=/path/to/libclang.so`` 即可（库文件在 Mac 上后缀为 ``.dylib`` ）。再次声明，这个 flag 是用来 **替换** 其他的 flag 。 **如果你使用的是从源码编译的 LLVM ，你就需要使用这个 flag** 。

    如果你编译时用到了 clang 支持，那么 ``cmake`` 命令也会为你把 ``libclang.[so|dylib|dll]`` 放到 ``YcmCompleteMe/third_party/ycmd`` 路径下（ YCM 工作时会用到它）。

    如果你 **在意** C 系语言支持，而且希望使用 **实验阶段的** 基于 clangd_ 的 completer ，那么你需要在 ``vimrc`` 中加上：

    .. code-block:: vim

        let g:ycm_use_clangd = "Always"
        let g:ycm_clangd_binary_path = "/path/to/clangd"


    用你在第三步的下载地址来替换 /path/to/clangd 。

5.
    *这一步是可选的。*

    为了更好的 Unicode 支持以及更好的正则表达式性能，编译 regex_ 模块。步骤类似于编译 ``ycm_core`` 库：

    .. code-block:: bash

        cd ~
        mkdir regex_build
        cd regex_build
        cmake -G "<generator>" . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party/cregex
        cmake --build . --target _regex --config Release


    其中 ``<generator>`` 同前文所述。

6.
    按照需要，配置其他语言支持：

    - C# 支持：安装 `非 Windows 平台的 Mono <mono-install_>`_ 。
      切换到 ``YouCompleteMe/third_party/ycmd/third_party/OmniSharpServer`` 目录，执行：

      .. code-block:: bash

          msbuild /property:Configuration=Release /property:Platform="Any CPU" /property:TargetFrameworkVersion=v4.5

      在 Windows 上，确保 `msbuild 在你的 PATH 环境变量里 <add-msbuild-to-path>`_ 。

    - Go 支持：安装 `Go <go-install_>`_ ，并将其添加到环境变量路径中。切换到 ``YouCompleteMe/third_party/ycmd/third_party/gocode`` 执行 ``go build`` 。

    - JavaScript 和 TypeScript 支持：安装 `Node.js and npm <npm-install_>`_ ，切换到 ``YouCompleteMe/third_party/ycmd`` 执行 ``npm install -g --prefix third_party/tsserver typescript`` 。

    - Rust 支持：安装 `Rust <rust-install_>`_ ，切换到 ``YouCompleteMe/third_party/ycmd/third_party/racerd`` 执行 ``cargo build --release`` 。
    - Java 支持：安装 `JDK8 (version 8 required) <jdk-install_>`_ 。下载 `binary release of eclipse.jdt.ls <jdtls-release_>`_ 然后解压到 ``YouCompleteMe/third_party/ycmd/third_party/eclipse.jdt.ls/target/repository`` 。
      
      注意：这个方法不推荐大部分用户使用，只支持愿意尽情折腾的高级用户和 YCM 开发者。
      请使用 ``install.py`` 来开启 java 支持。

搞定。查阅 `用户指南` 了解 YCM 的用法。不要忘记，如果你需要 C 系语言的补全引擎正常工作，则需要对 YCM 提供你的项目的 compilation flags 。这些都可以在用户指南中找到。

YCM 拥有健全的默认配置，但你可能依然想要看看可选的配置细节。基于谨慎考虑，一些有趣的配置默认为关闭状态，而你可能想要开启它们。
    

特性速览
--------



通用（所有语言）
~~~~~~~~~~~~~~~

- 极快的基于标识符的 completer ，包含 tags 文件和语法元素

C系语言（C，C++，Objective C，Objective C++，CUDA）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



    **翻译待续**

.. HERE

用户指南
--------

C系语言语义补全
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


completer 子命令全列表
------------------------


FAQ
------

联系方式
--------

许可
------

.. ref

.. _ycmd: https://github.com/Valloric/ycmd
.. _Clang: http://clang.llvm.org/
.. _vundle: https://github.com/VundleVim/Vundle.vim#about
.. _pathogen: https://github.com/tpope/vim-pathogen#pathogenvim
.. _clang-download: http://llvm.org/releases/download.html
.. _brew: http://brew.sh
.. _cmake-download: https://cmake.org/download/
.. _macvim: https://github.com/macvim-dev/macvim/releases
.. _vimrc: http://vimhelp.appspot.com/starting.txt.html#vimrc
.. _gpl: http://www.gnu.org/copyleft/gpl.html
.. _vim: http://www.vim.org/
.. _syntastic: https://github.com/scrooloose/syntastic
.. _lightline: https://github.com/itchyny/lightline.vim
.. _ycm_flags_example: https://github.com/Valloric/YouCompleteMe/blob/master/.ycm_extra_conf.py
.. _ycmd_flags_example: https://raw.githubusercontent.com/Valloric/ycmd/66030cd94299114ae316796f3cad181cac8a007c/.ycm_extra_conf.py
.. _compdb: http://clang.llvm.org/docs/JSONCompilationDatabase.html
.. _subsequence: https://en.wikipedia.org/wiki/Subsequence
.. _listtoggle: https://github.com/Valloric/ListToggle
.. _vim-build: https://github.com/Valloric/YouCompleteMe/wiki/Building-Vim-from-source
.. _tracker: https://github.com/Valloric/YouCompleteMe/issues?state=open
.. _issue18: https://github.com/Valloric/YouCompleteMe/issues/18
.. _delimitMate: https://github.com/Raimondi/delimitMate
.. _completer-api: https://github.com/Valloric/ycmd/blob/master/ycmd/completers/completer.py
.. _eclim: http://eclim.org/
.. _jedi: https://github.com/davidhalter/jedi
.. _ultisnips: https://github.com/SirVer/ultisnips/blob/master/doc/UltiSnips.txt
.. _exuberant-ctags: http://ctags.sourceforge.net/
.. _universal-ctags: https://github.com/universal-ctags/ctags
.. _ctags-format: http://ctags.sourceforge.net/FORMAT
.. _vundle-bug: https://github.com/VundleVim/Vundle.vim/issues/48
.. _ycm-users: https://groups.google.com/forum/?hl=en#!forum/ycm-users
.. _omnisharp: https://github.com/OmniSharp/omnisharp-server
.. _issue-303: https://github.com/Valloric/YouCompleteMe/issues/303
.. _issue-593: https://github.com/Valloric/YouCompleteMe/issues/593
.. _issue-669: https://github.com/Valloric/YouCompleteMe/issues/669
.. _status-mes: https://groups.google.com/forum/#!topic/vim_dev/WeBBjkXE8H8
.. _python-re: https://docs.python.org/2/library/re.html#regular-expression-syntax
.. _Bear: https://github.com/rizsotto/Bear
.. _ygen: https://github.com/rdnetto/YCM-Generator
.. _Gocode: https://github.com/nsf/gocode
.. _Godef: https://github.com/Manishearth/godef
.. _TSServer: https://github.com/Microsoft/TypeScript/tree/master/src/server
.. _jsconfig.json: https://code.visualstudio.com/docs/languages/jsconfig
.. _tsconfig.json: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html
.. _vim-win-download: https://bintray.com/micbou/generic/vim
.. _python-win-download: https://www.python.org/downloads/windows/
.. _visual-studio-download: https://www.visualstudio.com/downloads/
.. _7z-download: http://www.7-zip.org/download.html
.. _mono-install-osx: http://www.mono-project.com/docs/getting-started/install/mac/
.. _mono-install-linux: https://www.mono-project.com/download/stable/#download-lin
.. _mono-install: http://www.mono-project.com/docs/getting-started/install/
.. _go-install: https://golang.org/doc/install
.. _npm-install: https://docs.npmjs.com/getting-started/installing-node#1-install-nodejs--npm
.. _tern-instructions: https://github.com/Valloric/YouCompleteMe/wiki/JavaScript-Semantic-Completion-through-Tern
.. _Tern: http://ternjs.net
.. _racer: https://github.com/phildawes/racer
.. _rust-install: https://www.rust-lang.org/
.. _rust-src: https://www.rust-lang.org/downloads.html
.. _add-msbuild-to-path: http://stackoverflow.com/questions/6319274/how-do-i-run-msbuild-from-the-command-line-using-windows-sdk-7-1
.. _identify-R6034-cause: http://stackoverflow.com/questions/14552348/runtime-error-r6034-in-embedded-python-application/34696022
.. _ccoc: https://github.com/Valloric/YouCompleteMe/blob/master/CODE_OF_CONDUCT.md
.. _vim_win-python2.7.11-bug: https://github.com/vim/vim/issues/717
.. _vim_win-python2.7.11-bug_workaround: https://github.com/vim/vim-win32-installer/blob/a27bbdba9bb87fa0e44c8a00d33d46be936822dd/appveyor.bat#L86-L88
.. _gitter: https://gitter.im/Valloric/YouCompleteMe
.. _ninja-compdb: https://ninja-build.org/manual.html
.. _vim-nerdtree-tabs: https://github.com/jistr/vim-nerdtree-tabs
.. _++enc: http://vimdoc.sourceforge.net/htmldoc/editing.html#++enc
.. _rustup: https://www.rustup.rs/
.. _contributing-md: https://github.com/Valloric/YouCompleteMe/blob/master/CONTRIBUTING.md
.. _jdt.ls: https://github.com/eclipse/eclipse.jdt.ls
.. _jdk-install: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
.. _mvn-project: https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
.. _eclipse-project: https://help.eclipse.org/oxygen/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Freference%2Fmisc%2Fproject_description_file.html
.. _gradle-project: https://docs.gradle.org/current/userguide/tutorial_java_projects.html
.. _eclipse-dot-project: https://help.eclipse.org/oxygen/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Freference%2Fmisc%2Fproject_description_file.html
.. _eclipse-dot-classpath: https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.isv%2Freference%2Fapi%2Forg%2Feclipse%2Fjdt%2Fcore%2FIClasspathEntry.html
.. _ycmd-eclipse-project: https://github.com/Valloric/ycmd/tree/3602f38ef7a762fc765afd75e562aec9a134711e/ycmd/tests/java/testdata/simple_eclipse_project
.. _ycmd-mvn-pom-xml: https://github.com/Valloric/ycmd/blob/3602f38ef7a762fc765afd75e562aec9a134711e/ycmd/tests/java/testdata/simple_maven_project/pom.xml
.. _ycmd-gradle-project: https://github.com/Valloric/ycmd/tree/3602f38ef7a762fc765afd75e562aec9a134711e/ycmd/tests/java/testdata/simple_gradle_project
.. _jdtls-release: http://download.eclipse.org/jdtls/milestones
.. _diacritic: https://www.unicode.org/glossary/#diacritic
.. _regex: https://pypi.org/project/regex/
.. _clangd: https://clang.llvm.org/extra/clangd.html
.. _fixedcdb: https://clang.llvm.org/docs/JSONCompilationDatabase.html#alternatives
.. _clangd-indexing: https://clang.llvm.org/extra/clangd.html#project-wide-indexing


.. |Gitter room| image:: https://img.shields.io/gitter/room/Valloric/YouCompleteMe.svg
.. _Gitter room: https://gitter.im/Valloric/YouCompleteMe
.. |Linux build status| image:: https://img.shields.io/travis/Valloric/YouCompleteMe/master.svg?label=Linux
.. _Linux build status: https://travis-ci.org/Valloric/YouCompleteMe
.. |macOS build status| image:: https://img.shields.io/circleci/project/github/Valloric/YouCompleteMe/master.svg?label=macOS
.. _macOS build status: https://circleci.com/gh/Valloric/YouCompleteMe
.. |Windows build status| image:: https://img.shields.io/appveyor/ci/Valloric/YouCompleteMe/master.svg?label=Windows
.. _Windows build status: https://ci.appveyor.com/project/Valloric/YouCompleteMe
.. |Coverage status| image:: https://img.shields.io/codecov/c/github/Valloric/YouCompleteMe/master.svg
.. _Coverage status: https://codecov.io/gh/Valloric/YouCompleteMe

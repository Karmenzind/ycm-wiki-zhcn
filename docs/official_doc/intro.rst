README 文档
==========

**Vim代码补全引擎YouCompleteMe**

|Gitter room|_ |Linux build status|_ |macOS build status|_ |Windows build status|_ |Coverage status|_


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


|YouCompleteMe GIF demo|

.. |YouCompleteMe GIF demo| image:: http://i.imgur.com/0OP4ood.gif

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


编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    ./install.py


以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

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


编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: bash

    cd ~/.vim/bundle/YouCompleteMe
    python3 install.py

以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

如果要一次性编译所有特性，则加上 ``--all`` 参数。如果要安装所有语言特性，确保在 ``PATH`` 路径下安装了 ``xbuild``, ``go``, ``tsserver``, ``node``,
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


编译 **不包含** 对 C 系语言语义支持的 YCM ：

.. code-block:: cmd

    cd %USERPROFILE%/vimfiles/bundle/YouCompleteMe
    python install.py

以下为可用的附加语言支持选项：

- C# 支持：用 Homebrew_ 安装 Mono 或下载[Mono Mac package][mono-install-osx] 并在执行 ``install.py`` 时添加 ``--cs-completer`` 。
- Go 支持：安装 `Go <go-install_>`_ ，并在执行 ``install.py`` 时添加 ``--go-completer`` 。
- JavaScript 和 TypeScript 支持：安装 `Node.js 和 npm <npm-install_>`_ ，并在执行 ``install.py`` 时添加 ``--ts-completer`` 。
- Rust 支持：安装 `JDK8 (必须是8) <jdk-install_>`_ ，并在执行 ``install.py`` 时添加 ``--java-completer`` 。

如果要一次性编译所有特性，则加上 ``--all`` 参数。如果要安装所有语言特性，确保在 ``PATH`` 路径下安装了 ``msbuild``, ``go``, ``tsserver``, ``node``,
``npm`` 和 ``cargo`` 然后直接运行：

.. code-block:: cmd

    cd %USERPROFILE%/vimfiles/bundle/YouCompleteMe
    python install.py --all

你可以用 ``--msvc`` 参数来指定 Microsoft Visual C++ (MSVC) 。 YCM 官方支持 MSVC 14 (Visual Studio 2015) 和 15 (2017) 。

搞定。查阅 `用户指南` 了解 YCM 的用法。不要忘记，如果你需要 C 系语言的补全引擎正常工作，则需要对 YCM 提供你的项目的 compilation flags 。这些都可以在用户指南中找到。

YCM 拥有健全的默认配置，但你可能依然想要看看可选的配置细节。基于谨慎考虑，一些有趣的配置默认为关闭状态，而你可能想要开启它们。


FreeBSD/OpenBSD
~~~~~~~~~~~~~~~~

完整安装指导
~~~~~~~~~~~~~~~~

.. HERE



特性速览
--------

通用（所有语言）
~~~~~~~~~~~~~~~


用户指南
--------


completer 子命令全列表
------------------------

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

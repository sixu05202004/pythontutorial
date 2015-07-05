.. _tut-using:

****************************
使用 Python 解释器
****************************


.. _tut-invoking:

调用 Python 解释器
========================

Python 解释器通常被安装在目标机器的 :file:`/usr/local/bin/python` 目录下。将 :file:`/usr/local/bin` 目录包含进 Unix shell 的搜索路径里，以确保可以通过输入::

   python

命令来启动它。由于 Python 解释器的安装路径是可选的，这也可能是其他路径，你可以联系安装 Python 的用户或系统管理员确认。(例如，:file:`/usr/local/python` 就是一个常见的选择)

在 Windows 机器上，Python 通常安装在 :file:`C:\\Python27` 位置，当然你可以在运行安装向导时修改此值。要想把此目录添加到你的 PATH 环境变量中，你可以在 DOS 窗口中输入以下命令::

   set path=%path%;C:\python27

通常你可以在主窗口输入一个文件结束符( Unix 系统是 :kbd:`Control-D`，Windows 系统是 :kbd:`Control-Z` )让解释器以 0 状态码退出。如果它不起作用，你可以通过输入 ``quit()``  命令退出解释器。

Python 解释器具有简单的行编辑功能。在 Unix 系统上，任何 Python 解释器都可能已经添加了 GNU readline 库支持，这样就具备了精巧的交互编辑和历史记录等功能。在 Python 主窗口中输入 Control-P 可能是检查是否支持命令行编辑的最简单的方法。如果发出嘟嘟声(计算机扬声器)，则说明你可以使用命令行编辑功能；更多快捷键的介绍请参考 :ref:`tut-interacting`。 如果没有任何声音，或者显示 ``^P`` 字符，则说明命令行编辑功能不可用；你只能通过退格键从当前行删除已键入的字符并重新输入。

Python 解释器有些操作类似 Unix shell：当使用终端设备(tty)作为标准输入调用时，它交互地解释并执行命令；当使用文件名参数或以文件作为标准输入调用时，它读取文件并将文件作为 *脚本* 执行。

第二种启动 Python 解释器的方法是 ``python -c command [arg] ...``，这种方法可以在 *命令行* 执行 Python 语句，类似于 shell 中的 :option:`-c` 选项。由于 Python 语句通常会包含空格或其他特殊 shell 字符，一般建议将 *命令* 用单引号包裹起来。

有一些 Python 模块也可以当作脚本使用。你可以使用 ``python -m module [arg] ...`` 命令来调用它们，这类似在命令行中键入完整的路径名执行 *模块* 源文件一样。

使用脚本文件时，经常会运行脚本然后进入交互模式。这也可以通过在脚本之前加上 :option:`-i` 参数来实现。

所有的命令行参数详细描述在 `命令行和环境 <https://docs.python.org/3.6/using/cmdline.html#using-on-general>`_ 。

.. _tut-argpassing:

参数传递
----------------

调用解释器时，脚本名和附加参数传入一个名为 ``sys.argv`` 的字符串列表。你能够通过执行 ``import sys`` 来获取这个列表，列表的长度大于等于1；没有给定脚本和参数时，它至少也有一个元素：``sys.argv[0]`` 此时为空字符串。

脚本名指定为 ``'-'`` (表示标准输入)时，``sys.argv[0]`` 被设定为 ``'-'`` ，使用 :option:`-c` *指令* 时，``sys.argv[0]`` 被设定为 ``'-c'``。 

使用 :option:`-m` *模块* 参数时，``sys.argv[0]`` 被设定为指定模块的全名。:option:`-c` *指令* 或者 :option:`-m` *模块* 之后的参数不会被 Python 解释器的选项处理机制所截获，而是留在 ``sys.argv`` 中，供脚本命令操作。


.. _tut-interactive:

交互模式
----------------

从 tty 读取命令时，我们称解释器工作于 *交互模式*。这种模式下它根据 *主提示符* 来执行，主提示符通常标识为三个大于号 (``>>>``)；继续的部分被称为 *从属提示符*，由三个点标识 (``...``)。在第一行之前，解释器打印欢迎信息、版本号和授权提示::

   python
   Python 2.7 (#1, Feb 28 2010, 00:02:06)
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

.. XXX update for new releases

输入多行结构时需要从属提示符了，例如，下面这个 `if <https://docs.python.org/2.7/reference/compound_stmts.html#if>`_ 语句::

   >>> the_world_is_flat = 1
   >>> if the_world_is_flat:
   ...     print "Be careful not to fall off!"
   ...
   Be careful not to fall off!

关于交互魔术更多的信息，请见 :ref:`tut-interacting`。


.. _tut-interp:

解释器及其环境
===================================


.. _tut-source-encoding:

源程序编码
--------------------

在Python源文件中可以使用非 ASCII 编码。最好的方法是在 ``#!`` 行的后面再增加一行特殊的注释来定义源文件的编码::

   # -*- coding: encoding -*-


通过此声明，源文件中所有的东西都会被当做用 *encoding* 指代的 UTF-8 编码对待。在 Python 库参考手册 `codecs <https://docs.python.org/2.7/library/codecs.html#module-codecs>`_ 一节中你可以找到一张可用的编码列表。

例如，若要写入包含欧元货币符号的 Unicode 字面量，可以使用 ISO-8859-15 编码，其欧元符号的值为 164 。此脚本中，以 ISO-8859-15 编码，保存时将打印的值 8364 (Unicode 代码点相应的欧元符号），然后退出::

   # -*- coding: iso-8859-15 -*-

   currency = u"€"
   print ord(currency)

如果你的编辑器支持保存为带有 ``UTF-8`` *字节顺序标记* (也叫做 BOM ) 的 UTF-8 格式的文件，你可以使用这种功能而不用编码声明。IDLE 如果设置了 ``Options/General/Default Source Encoding/UTF-8`` 也支持此功能。注意，这种标记方法在旧的 Python 版本中（2.2 及更早）是不能识别的，同样也不能被能够处理 ``#!`` （只在 Unix 系统上使用）行的操作系统识别。

通过使用 UTF-8 编码（无论是BOM方式或者是编码声明方式），世界上大多数语言的字符可以在字符串字面量和注释中同时使用。在标识符中使用非 ASCII 字符是不支持的。若要正确显示所有这些字符，您的编辑器必须认识该文件是 UTF-8 编码，并且它必须使用支持文件中所有字符的字体。
.. _tut-using:

****************************
使用Python解释器
****************************


.. _tut-invoking:

调用解释器
========================

通常 Python 的解释器被安装在目标机器的 :file:`/usr/local/bin/python`
目录下；把 :file:`/usr/local/bin` 
目录放进你的 Unix Shell 的搜索路径里，确保它可以通过输入::

   python

来启动。因为安装路径是可选的，所以也有可能安装在其它位置，你可以与安装 Python 的用户或系统管理员联系。（例如， :file:`/usr/local/python` 就是一个很常见的选择。）

在 Windows机器上，Python 通常安装在 :file:`C:\\Python27` 当然，我们在运行安装程序的时候可以改变它。需要把这个目录加入到我们的 Path 中的话，可以像下面这样在 DOS 窗中输入命令行:

   set path=%path%;C:\python27

输入一个文件结束符 (在Unix上 :kbd:`Control-D`, 在
Windows上 :kbd:`Control-Z`)解释器会以 0 值退出。如果这没有起作用，你可以输入以下命令退出: 
``quit()``。

解释器的行编辑功能并不很复杂。装在 Unix 上的解释器可能会有 GNU readline 库支持，这样就可以额外得到精巧的交互编辑和历史记录功能。可能检查命令行编辑器支持能力。最方便的方式是在主提示符下输入 Control-P。 如果有嘟嘟声（计算机扬声器），说明你可以使用命令行编辑功能； 从附录中 :ref:`tut-interacting` 可以查到快捷键的介绍。如果什么声音也没有，或者显示 ``^P`` ,
说明命令行编辑功能不可用，你只有用退格键删掉输入的命令了。 

解释器的操作有些像 Unix Shell：使用终端设备做为标准输入来调用它时，解释器交互的解读和执行命令，通过文件名参数或以文件做为标准输入设备时，它从文件中解读并执行 *脚本*。

启动解释器的第二个方法是 ``python -c command [arg] ...``,
这种方法可以在 *命令行* 中直接执行语句， 等同于Shell的
:option:`-c` 选项。 因为Python语句通常会包括空格之类的特殊字符，所以最好把整个 *命令* 用单引号包起来。

有些 Python 模块也可以当作脚本使用。它们可以用
``python -m module [arg] ...`` 调用， 这样就会像你在命令行中给出其完整名字一样运行 *模块* 源文件。

使用脚本文件时，经常会运行脚本然后进入交互模式。这也可以通过在脚本之前加上 :option:`-i` 参数来实现


.. _tut-argpassing:

参数传递
----------------

调用解释器时，脚本名和附加参数传入一个名为 ``sys.argv`` 的字符串列表。你能够获取这个列表通过执行 ``import
sys``，列表的长度大于等于1；没有给定脚本和参数时，它至少也有一个元素： ``sys.argv[0]`` 此时为空字符串。脚本名指定为 ``'-'`` （表示标准输入）时， ``sys.argv[0]`` 被设定为 ``'-'`` ，使用 :option:`-c` *指令* 时， ``sys.argv[0]`` 被设定为 ``'-c'`` 。 使用 :option:`-m` *模块* 参数时， ``sys.agv[0]`` 被设定为指定模块的全名。:option:`-c` *指令* 或者 :option:`-m` *模块* 之后的参数不会被 Python 解释器的选项处理机制所截获，而是留在 ``sys.argv`` 中，供脚本命令操作。


.. _tut-interactive:

交互模式
----------------

从 tty 读取命令时，我们称解释器工作于 *交互模式* 。这种模式下它根据 主提示符 来执行，主提示符通常标识为三个大于号 (``>>>``)；继续的部分被称为 *从属提示符* ，由三个点标识 (``...``) 。在第一行之前，解释器打印欢迎信息、版本号和授权提示::

   python
   Python 2.7 (#1, Feb 28 2010, 00:02:06)
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

输入多行结构时需要从属提示符了，例如，下面这个 :keyword:`if` 语句::

   >>> the_world_is_flat = 1
   >>> if the_world_is_flat:
   ...     print "Be careful not to fall off!"
   ...
   Be careful not to fall off!


.. _tut-interp:

解释器及其环境
===================================


.. _tut-error:

错误处理
--------------

有错误发生时，解释器打印一个错误信息和栈跟踪器。交互模式下，它返回主提示符，如果从文件输入执行，它在打印栈跟踪器后以非零状态退出。（异常可以由 :keyword:`try` 语句中的 :keyword:`except` 子句来控制，这样就不会出现上文中的错误信息）有一些非常致命的错误会导致非零状态下退出，这由通常由内部矛盾和内存溢出造成。所有的错误信息都写入标准错误流；命令中执行的普通输出写入标准输出。

在主提示符或附属提示符输入中断符（通常是Control-C 或者 DEL）就会取消当前输入，回到主命令行。 [#]_ 执行命令时输入一个中断符会抛出一个 :exc:`KeyboardInterrupt` 异常，它可以被 :keyword:`try` 句截获。


.. _tut-scripts:

执行 Python 脚本
-------------------------

BSD类的 Unix系统中，Python 脚本可以像 Shell 脚本那样直接执行。只要在脚本文件开头写一行命令，指定文件和模式 ::

   #! /usr/bin/env python

(要确认 Python 解释器在用户的 :envvar:`PATH` 中) ``#!``  必须是文件的前两个字符，在某些平台上，第一行必须以 Unix 风格的行结束符（ ``'n'`` ）结束，不能用 Windows （ ``'rn'`` ） 的结束符。注意， ``'#'`` 是Python中是行注释的起始符。 
脚本可以通过 :program:`chmod` 命令指定执行模式和权限 ::

   $ chmod +x myscript.py

Windows 系统上没有“执行模式”。 Python 安装程序自动将 ``.py`` 文件关联到 ``python.exe`` ，所以在 Python 文件图标上双击，它就会作为脚本执行。同样 ``.pyw``  也作了这样的关联，通常它执行时不会显示控制台窗口。


.. _tut-source-encoding:

源程序编码
--------------------

Python 的源文件可以通过编码使用 ASCII 以外的字符集。最好的做法是在 ``#!``  行后面用一个特殊的注释行来定义字符集 ::

   # -*- coding: encoding -*-


根据这个声明，Python 会尝试将文件中的字符编码转为 *encoding*  编码。并且，它尽可能的将指定的编码直接写成 Unicode 文本。在 Python 库参考手册 中 :mod:`codecs`  部份可以找到可用的编码列表（推荐使用 utf-8 处理中文－－译者注）。

例如，可以用 ISO-8859-15 编码可以用来编写包含欧元符号的 Unicode 文本，其编码值为 164。这个脚本会输出 8364 （欧元符号的 Unicode 对应编码） 然后退出::

   # -*- coding: iso-8859-15 -*-

   currency = u"€"
   print ord(currency)

如果你的文件编辑器可以将文件保存为 ``UTF-8 格式``，并且可以保存 UTF-8 *字节序标记* （即 BOM - Byte Order Mark），你可以用这个来代替编码声明。IDLE可以通过设定 ``Options/General/Default Source Encoding/UTF-8`` 来支持它。需要注意的是旧版 Python 不支持这个标记（Python 2.2或更早的版本），也同样不去理解由操作系统调用脚本使用的 ``#!`` 行（仅限于 Unix系统）。 

使用 UTF-8 内码（无论是用标记还是编码声明），我们可以在字符串和注释中使用世界上的大部分语言。标识符中不能使用非 ASCII 字符集。为了正确显示所有的字符，你一定要在编辑器中将文件保存为 UTF-8 格式，而且要使用支持文件中所有字符的字体。


.. _tut-startup:

交互式环境的启动文件
----------------------------

使用 Python 解释器的时候，我们可能需要在每次解释器启动时执行一些命令。你可以在一个文件中包含你想要执行的命令，设定一个名为 :envvar:`PYTHONSTARTUP` 的环境变量来指定这个文件。这类似于 Unix shell的 :file:`.profile` 文件。 

这个文件在交互会话期是只读的，当 Python 从脚本中解读文件或以终端 :file:`/dev/tty` 做为外部命令源时则不会如此（尽管它们的行为很像是处在交互会话期。）它与解释器执行的命令处在同一个命名空间，所以由它定义或引用的一切可以在解释器中不受限制的使用。你也可以在这个文件中改变 ``sys.ps1`` 和 ``sys.ps2``  指令。 

如果你想要在当前目录中执行附加的启动文件，可以在全局启动文件中加入类似以下的代码： ``if os.path.isfile('.pythonrc.py'): execfile('.pythonrc.py')``  。如果你想要在某个脚本中使用启动文件，必须要在脚本中写入这样的语句::

   import os
   filename = os.environ.get('PYTHONSTARTUP')
   if filename and os.path.isfile(filename):
       execfile(filename)


.. _tut-customize:

本地化模块
-------------------------

Python提供了两个钩子（方法）来本地化: :mod:`sitecustomize` 和
:mod:`usercustomize`.  为了见识它们, 你首先需要找到你的site-packages的目录.  启动python执行下面的代码::

   >>> import site
   >>> site.getusersitepackages()
   '/home/user/.local/lib/python3.2/site-packages'

现在你可以在site-packages的目录下创建 :file:`usercustomize.py` 文件，内容就悉听尊便了。
这个文件将会影响python的每次调用，除非启动的时候加入 :option:`-s` 选项禁止自动导入。

:mod:`sitecustomize` 的工作方式一样, 但是是由电脑的管理账户创建以及在 :mod:`usercustomize` 之前导入。 具体可以参见 :mod:`site` 。

.. rubric:: Footnotes

.. [#] A problem with the GNU Readline package may prevent this.

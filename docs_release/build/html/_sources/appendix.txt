.. _tut-appendix:

********
附录
********


.. _tut-interac:

交互模式
================

.. _tut-error:

错误处理
--------------

有错误发生时，解释器会打印一个错误信息和栈跟踪器。在交互模式下，它返回主提示符，如果从文件输入执行，它在打印栈跟踪器后以非零状态退出。(异常可以由 `try <https://docs.python.org/2.7/reference/compound_stmts.html#try>`_ 语句中的 `except <https://docs.python.org/2.7/reference/compound_stmts.html#except>`_ 子句来控制，这样就不会出现上文中的错误信息) 有一些非常致命的错误会导致非零状态下退出，这通常由内部矛盾和内存溢出造成。所有的错误信息都写入标准错误流；命令中执行的普通输出写入标准输出。

在主提示符或从属提示符中输入中断符 (通常是 Control-C 或者 DEL) 就会取消当前输入，回到主命令行。[#]_ 执行命令时输入一个中断符会抛出一个 `KeyboardInterrupt <https://docs.python.org/2.7/library/exceptions.html#exceptions.KeyboardInterrupt>`_ 异常，它可以被 `try <https://docs.python.org/2.7/reference/compound_stmts.html#try>`_ 语句截获。


.. _tut-scripts:

执行 Python 脚本
-------------------------

BSD 类的 Unix 系统中，Python 脚本可以像 Shell 脚本那样直接执行。只要在脚本文件开头写一行命令，指定文件和模式::

   #! /usr/bin/env python

(首先要确认 Python 解释器在用户的 :envvar:`PATH` 中) ``#!``  必须是文件的前两个字符，在某些平台上，第一行必须以 Unix 风格的行结束符 (``'\n'``)结束，不能用 Windows (``'\r\n'``) 的结束符。注意，``'#'`` 是 Python 中是行注释的起始符。 

脚本可以通过 :program:`chmod` 命令指定执行模式和权限。

.. code-block:: bash

   $ chmod +x myscript.py

Windows 系统上没有“执行模式”。Python 安装程序自动将 ``.py`` 文件关联到 ``python.exe`` ，所以在 Python 文件图标上双击，它就会作为脚本执行。同样 ``.pyw``  也做了这样的关联，通常它执行时不会显示控制台窗口。


.. _tut-startup:

交互执行文件
----------------------------

使用 Python 解释器的时候，我们可能需要在每次解释器启动时执行一些命令。你可以在一个文件中包含你想要执行的命令，设定一个名为 `PYTHONSTARTUP <https://docs.python.org/2.7/using/cmdline.html#envvar-PYTHONSTARTUP>`_ 的环境变量来指定这个文件。这类似于 Unix shell 的 :file:`.profile` 文件。 

这个文件在交互会话期是只读的，当 Python 从脚本中解读文件或以终端 :file:`/dev/tty` 做为外部命令源时则不会如此 (尽管它们的行为很像是处在交互会话期) 它与解释器执行的命令处在同一个命名空间，所以由它定义或引用的一切可以在解释器中不受限制地使用。你也可以在这个文件中改变 ``sys.ps1`` 和 ``sys.ps2``  指令。 

如果你想要在当前目录中执行附加的启动文件，可以在全局启动文件中加入类似以下的代码：``if os.path.isfile('.pythonrc.py'): execfile('.pythonrc.py')``。如果你想要在某个脚本中使用启动文件，必须要在脚本中写入这样的语句::

   import os
   filename = os.environ.get('PYTHONSTARTUP')
   if filename and os.path.isfile(filename):
       with open(filename) as fobj:
          startup_file = fobj.read()
       exec(startup_file)


.. _tut-customize:

定制模块
-------------------------


Python 提供了两个钩子 (方法) 来本地化: :mod:`sitecustomize` 和
:mod:`usercustomize`。为了见识它们，你首先需要找到你的 site-packages 的目录。启动 python 执行下面的代码::

   >>> import site
   >>> site.getusersitepackages()
   '/home/user/.local/lib/python2.7/site-packages'

现在你可以在 site-packages 的目录下创建 :file:`usercustomize.py` 文件，内容就悉听尊便了。这个文件将会影响 python 的每次调用，除非启动的时候加入 `-s <https://docs.python.org/2.7/using/cmdline.html#cmdoption-s>`_ 选项禁止自动导入。

:mod:`sitecustomize` 的工作方式一样，但是是由电脑的管理账户创建以及在 :mod:`usercustomize` 之前导入。具体可以参见 `site <https://docs.python.org/2.7/library/site.html#module-site>`_ 。


.. rubric:: Footnotes

.. [#] GNU Readline包的一个问题可能禁止此功能。

.. _tut-structures:

***************
数据结构
***************

本章详细讨论了你已经学过的一些知识，同样也添加了一些新内容。

.. _tut-morelists:

关于列表更多的内容
===================

Python 的列表数据类型包含更多的方法。这里是所有的列表对象方法：


.. method:: list.append(x)
   :noindex:

   把一个元素添加到链表的结尾，相当于 ``a[len(a):] = [x]``。


.. method:: list.extend(L)
   :noindex:

   将一个给定列表中的所有元素都添加到另一个列表中，相当于 ``a[len(a):] = L``。


.. method:: list.insert(i, x)
   :noindex:

   在指定位置插入一个元素。第一个参数是准备插入到其前面的那个元素的索引，例如 ``a.insert(0, x)`` 会插入到整个链表之前，而 ``a.insert(len(a), x)`` 相当于 ``a.append(x)``。


.. method:: list.remove(x)
   :noindex:

   删除链表中值为 *x* 的第一个元素。如果没有这样的元素，就会返回一个错误。


.. method:: list.pop([i])
   :noindex:

   从链表的指定位置删除元素，并将其返回。如果没有指定索引，``a.pop()`` 返回最后一个元素。元素随即从链表中被删除。(方法中 *i* 两边的方括号表示这个参数是可选的，而不是要求你输入一对方括号，你会经常在 Python 库参考手册中遇到这样的标记。)


.. method:: list.index(x)
   :noindex:

   返回链表中第一个值为 *x* 的元素的索引。如果没有匹配的元素就会返回一个错误。


.. method:: list.count(x)
   :noindex:

   返回 *x* 在链表中出现的次数。


.. method:: list.sort(cmp=None, key=None, reverse=False)
   :noindex:

   对链表中的元素就地进行排序（参数可以用来自定义排序方法，参考 `sorted() <https://docs.python.org/2.7/library/functions.html#sorted>`_ 的更详细的解释）。


.. method:: list.reverse()
   :noindex:

   就地倒排链表中的元素。

下面这个示例演示了链表的大部分方法::

   >>> a = [66.25, 333, 333, 1, 1234.5]
   >>> print a.count(333), a.count(66.25), a.count('x')
   2 1 0
   >>> a.insert(2, -1)
   >>> a.append(333)
   >>> a
   [66.25, 333, -1, 333, 1, 1234.5, 333]
   >>> a.index(333)
   1
   >>> a.remove(333)
   >>> a
   [66.25, -1, 333, 1, 1234.5, 333]
   >>> a.reverse()
   >>> a
   [333, 1234.5, 1, 333, -1, 66.25]
   >>> a.sort()
   >>> a
   [-1, 1, 66.25, 333, 333, 1234.5]
   >>> a.pop()
   1234.5
   >>> a
   [-1, 1, 66.25, 333, 333]

也许大家会发现像 ``insert``， ``remove`` 或者 ``sort`` 这些修改列表的方法没有打印返回值 --它们返回 ``None``。[1]_  在 python 中对所有可变的数据类型这是统一的设计原则。


.. _tut-lists-as-stacks:

把链表当作堆栈使用
---------------------

.. sectionauthor:: Ka-Ping Yee <ping@lfw.org>


链表方法使得链表可以很方便的做为一个堆栈来使用，堆栈作为特定的数据结构，最先进入的元素最后一个被释放(后进先出)。用 :meth:`append` 方法可以把一个元素添加到堆栈顶。用不指定索引的 :meth:`pop` 方法可以把一个元素从堆栈顶释放出来。例如::

   >>> stack = [3, 4, 5]
   >>> stack.append(6)
   >>> stack.append(7)
   >>> stack
   [3, 4, 5, 6, 7]
   >>> stack.pop()
   7
   >>> stack
   [3, 4, 5, 6]
   >>> stack.pop()
   6
   >>> stack.pop()
   5
   >>> stack
   [3, 4]


.. _tut-lists-as-queues:

把链表当作队列使用
---------------------

.. sectionauthor:: Ka-Ping Yee <ping@lfw.org>

你也可以把链表当做队列使用，队列作为特定的数据结构，最先进入的元素最先释放(先进先出)。不过，列表这样用效率不高。相对来说从列表末尾添加和弹出很快；在头部插入和弹出很慢(因为为了一个元素，要移动整个列表中的所有元素)。 

要实现队列，使用 `collections.deque <https://docs.python.org/2.7/library/collections.html#collections.deque>`_，它为在首尾两端快速插入和删除而设计。例如::

   >>> from collections import deque
   >>> queue = deque(["Eric", "John", "Michael"])
   >>> queue.append("Terry")           # Terry arrives
   >>> queue.append("Graham")          # Graham arrives
   >>> queue.popleft()                 # The first to arrive now leaves
   'Eric'
   >>> queue.popleft()                 # The second to arrive now leaves
   'John'
   >>> queue                           # Remaining queue in order of arrival
   deque(['Michael', 'Terry', 'Graham'])


.. _tut-functional:

函数式编程工具
----------------------------

对于链表来讲，有三个内置函数非常有用: `filter() <https://docs.python.org/2.7/library/functions.html#filter>`_，`map() <https://docs.python.org/2.7/library/functions.html#map>`_ 以及 `reduce() <https://docs.python.org/2.7/library/functions.html#reduce>`_。

``filter(function, sequence)`` 返回一个 sequence(序列)，包括了给定序列中所有调用 ``function(item)`` 后返回值为 true 的元素(如果可能的话，会返回相同的类型)。如果该 *序列* (sequence)是一个 `str <https://docs.python.org/2.7/library/functions.html#str>`_, `unicode <https://docs.python.org/2.7/library/functions.html#unicode>`_ 或者 `tuple <https://docs.python.org/2.7/library/functions.html#tuple>`_，返回值必定是同一类型，否则，它总是 `list <https://docs.python.org/2.7/library/functions.html#list>`_。例如，以下程序可以计算一个被 3 或者 5 整除的序列::

   >>> def f(x): return x % 3 == 0 or x % 5 == 0
   ...
   >>> filter(f, range(2, 25))
   [3, 5, 6, 9, 10, 12, 15, 18, 20, 21, 24]

``map(function, sequence)`` 为每一个元素依次调用 ``function(item)`` 并将返回值组成一个链表返回。例如，以下程序计算立方::

   >>> def cube(x): return x*x*x
   ...
   >>> map(cube, range(1, 11))
   [1, 8, 27, 64, 125, 216, 343, 512, 729, 1000]

可以传入多个序列，函数也必须要有对应数量的参数，执行时会依次用各序列上对应的元素来调用函数(如果某些序列比其它的短，就用 ``None`` 来代替)。如果把 ``None`` 做为一个函数传入，则直接返回参数做为替代。例如::

   >>> seq = range(8)
   >>> def add(x, y): return x+y
   ...
   >>> map(add, seq, seq)
   [0, 2, 4, 6, 8, 10, 12, 14]

``reduce(function, sequence)`` 返回一个单值，它是这样构造的：首先以序列的前两个元素调用函数 *function*，再以返回值和第三个参数调用，依次执行下去。例如，以下程序计算 1 到 10 的整数之和::

   >>> def add(x,y): return x+y
   ...
   >>> reduce(add, range(1, 11))
   55

如果序列中只有一个元素，就返回它，如果序列是空的，就抛出一个异常。

可以传入第三个参数作为初始值。如果序列是空的，就返回初始值，否则函数会先接收初始值和序列的第一个元素，然后是返回值和下一个元素，依此类推。例如::

   >>> def sum(seq):
   ...     def add(x,y): return x+y
   ...     return reduce(add, seq, 0)
   ...
   >>> sum(range(1, 11))
   55
   >>> sum([])
   0

不要像示例中这样定义 `sum() <https://docs.python.org/2.7/library/functions.html#sum>`_：因为合计数值是一个通用的需求，早已有内置的 ``sum(sequence)`` 函数，非常好用。


.. _tut-listcomps:

列表推导式
-------------------

列表推导式为从序列中创建列表提供了一个简单的方法。普通的应用程序通过将一些操作应用于序列的每个成员并通过返回的元素创建列表，或者通过满足特定条件的元素创建子序列。

例如，假设我们创建一个 squares 列表，可以像下面方式::

   >>> squares = []
   >>> for x in range(10):
   ...     squares.append(x**2)
   ...
   >>> squares
   [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

我们同样能够达到目的采用下面的方式::

   squares = [x**2 for x in range(10)]

这也相当于 ``squares = map(lambda x: x**2, range(10))``，但是上面的方式显得简洁以及具有可读性。

列表推导式由包含一个表达式的括号组成，表达式后面跟随一个 `for <https://docs.python.org/2.7/reference/compound_stmts.html#for>`_ 子句，之后可以有零或多个 `for <https://docs.python.org/2.7/reference/compound_stmts.html#for>`_ 或 `if <https://docs.python.org/2.7/reference/compound_stmts.html#if>`_ 子句。结果是一个列表，由表达式依据其后面的 `for <https://docs.python.org/2.7/reference/compound_stmts.html#for>`_ 和 `if <https://docs.python.org/2.7/reference/compound_stmts.html#if>`_ 子句上下文计算而来的结果构成。

例如，如下的列表推导式结合两个列表的元素，如果元素之间不相等的话::

   >>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
   [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

等同于::

   >>> combs = []
   >>> for x in [1,2,3]:
   ...     for y in [3,1,4]:
   ...         if x != y:
   ...             combs.append((x, y))
   ...
   >>> combs
   [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

值得注意的是在上面两个方法中的 `for <https://docs.python.org/2.7/reference/compound_stmts.html#for>`_ 和 `if <https://docs.python.org/2.7/reference/compound_stmts.html#if>`_ 语句的顺序。

如果想要得到一个元组 (例如，上面例子中的 ``(x, y)``)，必须要加上括号::

   >>> vec = [-4, -2, 0, 2, 4]
   >>> # create a new list with the values doubled
   >>> [x*2 for x in vec]
   [-8, -4, 0, 4, 8]
   >>> # filter the list to exclude negative numbers
   >>> [x for x in vec if x >= 0]
   [0, 2, 4]
   >>> # apply a function to all the elements
   >>> [abs(x) for x in vec]
   [4, 2, 0, 2, 4]
   >>> # call a method on each element
   >>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
   >>> [weapon.strip() for weapon in freshfruit]
   ['banana', 'loganberry', 'passion fruit']
   >>> # create a list of 2-tuples like (number, square)
   >>> [(x, x**2) for x in range(6)]
   [(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
   >>> # the tuple must be parenthesized, otherwise an error is raised
   >>> [x, x**2 for x in range(6)]
     File "<stdin>", line 1, in ?
       [x, x**2 for x in range(6)]
                  ^
   SyntaxError: invalid syntax
   >>> # flatten a list using a listcomp with two 'for'
   >>> vec = [[1,2,3], [4,5,6], [7,8,9]]
   >>> [num for elem in vec for num in elem]
   [1, 2, 3, 4, 5, 6, 7, 8, 9]

列表推导式可使用复杂的表达式和嵌套函数::

   >>> from math import pi
   >>> [str(round(pi, i)) for i in range(1, 6)]
   ['3.1', '3.14', '3.142', '3.1416', '3.14159']


嵌套的列表推导式
'''''''''''''''''''

列表推导式可以嵌套。

考虑以下的 3x4 矩阵，一个列表中包含三个长度为4的列表::

   >>> matrix = [
   ...     [1, 2, 3, 4],
   ...     [5, 6, 7, 8],
   ...     [9, 10, 11, 12],
   ... ]

现在，如果你想交换行和列，可以用嵌套的列表推导式::

   >>> [[row[i] for row in matrix] for i in range(4)]
   [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

像前面看到的，嵌套的列表推导式是对 `for <https://docs.python.org/2.7/reference/compound_stmts.html#for>`_ 后面的内容进行求值，所以上例就等价于::

   >>> transposed = []
   >>> for i in range(4):
   ...     transposed.append([row[i] for row in matrix])
   ...
   >>> transposed
   [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

反过来说，如下也是一样的::

   >>> transposed = []
   >>> for i in range(4):
   ...     # the following 3 lines implement the nested listcomp
   ...     transposed_row = []
   ...     for row in matrix:
   ...         transposed_row.append(row[i])
   ...     transposed.append(transposed_row)
   ...
   >>> transposed
   [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

在实际中，你应该更喜欢使用内置函数组成复杂流程语句。对此种情况 `zip() <https://docs.python.org/2.7/library/functions.html#zip>`_ 函数将会做的更好::

   >>> list(zip(*matrix))
   [(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]

更多关于本行中使用的星号的说明，参考 :ref:`tut-unpacking-arguments` 。

.. _tut-del:

`del <https://docs.python.org/2.7/reference/simple_stmts.html#del>`_ 语句
==================================================================================

有个方法可以从列表中按给定的索引而不是值来删除一个子项：`del <https://docs.python.org/2.7/reference/simple_stmts.html#del>`_ 语句。它不同于有返回值的 :meth:`pop` 方法。语句 `del <https://docs.python.org/2.7/reference/simple_stmts.html#del>`_  还可以从列表中删除切片或清空整个列表(我们以前介绍过一个方法是将空列表赋值给列表的切片)。例如::

   >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
   >>> del a[0]
   >>> a
   [1, 66.25, 333, 333, 1234.5]
   >>> del a[2:4]
   >>> a
   [1, 66.25, 1234.5]
   >>> del a[:]
   >>> a
   []

`del <https://docs.python.org/2.7/reference/simple_stmts.html#del>`_ 也可以删除整个变量::

   >>> del a

此后再引用命名 ``a`` 会引发错误(直到另一个值赋给它为止)。我们在后面的内容中可以看到 `del <https://docs.python.org/2.7/reference/simple_stmts.html#del>`_ 的其它用法。


.. _tut-tuples:

元组和序列
====================

我们知道链表和字符串有很多通用的属性，例如索引和切割操作。它们是序列类型(参见 `Sequence Types — str, unicode, list, tuple, bytearray, buffer, xrange <https://docs.python.org/2.7/library/stdtypes.html#typesseq>`_ )中的两种。因为 Python 是一个在不断进化的语言，也可能会加入其它的序列类型，这里介绍另一种标准序列类型：*元组*。 

一个元组由数个逗号分隔的值组成，例如::

   >>> t = 12345, 54321, 'hello!'
   >>> t[0]
   12345
   >>> t
   (12345, 54321, 'hello!')
   >>> # Tuples may be nested:
   ... u = t, (1, 2, 3, 4, 5)
   >>> u
   ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
   >>> # Tuples are immutable:
   ... t[0] = 88888
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: 'tuple' object does not support item assignment
   >>> # but they can contain mutable objects:
   ... v = ([1, 2, 3], [3, 2, 1])
   >>> v
   ([1, 2, 3], [3, 2, 1])


如你所见，元组在输出时总是有括号的，以便于正确表达嵌套结构。在输入时可以有或没有括号，不过经常括号都是必须的(如果元组是一个更大的表达式的一部分)。不能给元组的一个独立的元素赋值(尽管你可以通过联接和切割来模拟)。还可以创建包含可变对象的元组，例如链表。

虽然元组看起来类似于列表，它们经常用于不同的场景和不同的目的。元组是 `不可变的 <https://docs.python.org/2.7/glossary.html#term-immutable>`_，通常包含不同种类的元素并通过分拆（参阅本节后面的内容）或索引访问（如果是 `namedtuples <https://docs.python.org/2.7/library/collections.html#collections.namedtuple>`_，甚至可以通过属性）。列表是 `可变的 <https://docs.python.org/2.7/glossary.html#term-mutable>`_，它们的元素通常是相同的类型并通过迭代访问。

一个特殊的问题是构造包含零个或一个元素的元组：为了适应这种情况，语法上有一些额外的改变。一对空的括号可以创建空元组；要创建一个单元素元组可以在值后面跟一个逗号(在括号中放入一个单值不够明确)。丑陋，但是有效。例如::

   >>> empty = ()
   >>> singleton = 'hello',    # <-- note trailing comma
   >>> len(empty)
   0
   >>> len(singleton)
   1
   >>> singleton
   ('hello',)

语句 ``t = 12345, 54321, 'hello!'`` 是 *元组封装* (tuple packing)的一个例子：值 ``12345`` ， ``54321`` 和 ``'hello!'`` 被封装进元组。其逆操作可能是这样::

   >>> x, y, z = t

这个调用等号右边可以是任何线性序列，称之为 *序列拆封* 非常恰当。序列拆封要求左侧的变量数目与序列的元素个数相同。要注意的是可变参数(multiple assignment)其实只是元组封装和序列拆封的一个结合。


.. _tut-sets:

集合
====

Python 还包含了一个数据类型 *set* (集合)。集合是一个无序不重复元素的集。基本功能包括关系测试和消除重复元素。集合对象还支持 union(联合)，intersection(交)，difference(差)和 sysmmetric difference(对称差集)等数学运算。 

大括号或 `set() <https://docs.python.org/2.7/library/stdtypes.html#set>`_ 函数可以用来创建集合。注意：想要创建空集合，你必须使用 ``set()`` 而不是 ``{}``。后者用于创建空字典，我们在下一节中介绍的一种数据结构。

以下是一个简单的演示::

   >>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
   >>> fruit = set(basket)               # create a set without duplicates
   >>> fruit
   set(['orange', 'pear', 'apple', 'banana'])
   >>> 'orange' in fruit                 # fast membership testing
   True
   >>> 'crabgrass' in fruit
   False

   >>> # Demonstrate set operations on unique letters from two words
   ...
   >>> a = set('abracadabra')
   >>> b = set('alacazam')
   >>> a                                  # unique letters in a
   set(['a', 'r', 'b', 'c', 'd'])
   >>> a - b                              # letters in a but not in b
   set(['r', 'd', 'b'])
   >>> a | b                              # letters in either a or b
   set(['a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'])
   >>> a & b                              # letters in both a and b
   set(['a', 'c'])
   >>> a ^ b                              # letters in a or b but not both
   set(['r', 'd', 'b', 'm', 'z', 'l'])

类似 :ref:`列表推导式 <tut-listcomps>`，这里有一种集合推导式语法::

   >>> a = {x for x in 'abracadabra' if x not in 'abc'}
   >>> a
   {'r', 'd'}



.. _tut-dictionaries:

字典
============

另一个非常有用的 Python 内建数据类型是 *字典* (参见 `Mapping Types — dict <https://docs.python.org/2.7/library/stdtypes.html#typesmapping>`_ )。字典在某些语言中可能称为 联合内存 (associative memories) 或 联合数组 (associative arrays)。序列是以连续的整数为索引，与此不同的是，字典以 *关键字* 为索引，关键字可以是任意不可变类型，通常用字符串或数值。如果元组中只包含字符串和数字，它可以作为关键字，如果它直接或间接地包含了可变对象，就不能当做关键字。不能用链表做关键字，因为链表可以用索引、切割或者 :meth:`append` 和 :meth:`extend` 等方法改变。 

理解字典的最佳方式是把它看做无序的键：*值对* (key:value 对)集合，键必须是互不相同的(在同一个字典之内)。一对大括号创建一个空的字典：``{}``。初始化链表时，在大括号内放置一组逗号分隔的键：值对，这也是字典输出的方式。 

字典的主要操作是依据键来存储和析取值。也可以用 ``del`` 来删除键：值对(key:value)。如果你用一个已经存在的关键字存储值，以前为该关键字分配的值就会被遗忘。试图从一个不存在的键中取值会导致错误。

对一个字典执行 :meth:`keys` 将返回一个字典中所有关键字组成的无序列表(如果你想要排序，只需使用 `sorted() <https://docs.python.org/2.7/library/functions.html#sorted>`_)。使用 `in <https://docs.python.org/2.7/reference/expressions.html#in>`_ 关键字(指 Python 语法)可以检查字典中是否存在某个关键字(指字典)。

这里是使用字典的一个小示例::

   >>> tel = {'jack': 4098, 'sape': 4139}
   >>> tel['guido'] = 4127
   >>> tel
   {'sape': 4139, 'guido': 4127, 'jack': 4098}
   >>> tel['jack']
   4098
   >>> del tel['sape']
   >>> tel['irv'] = 4127
   >>> tel
   {'guido': 4127, 'irv': 4127, 'jack': 4098}
   >>> tel.keys()
   ['guido', 'irv', 'jack']
   >>> 'guido' in tel
   True

`dict() <https://docs.python.org/2.7/library/stdtypes.html#dict>`_ 构造函数可以直接从 key-value 对中创建字典::

   >>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
   {'sape': 4139, 'jack': 4098, 'guido': 4127}

此外，字典推导式可以从任意的键值表达式中创建字典::

   >>> {x: x**2 for x in (2, 4, 6)}
   {2: 4, 4: 16, 6: 36}

如果关键字都是简单的字符串，有时通过关键字参数指定 key-value 对更为方便::

   >>> dict(sape=4139, guido=4127, jack=4098)
   {'sape': 4139, 'jack': 4098, 'guido': 4127}


.. _tut-loopidioms:

循环技巧
==================

在序列中循环时，索引位置和对应值可以使用 `enumerate() <https://docs.python.org/2.7/library/functions.html#enumerate>`_ 函数同时得到::

   >>> for i, v in enumerate(['tic', 'tac', 'toe']):
   ...     print(i, v)
   ...
   0 tic
   1 tac
   2 toe

同时循环两个或更多的序列，可以使用 `zip() <https://docs.python.org/2.7/library/functions.html#zip>`_ 整体打包::

   >>> questions = ['name', 'quest', 'favorite color']
   >>> answers = ['lancelot', 'the holy grail', 'blue']
   >>> for q, a in zip(questions, answers):
   ...     print 'What is your {0}?  It is {1}.'.format(q, a)
   ...
   What is your name?  It is lancelot.
   What is your quest?  It is the holy grail.
   What is your favorite color?  It is blue.

需要逆向循环序列的话，先正向定位序列，然后调用 `reversed() <https://docs.python.org/2.7/library/functions.html#reversed>`_ 函数::

   >>> for i in reversed(xrange(1, 10, 2)):
   ...     print(i)
   ...
   9
   7
   5
   3
   1

要按排序后的顺序循环序列的话，使用 `sorted() <https://docs.python.org/2.7/library/functions.html#sorted>`_  函数，它不改动原序列，而是生成一个新的已排序的序列::

   >>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
   >>> for f in sorted(set(basket)):
   ...     print f 
   ...
   apple
   banana
   orange
   pear

遍历字典时，使用 ``iteritems()`` 方法可以同时得到键和对应的值。::

   >>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
   >>> for k, v in knights.iteritems():
   ...     print k, v
   ...
   gallahad the pure
   robin the brave

若要在循环内部修改正在遍历的序列（例如复制某些元素），建议您首先制作副本。在序列上循环不会隐式地创建副本。切片表示法使这尤其方便::

   >>> words = ['cat', 'window', 'defenestrate']
   >>> for w in words[:]:  # Loop over a slice copy of the entire list.
   ...     if len(w) > 6:
   ...         words.insert(0, w)
   ...
   >>> words
   ['defenestrate', 'cat', 'window', 'defenestrate']


.. _tut-conditions:

深入条件控制
==================

``while`` 和 ``if`` 语句中使用的条件不仅可以使用比较，而且可以包含任意的操作。 

比较操作符 ``in`` 和 ``not in`` 用来判断值是否在一个区间之内。操作符 ``is`` 和 ``is not`` 比较两个对象是否相同；这只和诸如链表这样的可变对象有关。所有的比较操作符具有相同的优先级，低于所有的数值操作。 

比较操作可以传递。例如 ``a < b == c`` 判断是否 ``a`` 小于 ``b`` 并且 ``b`` 等于 ``c`` 。 

比较操作可以通过逻辑操作符 ``and`` 和 ``or`` 组合，比较的结果可以用 ``not`` 来取反义。这些操作符的优先级又低于比较操作符，在它们之中，``not`` 具有最高的优先级，``or`` 优先级最低，所以 ``A and not B or C`` 等于 ``(A and (notB)) or C``。当然，括号也可以用于比较表达式。 

逻辑操作符 ``and`` 和 ``or`` 也称作 短路操作符：它们的参数从左向右解析，一旦结果可以确定就停止。例如，如果 ``A`` 和 ``C`` 为真而 ``B`` 为假，``A and B and C`` 不会解析 ``C``。作用于一个普通的非逻辑值时，短路操作符的返回值通常是最后一个变量。 

可以把比较或其它逻辑表达式的返回值赋给一个变量，例如::

   >>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
   >>> non_null = string1 or string2 or string3
   >>> non_null
   'Trondheim'

需要注意的是 Python 与 C 不同，在表达式内部不能赋值。C 程序员经常对此抱怨，不过它避免了一类在 C 程序中司空见惯的错误：想要在解析式中使 ``==`` 时误用了 ``=`` 操作符。


.. _tut-comparing:

比较序列和其它类型
===================================

序列对象可以与相同类型的其它对象比较。比较操作按 *字典序* 进行：首先比较前两个元素，如果不同，就决定了比较的结果；如果相同，就比较后两个元素，依此类推，直到所有序列都完成比较。如果两个元素本身就是同样类型的序列，就递归字典序比较。如果两个序列的所有子项都相等，就认为序列相等。如果一个序列是另一个序列的初始子序列，较短的一个序列就小于另一个。字符串的字典序按照单字符的 ASCII 顺序。下面是同类型序列之间比较的一些例子::

   (1, 2, 3)              < (1, 2, 4)
   [1, 2, 3]              < [1, 2, 4]
   'ABC' < 'C' < 'Pascal' < 'Python'
   (1, 2, 3, 4)           < (1, 2, 4)
   (1, 2)                 < (1, 2, -1)
   (1, 2, 3)             == (1.0, 2.0, 3.0)
   (1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)

注意比较不同类型的对象也是合法的。比较的结果已经确定但是不一定合理： 类型按其名称进行排序。因此，列表始终小于字符串，字符串总是小于元组，等等。[#]_   不同数值类型按照它们的值比较，所以 0 等于 0.0，等等。


.. rubric:: Footnotes

.. [#] 不应该依赖不同类型对象的比较规则；它们在Python未来版本中可能发生变化。
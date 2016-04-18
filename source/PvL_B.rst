..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".
 
|

More datatypes
==============

You have already encountered the most important datatypes Python has to offer: bools, ints, floats, strings, tuples, lists and dictionaries. However, there is more to it than hinted at previously. In this section, we will focus mainly on tuples and lists, and introduce sets and frozensets.

Mutable versus immutable and aliasing
-------------------------------------

Some datatypes in Python are **mutable**. This means their contents can be changed after they have been created. Lists and dictionaries are good examples of mutable datatypes.

.. sourcecode:: python3
    
    >>> my_list = [2, 4, 5, 3, 6, 1]
    >>> my_list[0] = 9
    >>> my_list
    [9, 4, 5, 3, 6, 1]

Tuples and strings are examples of immutable datatypes, their contents can not be changed after they have been created:

.. sourcecode:: python3

    >>> my_tuple = (2, 5, 3, 1)
    >>> my_tuple[0] = 9
    Traceback (most recent call last):
      File "<interactive input>", line 2, in <module>
    TypeError: 'tuple' object does not support item assignment
    >>> 

Mutability is usually useful, but it may lead to something called aliasing. In this case, two variables refer to the same object and mutating one will also change the other:

.. sourcecode:: python3

    >>> list_one = [1, 2, 3, 4, 6]
    >>> list_two = list_one
    >>> list_two[-1] = 5
    >>> list_one
    [1, 2, 3, 4, 5]

This happens, because both list_one and list_two refer to the same memory address containing the actual list. You can check this using the built-in function ``id``:

.. sourcecode:: python3

    >>> list_one = [1, 2, 3, 4, 6]
    >>> list_two = list_one
    >>> id(list_one) == id(list_two)
    True

You can escape this problem by making a copy of the list: 

.. sourcecode:: python3

    >>> list_one = [1, 2, 3, 4, 6]
    >>> list_two = list_one[:]
    >>> id(list_one) == id(list_two)
    False
    >>> list_two[-1] = 5
    >>> list_two
    [1, 2, 3, 4, 5]
    >>> list_one
    [1, 2, 3, 4, 6]

However, this will not work for nested lists because of the same reason. The module ``copy`` provides functions to solve this.

Sets and frozensets
-------------------

Given that tuples and lists are ordered, and dictionaries are unordered, we can construct the following table.

+-------------+-----------+-------------+
|             |**Ordered**|**Unordered**|
+=============+===========+=============+
|**Mutable**  |list       |dict         |
+-------------+-----------+-------------+
|**Immutable**|tuple      |             |
+-------------+-----------+-------------+

This reveals an empty spot: we don't know any immutable, unordered datatypes yet. Additionally, you can argue that a dictionary doesn't belong in this table, since it is a *mapping* type whilst lists and tuples are not: a dictionary maps keys to values.
This is where sets and frozensets come in. A **set** is an unordered, mutable datatype; and a **frozenset** is an unordered, immutable datatype.

+-------------+-----------+-------------+
|             |**Ordered**|**Unordered**|
+=============+===========+=============+
|**Mutable**  |list       |set          |
+-------------+-----------+-------------+
|**Immutable**|tuple      |frozenset    |
+-------------+-----------+-------------+

Since sets and frozensets are unordered, they share some properties with dictionaries: for example, it's elements are unique. Creating a set, and adding elements to it is simple.

.. sourcecode:: python3
    
    >>> my_set = set([1, 4, 2, 3, 4])
    >>> my_set
    {1, 2, 3, 4}
    >>> my_set.add(13)
    >>> my_set
    {1, 2, 3, 4, 13}

Sets may seem sorted in the example above, but this is completely coincidental.
Sets also support common operations such as membership testing (``3 in my_set``); and iteration (``for x in my_set:``). Additionally, you can add and substract sets from eachother:

.. sourcecode:: python3
    :linenos:
    
    set1 = set([1, 2, 3])
    set2 = set([4, 5, 6])
    print(set1 | set2)  # {1, 2, 3, 4, 5, 6}
    print(set1 & set2)  # set()
    set2 = set([2, 3, 4, 5])
    print(set1 & set2)  # {2, 3}
    print(set1 - set2)  # {1}



Frozensets are mostly the same as set, other then that they can not be modified; i.e. you can't add or remove items. See also the documentation online_.

.. _online: https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset

More exotic data types - such as queues, stacks and ordered dictionaries - are provided in Python's ``collections`` module. You can find the documentation here_.

.. _here: https://docs.python.org/3/library/collections.html


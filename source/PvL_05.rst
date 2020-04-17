..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".

|
    
Data Types
==========

Strings
#######

.. index:: compound data type, character, subscript operator, index

A compound data type
--------------------

So far we have seen built-in types like ``int``, ``float``, 
``bool``, ``str`` and we've seen lists and pairs. 
Strings, lists, and pairs are qualitatively different from the others because they
are made up of smaller pieces.  In the case of strings, they're made up of smaller
strings each containing one **character**.  

Types that comprise smaller pieces are called **compound data types**.
Depending on what we are doing, we may want to treat a compound data type as a
single thing, or we may want to access its parts. This ambiguity is useful.

Working with strings as single things
-------------------------------------

We previously saw that each turtle instance has its own attributes and 
a number of methods that can be applied to the instance.  For example,
we could set the turtle's color, and we wrote ``tess.turn(90)``.  

Just like a turtle, a string is also an object.  So each string instance 
has its own attributes and methods.  

For example:

    .. sourcecode:: python3

        >>> our_string = "Hello, World!"
        >>> all_caps = our_string.upper()
        >>> all_caps
        'HELLO, WORLD!'
    
``upper`` is a method that can be invoked on any string object 
to create a new string, in which all the 
characters are in uppercase.  (The original string ``our_string`` remains unchanged.)

There are also methods such as ``lower``, ``capitalize``, and
``swapcase`` that do other interesting stuff.

To learn what methods are available, you can consult the Help documentation, look for 
string methods, and read the documentation.  Or, if you're a bit lazier, 
simply type the following into an editor like Spyder or PyScripter script: 

    .. sourcecode:: python3
        :linenos:
        
        our_string = "Hello, World!"
        new_string = our_string.
    
When you type the period to select one of the methods of ``our_string``, your editor might pop up a 
selection window — typically by pressing ``Tab`` — showing all the methods (there are around 70 of them --- thank goodness we'll only
use a few of those!) that could be used on your string. 

    .. image::  illustrations/string_methods.png
 
When you type the name of the method, some further help about its parameter and return
type, and its docstring, may be displayed by your scripting environments (for instance, in a Jupyter
notebook you can get this inofrmation by pressing ``Shift+Tab`` after a function name).

    .. image::  illustrations/swapcase.png

Working with the parts of a string
----------------------------------

The **indexing operator** (Python uses square brackets to enclose the index) 
selects a single character substring from a string:

    .. sourcecode:: python3
        
        >>> fruit = "banana"
        >>> letter = fruit[1]
        >>> print(letter)

        
The expression ``fruit[1]`` selects character number 1 from ``fruit``, and creates a new
string containing just this one character. The variable ``letter`` refers to the result. 
When we display ``letter``, we could get a surprise: 

    .. sourcecode:: pycon

        a

Computer scientists always start counting
from zero! The letter at subscript position zero of ``"banana"`` is ``b``.  So at
position ``[1]`` we have the letter ``a``.

If we want to access the zero-eth letter of a string, we just place 0,
or any expression that evaluates to 0, inbetween the brackets:

    .. sourcecode:: python3
        
        >>> letter = fruit[0]
        >>> print(letter)
        b

The expression in brackets is called an **index**. An index specifies a member
of an ordered collection, in this case the collection of characters in the string. The index
*indicates* which one you want, hence the name. It can be any integer
expression.

We can use ``enumerate`` to visualize the indices:

    .. sourcecode:: python3

        >>> fruit = "banana"
        >>> list(enumerate(fruit))
        [(0, 'b'), (1, 'a'), (2, 'n'), (3, 'a'), (4, 'n'), (5, 'a')]

Do not worry about ``enumerate`` at this point, we will see more of it
in the chapter on lists.

Note that indexing returns a *string* --- Python has no special type for a single character.
It is just a string of length 1.

We've also seen lists previously.  The same indexing notation works to extract elements from
a list: 

    .. sourcecode:: python3

        >>> prime_numbers = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31]
        >>> prime_numbers[4]
        11
        >>> friends = ["Joe", "Zoe", "Brad", "Angelina", "Zuki", "Thandi", "Paris"]
        >>> friends[3]
        'Angelina'


.. index::
    single: len function
    single: function; len
    single: runtime error
    single: negative index
    single: index; negative

Length
------

The ``len`` function, when applied to a string, returns the number of characters in a string:

    .. sourcecode:: python3
        
        >>> word = "banana"
        >>> len(word)
        6

To get the last letter of a string, you might be tempted to try something like
this:

    .. sourcecode:: python3
        :linenos:
        
        size = len(word)
        last = word[size]       # ERROR!

That won't work. It causes the runtime error
``IndexError: string index out of range``. The reason is that there is no
character at index position 6 in ``"banana"``. 
Because we start counting at zero, the six indexes are
numbered 0 to 5. To get the last character, we have to subtract 1 from
the length of ``word``:

    .. sourcecode:: python3
        :linenos:
            
        size = len(word)
        last = word[size-1]

Alternatively, we can use **negative indices**, which count backward from the
end of the string. The expression ``word[-1]`` yields the last letter,
``word[-2]`` yields the second to last, and so on.

As you might have guessed, indexing with a negative index also works like this for lists. 


.. index:: traversal, for loop, concatenation, abecedarian series

.. index::
    single: McCloskey, Robert
    single: Make Way for Ducklings    

Traversal and the ``for`` loop
------------------------------

A lot of computations involve processing a string one character at a time.
Often they start at the beginning, select each character in turn, do something
to it, and continue until the end. This pattern of processing is called a
**traversal**. One way (a very bad way) to encode a traversal is with a ``while`` statement:

    .. sourcecode:: python3
        :linenos:
             
        ix = 0
        while ix < len(fruit):
            letter = fruit[ix]
            print(letter)
            ix += 1

This loop traverses the string and displays each letter on a line by itself.
It uses ix for the index, which does not make it any clearer. 
The loop condition is ``ix < len(fruit)``, so when ``ix`` is equal to the
length of the string, the condition is false, and the body of the loop is not
executed. The last character accessed is the one with the index
``len(fruit)-1``, which is the last character in the string. 
However, this code is a lot longer than it needs to be, and not very clear at all.

But we've previously seen how the ``for`` loop can easily iterate over
the elements in a list and it can do so for strings as well:

    .. sourcecode:: python3
        :linenos:

        word="Banana"
        for letter in word:
            print(letter)

Each time through the loop, the next character in the string is assigned to the
variable ``c``. The loop continues until no characters are left. Here we
can see the expressive power the ``for`` loop gives us compared to the
while loop when traversing a string.

The following example shows how to use concatenation and a ``for`` loop to
generate an abecedarian series. Abecedarian refers to a series or list in which
the elements appear in alphabetical order. For example, in Robert McCloskey's
book *Make Way for Ducklings*, the names of the ducklings are Jack, Kack, Lack,
Mack, Nack, Ouack, Pack, and Quack.  This loop outputs these names in order:

    .. sourcecode:: python3
        :linenos:
        
        prefixes = "JKLMNOPQ"
        suffix = "ack"
           
        for p in prefixes:
            print(p + suffix)

The output of this program is: 
 
    .. sourcecode:: pycon 

            Jack
            Kack
            Lack
            Mack
            Nack
            Oack
            Pack
            Qack


Of course, that's not quite right because Ouack and Quack are misspelled.
You'll fix this as an exercise below.


.. index:: slice, string slice, substring, sublist

Slices
------

A *substring* of a string is obtained by taking a **slice**.   Similarly, we can
slice a list to refer to some sublist of the items in the list:

    .. sourcecode:: python3
        
        >>> phrase = "Pirates of the Caribbean"
        >>> print(phrase[0:7])
        Pirates
        >>> print(phrase[11:14])
        the
        >>> print(phrase[13:24])
        e Caribbean
        >>> friends = ["Joe", "Zoe", "Brad", "Angelina", "Zuki", "Thandi", "Paris"]
        >>> print(friends[2:4])
        ['Brad', 'Angelina']

The operator ``[n:m]`` returns the part of the string from the n'th character
to the m'th character, including the first but excluding the last. This
behavior makes sense if you imagine the indices
pointing *between* the characters, as in the following diagram:

    .. image:: illustrations/banana.png
       :alt: 'banana' string

If you imagine this as a piece of paper, the slice operator ``[n:m]`` copies out
the part of the paper between the ``n`` and ``m`` positions.  Provided ``m`` and ``n`` are
both within the bounds of the string, your result will be of length (m-n).
   
Three tricks are added to this: if you omit the first index (before the colon), 
the slice starts at the beginning of the string (or list). If you omit the second index, 
the slice extends to the end of the string (or list). Similarly, if you provide value for
``n`` that is bigger than the length of the string (or list), the slice will take all the 
values up to the end. (It won't give an "out of range" error like the normal indexing operation
does.)   Thus:

    .. sourcecode:: python3
        
        >>> word = "banana"
        >>> word[:3]
        'ban'
        >>> word[3:]
        'ana'
        >>> word[3:999]
        'ana'

What do you think ``phrase[:]`` means?   What about ``friends[4:]``? ``phrase[-5:-3]``?


.. index:: string comparison, comparison of strings

String comparison
-----------------

The comparison operators work on strings. To see if two strings are equal:

    .. sourcecode:: python3
        :linenos:
        
        if word == "banana":
            print("Yes, we have no bananas!")

Other comparison operations are useful for putting words in
`lexicographical` order:

    .. sourcecode:: python3
        :linenos:
        
        if word < "banana":
            print("Your word, " + word + ", comes before banana.")
        elif word > "banana":
            print("Your word, " + word + ", comes after banana.")
        else:
            print("Yes, we have no bananas!")

This is similar to the alphabetical order you would use with a dictionary,
except that all the uppercase letters come before all the lowercase letters. As
a result:

    .. sourcecode:: pycon
        
        Your word, Zebra, comes before banana.

A common way to address this problem is to convert strings to a standard
format, such as all lowercase, before performing the comparison. A more
difficult problem is making the program realize that zebras are not fruit.


.. index:: mutable, immutable, runtime error

Strings are immutable
---------------------

It is tempting to use the ``[]`` operator on the left side of an assignment,
with the intention of changing a character in a string.  For example:

    .. sourcecode:: python3
        :linenos:
        
        greeting = "Hello, world!"
        greeting[0] = 'J'            # ERROR!
        print(greeting)

Instead of producing the output ``Jello, world!``, this code produces the
runtime error ``TypeError: 'str' object does not support item assignment``.

Strings are **immutable**, which means you can't change an existing string. The
best you can do is create a new string that is a variation on the original:

    .. sourcecode:: python3
        :linenos:
        
        greeting = "Hello, world!"
        new_greeting = "J" + greeting[1:]
        print(new_greeting)

The solution here is to concatenate a new first letter onto a slice of
``greeting``. This operation has no effect on the original string.


.. index::
    single: in operator
    single: operator; in

The ``in`` and ``not in`` operators
-----------------------------------

The ``in`` operator tests for membership. When both of the arguments to ``in``
are strings, ``in`` checks whether the left argument is a substring of the right
argument.

    .. sourcecode:: python3
        
        >>> "p" in "apple"
        True
        >>> "i" in "apple"
        False
        >>> "ap" in "apple"
        True
        >>> "pa" in "apple"
        False

Note that a string is a substring of itself, and the empty string is a 
substring of any other string. (Also note that computer scientists 
like to think about these edge cases quite carefully!) 

    .. sourcecode:: python3
        
        >>> "a" in "a"
        True
        >>> "apple" in "apple"
        True
        >>> "" in "a"
        True
        >>> "" in "apple"
        True
    
The ``not in`` operator returns the logical opposite results of ``in``: 

    .. sourcecode:: python3
        
        >>> "x" not in "apple"
        True

Combining the ``in`` operator with string concatenation using ``+``, we can
write a function that removes all the vowels from a string:

    .. sourcecode:: python3
        :linenos:
        
        def remove_vowels(phrase):
            vowels = "aeiou"
            string_sans_vowels = ""
            for letter in phrase:
                if letter.lower() not in vowels:
                    string_sans_vowels += letter
            return string_sans_vowels

Important to note is the ``letter.lower()`` in line 5, without it, any uppercase vowels would not be removed. 

.. index:: traversal, eureka traversal, short-circuit evaluation, pattern of computation,
           computation pattern

A ``find`` function
-------------------

What does the following function do?

    .. sourcecode:: python3
        :linenos:
        
        def my_find(haystack, needle):
            """
              Find and return the index of needle in haystack.  
              Return -1 if needle does not occur in haystack.
            """
            for index, letter in enumerate(haystack):
                if letter == needle:
                    return index
            return -1
            
    
Compare the output of the code above with what Python does itself with the code below:

    .. sourcecode:: python3
        :linenos:

        haystack = "Bananarama!"
        print(haystack.find('a'))
        print(my_find(haystack,'a'))


In a sense, ``find`` is the opposite of the indexing operator. Instead of taking
an index and extracting the corresponding character, it takes a character and
finds the index where that character appears. If the character is not found,
the function returns ``-1``.

This is another example where we see a ``return`` statement inside a loop.
If ``letter == needle``, the function returns immediately, breaking out of
the loop prematurely.

If the character doesn't appear in the string, then the program exits the loop
normally and returns ``-1``.

This pattern of computation is sometimes called a **eureka traversal** or
**short-circuit evaluation**,  because as soon as we find what we are looking for, 
we can cry "Eureka!", take the short-circuit, and stop looking.


.. index:: counting pattern

Looping and counting
--------------------

The following program counts the number of times the letter ``a`` appears in a
string, and is another example of the counter pattern introduced in
:ref:`counting`:

    .. sourcecode:: python3
        :linenos:
        
        def count_a(text): 
            count = 0
            for letter in text:
                if letter == "a":
                    count += 1
            return(count)

        print(count_a("banana") == 3)    

.. index:: optional parameter, default value, parameter; optional

.. _optional_parameters:

Optional parameters
-------------------

To find the locations of the second or third occurrence of a character in a
string, we can modify the ``find`` function, adding a third parameter for the
starting position in the search string:

    .. sourcecode:: python3
        :linenos:
        
        def find2(haystack, needle, start):
            for index,letter in enumerate(haystack[start:]):
                if letter == needle:
                    return index + start
            return -1
            
    
            
        print(find2("banana", "a", 2) == 3)

The call ``find2("banana", "a", 2)`` now returns ``3``, the index of the first
occurrence of "a" in "banana" starting the search at index 2. What does
``find2("banana", "n", 3)`` return? If you said, 4, there is a good chance you
understand how ``find2`` works.

Better still, we can combine ``find`` and ``find2`` using an
**optional parameter**:

    .. sourcecode:: python3
        :linenos:
        
        def find(haystack, needle, start=0):
            for index,letter in enumerate(haystack[start:]):
                if letter == needle:
                    return index + start
            return -1
            

When a function has an optional parameter, the caller `may` provide a 
matching argument. If the third argument is provided to ``find``, it gets assigned 
to ``start``.  But if the caller leaves the argument out, then start is given
a default value indicated by the assignment ``start=0`` in the function definition.
 
So the call ``find("banana", "a", 2)`` to this version of ``find`` behaves just
like ``find2``, while in the call ``find("banana", "a")``, ``start`` will be
set to the **default value** of ``0``.

Adding another optional parameter to ``find`` makes it search from a starting
position, up to but not including the end position:

    .. sourcecode:: python3
        :linenos:
        
        def find(haystack, needle, start=0, end=-1):
            for index,letter in enumerate(haystack[start:end])
                if letter == needle:
                    return index + start
            return -1
            
The semantics of ``start`` and ``end`` in this function are precisely the same as they are in
the ``range`` function.

.. index:: module, string module, dir function, dot notation, function type,
           docstring

The built-in ``find`` method
----------------------------
 
Now that we've done all this work to write a powerful ``find`` function, we can reveal that
strings already have their own built-in ``find`` method.  It can do everything 
that our code can do, and more! Try all the examples listed above, and check the results! 

The built-in ``find`` method is more general than our version. It can find
substrings, not just single characters:

    .. sourcecode:: python3
        
        >>> "banana".find("nan")
        2
        >>> "banana".find("na", 3)
        4

Usually we'd prefer to use the methods that Python provides rather than reinvent
our own equivalents. But many of the built-in functions and methods make good
teaching exercises, and the underlying techniques you learn are your building blocks
to becoming a proficient programmer.

The ``split`` method
--------------------

One of the most useful methods on strings is the ``split`` method:
it splits a single multi-word string into a list of individual words, removing
all the whitespace between them.  (Whitespace means any tabs, newlines, or spaces.)
This allows us to read input as a single string,
and split it into words.

    .. sourcecode:: python3 
    
        >>> phrase = "Well I never did said Alice" 
        >>> words = phrase.split()
        >>> words
        ['Well', 'I', 'never', 'did', 'said', 'Alice']
    
Cleaning up your strings
------------------------

We'll often work with strings that contain punctuation, or tab and newline characters,
especially, as we'll see in a future chapter, when we read our text from files or from 
the Internet. But if we're writing a program, say, to count word frequencies or check the
spelling of each word, we'd prefer to strip off these unwanted characters.

We'll show just one example of how to strip punctuation from a string.
Remember that strings are immutable, so we cannot change the string with the
punctuation --- we need to traverse the original string and create a new string,
omitting any punctuation:

    .. sourcecode:: python3 
        :linenos:   
     
        punctuation = "!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~"
        
        def remove_punctuation(phrase):
            phrase_sans_punct = ""
            for letter in phrase:
                if letter not in punctuation:
                    phrase_sans_punct += letter
            return phrase_sans_punct

Setting up that first assignment is messy and error-prone.  
Fortunately, the Python ``string`` module already does it
for us.  So we will make a slight improvement to this 
program --- we'll import the ``string`` module and use its definition: 

    .. sourcecode:: python3 
        :linenos:

        import string
        
        def remove_punctuation(phrase):
            phrase_sans_punct = ""
            for letter in phrase:
                if letter not in string.punctuation:
                    phrase_sans_punct += letter
            return phrase_sans_punct
    
Try the examples below: 
"Well, I never did!", said Alice.
"Are you very, very, sure?"


Composing together this function and the ``split`` method from the previous section
makes a useful combination --- we'll clean out the punctuation, and
``split`` will clean out the newlines and tabs while turning the string into
a list of words:

    .. sourcecode:: python3 
           :linenos:

           my_story = """
           Pythons are constrictors, which means that they will 'squeeze' the life 
           out of their prey. They coil themselves around their prey and with 
           each breath the creature takes the snake will squeeze a little tighter 
           until they stop breathing completely. Once the heart stops the prey 
           is swallowed whole. The entire animal is digested in the snake's 
           stomach except for fur or feathers. What do you think happens to the fur, 
           feathers, beaks, and eggshells? The 'extra stuff' gets passed out as --- 
           you guessed it --- snake POOP! """
           
           words = remove_punctuation(my_story).split()
           print(words)
       
The output: 

    .. sourcecode:: pycon  
    
       ['Pythons', 'are', 'constrictors', ... , 'it', 'snake', 'POOP']                            
  
There are other useful string methods, but this book isn't intended to
be a reference manual. On the other hand, the *Python Library Reference*
is. Along with a wealth of other documentation, it is available at
the `Python website <http://www.python.org>`__.


.. index:: string formatting, operations on strings, formatting; strings, justification, field width

The string format method 
------------------------
 
The easiest and most powerful way to format a string in Python 3 is to use the
``format`` method.  To see how this works, let's start with a few examples:

    .. sourcecode:: python3
        :linenos:
        
        phrase = "His name is {0}!".format("Arthur")
        print(phrase)

        name = "Alice"
        age = 10
        phrase = "I am {1} and I am {0} years old.".format(age, name)
        print(phrase)
        phrase = "I am {0} and I am {1} years old.".format(age, name)
        print(phrase)

        x = 4
        y = 5
        phrase = "2**10 = {0} and {1} * {2} = {3:f}".format(2**10, x, y, x * y)
        print(phrase)
    
Running the script produces: 

    .. sourcecode:: pycon
    
        His name is Arthur!
        I am Alice and I am 10 years old.
        I am 10 and I am Alice years old.
        2**10 = 1024 and 4 * 5 = 20.000000

The template string contains *place holders*,  ``... {0} ... {1} ... {2} ...`` etc.   
The ``format`` method substitutes its arguments into the place holders.
The numbers in the place holders are indexes that determine which argument
gets substituted --- make sure you understand line 6 above! 

But there's more!  Each of the replacement fields can also contain a **format specification** ---
it is always introduced by the ``:`` symbol  (Line 13 above uses one.)  
This modifies how the substitutions are made into the template, and can control things like:

* whether the field is aligned to the left ``<``, center ``^``, or right ``>``
* the width allocated to the field within the result string (a number like ``10``)
* the type of conversion (we'll initially only force conversion to float, ``f``, as we did in
  line 13 of the code above, or perhaps we'll ask integer numbers to be converted to hexadecimal using ``x``)
* if the type conversion is a float, you can also specify how many decimal places are wanted 
  (typically, ``.2f`` is useful for working with currencies to two decimal places.)

Let's do a few simple and common examples that should be enough for most needs.  If you need to
do anything more esoteric, use *help* and read all the powerful, gory details.

    .. sourcecode:: python3
        :linenos:

        name1 = "Paris"
        name2 = "Whitney"
        name3 = "Hilton"

        print("Pi to three decimal places is {0:.3f}".format(3.1415926))
        print("123456789 123456789 123456789 123456789 123456789 123456789")
        print("|||{0:<15}|||{1:^15}|||{2:>15}|||Born in {3}|||" 
                .format(name1,name2,name3,1981))
        print("The decimal value {0} converts to hex value {0:x}"
                .format(123456))

This script produces the output: 

    .. sourcecode:: pycon

        Pi to three decimal places is 3.142
        123456789 123456789 123456789 123456789 123456789 123456789
        |||Paris          |||    Whitney    |||         Hilton|||Born in 1981|||
        The decimal value 123456 converts to hex value 1e240
    
You can have multiple placeholders indexing the
same argument, or perhaps even have extra arguments that are not referenced
at all:

    .. sourcecode:: python3
        :linenos:

        letter = """
        Dear {0} {2}.
         {0}, I have an interesting money-making proposition for you!
         If you deposit $10 million into my bank account, I can 
         double your money ...
        """

        print(letter.format("Paris", "Whitney", "Hilton"))
        print(letter.format("Bill", "Henry", "Gates"))
    
This produces the following:

    .. sourcecode:: pycon
        
        Dear Paris Hilton.
         Paris, I have an interesting money-making proposition for you!
         If you deposit $10 million into my bank account, I can
         double your money ...
         
         
        Dear Bill Gates.
         Bill, I have an interesting money-making proposition for you!
         If you deposit $10 million into my bank account I can
         double your money ...


As you might expect, you'll get an index error if 
your placeholders refer to arguments that you do not provide: 

    .. sourcecode:: python3
    
        >>> "hello {3}".format("Dave")
        Traceback (most recent call last):
          File "<interactive input>", line 1, in <module>
        IndexError: tuple index out of range
    
The following example illustrates the real utility of string formatting.
First, we'll try to print a table without using string formatting:

    .. sourcecode:: python3
        :linenos:
        
        print("i\ti**2\ti**3\ti**5\ti**10\ti**20")
        for i in range(1, 11):
            print(i, "\t", i**2, "\t", i**3, "\t", i**5, "\t", 
                                                    i**10, "\t", i**20)

This program prints out a table of various powers of the numbers from 1 to 10.
(This assumes that the tab width is 8.  You might see
something even worse than this if you tab width is set to 4.)
In its current form it relies on the tab character ( ``\t``) to align the
columns of values, but this breaks down when the values in the table get larger
than the tab width:

    .. sourcecode:: pycon
        
        i       i**2    i**3    i**5    i**10   i**20
        1       1       1       1       1       1
        2       4       8       32      1024    1048576
        3       9       27      243     59049   3486784401
        4       16      64      1024    1048576         1099511627776
        5       25      125     3125    9765625         95367431640625
        6       36      216     7776    60466176        3656158440062976
        7       49      343     16807   282475249       79792266297612001
        8       64      512     32768   1073741824      1152921504606846976
        9       81      729     59049   3486784401      12157665459056928801
        10      100     1000    100000  10000000000     100000000000000000000

One possible solution would be to change the tab width, but the first column
already has more space than it needs. The best solution would be to set the
width of each column independently. As you may have guessed by now, string
formatting provides a much nicer solution.  We can also right-justify each field:

    .. sourcecode:: python3
        :linenos:
            
        layout = "{0:>4}{1:>6}{2:>6}{3:>8}{4:>13}{5:>24}"

        print(layout.format("i", "i**2", "i**3", "i**5", "i**10", "i**20"))
        for i in range(1, 11):
            print(layout.format(i, i**2, i**3, i**5, i**10, i**20))
 

Running this version produces the following (much more satisfying) output: 

    .. sourcecode:: pycon
        
       i  i**2  i**3    i**5        i**10                   i**20
       1     1     1       1            1                       1
       2     4     8      32         1024                 1048576
       3     9    27     243        59049              3486784401
       4    16    64    1024      1048576           1099511627776
       5    25   125    3125      9765625          95367431640625
       6    36   216    7776     60466176        3656158440062976
       7    49   343   16807    282475249       79792266297612001
       8    64   512   32768   1073741824     1152921504606846976
       9    81   729   59049   3486784401    12157665459056928801
      10   100  1000  100000  10000000000   100000000000000000000


Summary 
------- 

This chapter introduced a lot of new ideas.  The following summary 
may prove helpful in remembering what you learned.

.. glossary::

    indexing (``[]``)
        Access a single character in a string using its position (starting from
        0).  Example: ``"This"[2]`` evaluates to ``"i"``.

    length function (``len``)
        Returns the number of characters in a string.  Example:
        ``len("happy")`` evaluates to ``5``.

    for loop traversal (``for``)
        *Traversing* a string means accessing each character in the string, one
        at a time.  For example, the following for loop:

            .. sourcecode:: python3

                for ch in "Example":
                    ...

        executes the body of the loop 7 times with different values of ``ch`` each time.

    slicing (``[:]``)
        A *slice* is a substring of a string. Example: ``'bananas and
        cream'[3:6]`` evaluates to ``ana`` (so does ``'bananas and
        cream'[1:4]``).

    string comparison (``>, <, >=, <=, ==, !=``)
        The six common comparison operators work with strings, evaluating according to
        `lexicographical` order.  Examples:
        ``"apple" < "banana"`` evaluates to ``True``.  ``"Zeta" < "Appricot"``
        evaluates to ``False``.  ``"Zebra" <= "aardvark"`` evaluates to
        ``True`` because all upper case letters precede lower case letters.

    in and not in operator (``in``, ``not in``)
        The ``in`` operator tests for membership. In the case of
        strings, it tests whether one string is contained inside another
        string.  Examples: ``"heck" in "I'll be checking for you."``
        evaluates to ``True``.  ``"cheese" in "I'll be checking for
        you."`` evaluates to ``False``.


Glossary
--------

.. glossary::

    compound data type
        A data type in which the values are made up of components, or elements,
        that are themselves values.

    default value
        The value given to an optional parameter if no argument for it is
        provided in the function call.

    docstring
        A string constant on the first line of a function or module definition
        (and as we will see later, in class and method definitions as well).
        Docstrings provide a convenient way to associate documentation with
        code. Docstrings are also used by programming tools to provide interactive help.

    dot notation
        Use of the **dot operator**, ``.``, to access methods and attributes of an object.

    immutable data value
        A data value which cannot be modified.  Assignments to elements or
        slices (sub-parts) of immutable values cause a runtime error.

    index
        A variable or value used to select a member of an ordered collection, such as
        a character from a string, or an element from a list.

    mutable data value
        A data value which can be modified. The types of all mutable values 
        are compound types.  Lists and dictionaries are mutable; strings
        and tuples are not.

    optional parameter
        A parameter written in a function header with an assignment to a
        default value which it will receive if no corresponding argument is
        given for it in the function call.
        
    short-circuit evaluation
        A style of programming that shortcuts extra work as soon as the 
        outcome is know with certainty. In this chapter our ``find`` 
        function returned as soon as it found what it was looking for; it
        didn't traverse all the rest of the items in the string.

    slice
        A part of a string (substring) specified by a range of indices. More
        generally, a subsequence of any sequence type in Python can be created
        using the slice operator (``sequence[start:stop]``).

    traverse
        To iterate through the elements of a collection, performing a similar
        operation on each.

    whitespace
        Any of the characters that move the cursor without printing visible
        characters. The constant ``string.whitespace`` contains all the
        white-space characters.


Exercises
---------


#. What is the result of each of the following:

    .. sourcecode:: python3
    
        >>> "Python"[1]
        >>> "Strings are sequences of characters."[5]
        >>> len("wonderful")
        >>> "Mystery"[:4]
        >>> "p" in "Pineapple"
        >>> "apple" in "Pineapple"
        >>> "pear" not in "Pineapple"
        >>> "apple" > "pineapple"
        >>> "pineapple" < "Peach"
    
#. Modify:

       .. sourcecode:: python3
           :linenos:
        
           prefixes = "JKLMNOPQ"
           suffix = "ack"
           
           for letter in prefixes:
               print(letter + suffix)

   so that ``Ouack`` and ``Quack`` are spelled correctly.
   
#. Encapsulate

       .. sourcecode:: python3
           :linenos:
        
           word = "banana"
           count = 0
           for letter in word:
               if letter == "a":
                   count += 1
           print(count)

   in a function named ``count_letters``, and generalize it so that it accepts
   the string and the letter as arguments.  Make the function return the number
   of characters, rather than print the answer.  The caller should do the printing.
     
#. Now rewrite the ``count_letters`` function so that instead of traversing the 
   string, it repeatedly calls the ``find`` method, with the optional third parameter 
   to locate new occurrences of the letter being counted.
   
#. Assign to a variable in your program a triple-quoted string that contains 
   your favourite paragraph of text --- perhaps a poem, a speech, instructions
   to bake a cake, some inspirational verses, etc.

   Write a function which removes all punctuation from the string, breaks the string
   into a list of words, and counts the number of words in your text that contain
   the letter "e".  Your program should print an analysis of the text like this:
   
       .. sourcecode:: pycon

           Your text contains 243 words, of which 109 (44.8%) contain an "e".      

#. Print a neat looking multiplication table like this:

       .. sourcecode:: pycon
       
                  1   2   3   4   5   6   7   8   9  10  11  12
            :--------------------------------------------------
           1:     1   2   3   4   5   6   7   8   9  10  11  12
           2:     2   4   6   8  10  12  14  16  18  20  22  24
           3:     3   6   9  12  15  18  21  24  27  30  33  36
           4:     4   8  12  16  20  24  28  32  36  40  44  48
           5:     5  10  15  20  25  30  35  40  45  50  55  60
           6:     6  12  18  24  30  36  42  48  54  60  66  72
           7:     7  14  21  28  35  42  49  56  63  70  77  84
           8:     8  16  24  32  40  48  56  64  72  80  88  96
           9:     9  18  27  36  45  54  63  72  81  90  99 108
          10:    10  20  30  40  50  60  70  80  90 100 110 120
          11:    11  22  33  44  55  66  77  88  99 110 121 132
          12:    12  24  36  48  60  72  84  96 108 120 132 144

#. Write a function that reverses its string argument, and satisfies these tests:

       .. sourcecode:: python3
           :linenos:
           
           reverse("happy") == "yppah"
           reverse("Python") == "nohtyP"
           reverse("") == ""
           reverse("a") == "a"
   
#. Write a function that mirrors its argument:

       .. sourcecode:: python3
           :linenos:
          
           mirror("good") == "gooddoog"
           mirror("Python") == "PythonnohtyP"
           mirror("") == ""
           mirror("a") == "aa"

#. Write a function that removes all occurrences of a given letter from a string:
    
        .. sourcecode:: python3
            :linenos:   
            
            remove_letter("a", "apple") == "pple"
            remove_letter("a", "banana") == "bnn"
            remove_letter("z", "banana") == "banana"
            remove_letter("i", "Mississippi") == "Msssspp"
            remove_letter("b", "") = ""
            remove_letter("b", "c") = "c"

#. Write a function that recognizes palindromes. (Hint: use your ``reverse`` function to make this easy!):

        .. sourcecode:: python3
            :linenos:   
            
            is_palindrome("abba")
            not is_palindrome("abab")
            is_palindrome("tenet")
            not is_palindrome("banana")
            is_palindrome("straw warts")
            is_palindrome("a")
            # is_palindrome(""))    # Is an empty string a palindrome?

#. Write a function that counts how many times a substring occurs in a string: 
   
        .. sourcecode:: python3
            :linenos: 
            
            count("is", "Mississippi") == 2
            count("an", "banana") == 2
            count("ana", "banana") == 2
            count("nana", "banana") == 1
            count("nanan", "banana") == 0
            count("aaa", "aaaaaa") == 4
   
#. Write a function that removes the first occurrence of a string from another string: 

        .. sourcecode:: python3
            :linenos: 
            
            remove("an", "banana") == "bana"
            remove("cyc", "bicycle") == "bile"
            remove("iss", "Mississippi") == "Missippi"
            remove("eggs", "bicycle") == "bicycle"
 
#. Write a function that removes all occurrences of a string from another string: 

        .. sourcecode:: python3
            :linenos: 
            
            remove_all("an", "banana") == "ba"
            remove_all("cyc", "bicycle") == "bile"
            remove_all("iss", "Mississippi") == "Mippi"
            remove_all("eggs", "bicycle") == "bicycle"

..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".




There are only four *really* important operations on strings, and we'll be able to do
just about anything.  There are many more nice-to-have methods 
(we'll call them sugar coating) 
that can make life easier, but if we can work with the basic four operations 
smoothly, we'll have a great grounding.

* len(str)  finds the length of a string.
* str[i]    the subscript operation extracts the i'th character of the string, as a new string.
* str[i:j]  the slice operation extracts a substring out of a string.
* str.find(target) returns the index where target occurs within the string, or -1 if it is not found.

So if we need to know if "snake" occurs as a substring within ``s``, we could write

    .. sourcecode:: python3
       :linenos:
       
       if s.find("snake") >= 0:  ...
       if "snake" in s: ...           # Also works, nice-to-know sugar coating!
   
It would be wrong to split the string into words unless we were asked whether the *word* "snake"
occurred in the string.  

Suppose we're asked to read some lines of data and find function definitions, e.g.: ``def some_function_name(x, y):``, 
and we are further asked to isolate and work with the name of the function. (Let's say, print it.)

    .. sourcecode:: python3
       :linenos:
       
       s = "..."                         # Get the next line from somewhere 
       def_pos = s.find("def ")          # Look for "def " in the line
       if def_pos == 0:                  # If it occurs at the left margin 
         op_index = s.find("(")          # Find the index of the open parenthesis
         fnname = s[4:op_index]          # Slice out the function name
         print(fnname)                   # ... and work with it.
     
One can extend these ideas:  

* What if the function def was indented, and didn't start at column 0? 
  The code would need a bit of adjustment, and we'd probably want to be sure that
  all the characters in front of the ``def_pos`` position were spaces. We would not want to 
  do the wrong thing on data like this:  ``# I def initely like Python!``
* We've assumed on line 3 that we will find an open parenthesis.  It may need to
  be checked that we did! 
* We have also assumed that there was exactly one space between the keyword ``def`` and
  the start of the function name.  It will not work nicely for ``def       f(x)``
  
As we've already mentioned, there are many more "sugar-coated" methods that let us
work more easily with strings.  There is an ``rfind`` method, like ``find``, that searches from the 
end of the string backwards.  It is useful if we want to find the last occurrence of something.
The ``lower`` and ``upper`` methods can do case conversion.  And the ``split`` method is great for
breaking a string into a list of words, or into a list of lines.  We've also made extensive use
in this book of the ``format`` method. In fact, if we want to 
practice reading the Python documentation and learning some new methods on our own, the
string methods are an excellent resource. 


Exercises:

* Suppose any line of text can contain at most one url that starts with "http://"
  and ends at the next space in the line.  Write a fragment of code to 
  extract and print the full url if it is present.  (Hint: read the documentation
  for ``find``.  It takes some extra arguments, so you can set a starting point
  from which it will search.)
* Suppose a string contains at most one substring "< ... >".  Write a fragment of code to 
  extract and print the portion of the string between the angle brackets.   

  

 
|      
    
Tuples
######

.. index:: mutable, immutable, tuple

Tuples are used for grouping data
---------------------------------

We saw earlier that we could group together pairs of values by
surrounding with parentheses.  Recall this example: 

    .. sourcecode:: python3

        >>> year_born = ("Paris Hilton", 1981) 

This is an example of a **data structure** --- a mechanism for grouping and
organizing data to make it easier to use.
    
The pair is an example of a **tuple**. Generalizing this, a tuple can
be used to group any number of items into a single compound value.  
Syntactically, a tuple is a comma-separated sequence of values.  
Although it is not necessary, it is conventional to enclose tuples in parentheses:

    .. sourcecode:: python3
        
        >>> julia = ("Julia", "Roberts", 1967, "Duplicity", 2009, "Actress", "Atlanta, Georgia")

The other thing that could be said somewhere around here, is that the
parentheses are there to disambiguate. For example, if we have a
tuple nested within another tuple and the parentheses weren't there,
how would we tell where the nested tuple begins/ends?
Also: the creation of an empty tuple is done like this:
``empty_tuple=()``
    
Tuples are useful for representing what other languages often call *records* (or structs) ---
some related information that belongs together, like your student record.  There is
no description of what each of these fields means, but we can guess.  A tuple
lets us "chunk" together related information and use it as a single thing.
 
Tuples support the same sequence operations as strings. The index operator 
selects an element from a tuple.

    .. sourcecode:: python3
        
        >>> julia[2]
        1967

But if we try to use item assignment to modify one of the elements of the
tuple, we get an error:

    .. sourcecode:: python3
        
        >>> julia[0] = "X"
        TypeError: 'tuple' object does not support item assignment

So like strings, tuples are immutable.  Once Python has created a tuple
in memory, it cannot be changed.  

Of course, even if we can't modify the 
elements of a tuple, we can always make the ``julia`` variable reference
a new tuple holding different information.  To construct the new tuple,
it is convenient that we can slice parts of the old tuple and join up the
bits to make the new tuple.  So  if ``julia`` has a new recent film, we could 
change her variable to reference a new tuple that used some information 
from the old one:

    .. sourcecode:: python3
        
        >>> julia = julia[:3] + ("Eat Pray Love", 2010) + julia[5:]
        >>> julia
        ("Julia", "Roberts", 1967, "Eat Pray Love", 2010, "Actress", "Atlanta, Georgia")


To create a tuple with a single element (but you're probably not likely
to do that too often), we have to include the final comma, because without
the final comma, Python treats the ``(5)`` below as an integer in parentheses:

    .. sourcecode:: python3
        
        >>> tup = (5,)
        >>> type(tup)
        <class 'tuple'> 
        >>> x = (5)
        >>> type(x)
        <class 'int'>     
          
          
.. index::
    single: assignment; tuple 
    single: tuple; assignment  
  
Tuple assignment
----------------

Python has a very powerful **tuple assignment** feature that allows a tuple of variables 
on the left of an assignment to be assigned values from a tuple
on the right of the assignment.   (We already saw this used for pairs, but it generalizes.)

    .. sourcecode:: python3
        
        (name, surname, year_born, movie, year_movie, profession, birthplace) = julia
    
This does the equivalent of seven assignment statements, all on one easy line.  
One requirement is that the number of variables on the left must match the number
of elements in the tuple. 
 
One way to think of tuple assignment is as tuple packing/unpacking.

In tuple packing, the values on the left are 'packed' together in a
tuple:

    .. sourcecode:: python3

        >>> bob = ("Bob", 19, "CS")    # tuple packing

In tuple unpacking, the values in a tuple on the right are 'unpacked'
into the variables/names on the right:

    .. sourcecode:: python3

        >>> bob = ("Bob", 19, "CS")
        >>> (name, age, studies) = bob    # tuple unpacking
        >>> name
        'Bob'
        >>> age
        19
        >>> studies
        'CS'

Once in a while, it is useful to swap the values of two variables.  With
conventional assignment statements, we have to use a temporary variable. For
example, to swap ``a`` and ``b``:

    .. sourcecode:: python3
        :linenos:
        
        temp = a
        a = b
        b = temp

Tuple assignment solves this problem neatly:

    .. sourcecode:: python3
        :linenos:
        
        (a, b) = (b, a)

The left side is a tuple of variables; the right side is a tuple of values.
Each value is assigned to its respective variable. All the expressions on the
right side are evaluated before any of the assignments. This feature makes
tuple assignment quite versatile.

Naturally, the number of variables on the left and the number of values on the
right have to be the same:

    .. sourcecode:: python3
        
        >>> (one, two, three, four) = (1, 2, 3)
        ValueError: need more than 3 values to unpack 

.. index::
    single: tuple; return value 
    
.. index:: return a tuple

Tuples as return values
-----------------------

Functions can always only return a single value, but by making that value a tuple, 
we can effectively group together as many values 
as we like, and return them together.   This is very useful --- we often want to
know some batsman's highest and lowest score, or we want to find the mean and the standard 
deviation, or we want to know the year, the month, and the day, or if we're doing some
some ecological modelling we may want to know the number of rabbits and the number
of wolves on an island at a given time.  

For example, we could write a function that returns both the area and the circumference
of a circle of radius r:

    .. sourcecode:: python3
        :linenos:
        
        def circle_stats(r):
            """ Return (circumference, area) of a circle of radius r """
            circumference = 2 * math.pi * r
            area = math.pi * r * r
            return (circumference, area)
 
 
Composability of Data Structures
--------------------------------
    
We saw in an earlier chapter that we could make a list of pairs, and we had an example 
where one of the items in the tuple was itself a list: 

    .. sourcecode:: python3
    
        students = [
            ("John", ["CompSci", "Physics"]),
            ("Vusi", ["Maths", "CompSci", "Stats"]),
            ("Jess", ["CompSci", "Accounting", "Economics", "Management"]),
            ("Sarah", ["InfSys", "Accounting", "Economics", "CommLaw"]),
            ("Zuki", ["Sociology", "Economics", "Law", "Stats", "Music"])]

Tuples items can themselves be other tuples.  For example, we could improve
the information about our movie stars to hold the full date of birth rather
than just the year, and we could have a list of some of her movies and dates that they
were made, and so on:

    .. sourcecode:: python3

       julia_more_info = ( ("Julia", "Roberts"), (8, "October", 1967), 
                            "Actress", ("Atlanta", "Georgia"),  
                            [ ("Duplicity", 2009), 
                              ("Notting Hill", 1999),
                              ("Pretty Woman", 1990),
                              ("Erin Brockovich", 2000),
                              ("Eat Pray Love", 2010),
                              ("Mona Lisa Smile", 2003),
                              ("Oceans Twelve", 2004) ])
                          
Notice in this case that the tuple has just five elements --- but each of those in turn
can be another tuple, a list, a string, or any other kind of Python value.
This property is known as being **heterogeneous**, meaning that it can
be composed of elements of different types.

Glossary
--------

.. glossary::


    data structure
        An organization of data for the purpose of making it easier to use.

    immutable data value
        A data value which cannot be modified.  Assignments to elements or
        slices (sub-parts) of immutable values cause a runtime error.

    mutable data value
        A data value which can be modified. The types of all mutable values 
        are compound types.  Lists and dictionaries are mutable; strings
        and tuples are not.

    tuple
        An immutable data value that contains related elements. Tuples are used
        to group together related data, such as a person's name, their age, 
        and their gender.  

    tuple assignment
        An assignment to all of the elements in a tuple using a single
        assignment statement. Tuple assignment occurs *simultaneously* rather than
        in sequence, making it useful for swapping values.


Exercises
---------
   
#.  We've said nothing in this chapter about whether you can pass tuples as 
    arguments to a function. Construct a small Python example to test whether 
    this is possible, and write up your findings.
    
#.  Is a pair a generalization of a tuple, or is a tuple a generalization of a pair?

#.  Is a pair a kind of tuple, or is a tuple a kind of pair? 

   
..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".|    
    
.. index:: list, element, item, sequence, collection    
    
Lists
#####

A **list** is an ordered collection of values. The values that make up a list 
are called its **elements**, or its **items**. 
We will use the term `element` or `item` to mean the same thing. Lists are
similar to strings, which are ordered collections of characters, except that the
elements of a list can be of any type.  Lists and strings --- and other collections
that maintain the order of their items --- are called **sequences**.

.. index:: nested list, list; nested

List values
-----------

There are several ways to create a new list; the simplest is to enclose the
elements in square brackets (``[`` and ``]``):

    .. sourcecode:: python3
        :linenos:
        
        numbers = [10, 20, 30, 40]
        words = ["spam", "bungee", "swallow"]

The first example is a list of four integers. The second is a list of three
strings. The elements of a list don't have to be the same type.  The following
list contains a string, a float, an integer, and
(amazingly) another list:

    .. sourcecode:: python3
        :linenos:
        
        stuffs = ["hello", 2.0, 5, [10, 20]]

A list within another list is said to be **nested**.

Finally, a list with no elements is called an empty list,
and is denoted ``[]``.

We have already seen that we can assign list values to variables or pass lists as parameters to functions:

    .. sourcecode:: python3
        :linenos:
        
        >>> vocabulary = ["apple", "cheese", "dog"]
        >>> numbers = [17, 123]
        >>> an_empty_list = []
        >>> print(vocabulary, numbers, an_empty_list)
        ["apple", "cheese", "dog"] [17, 123] []

.. _accessing-elements:

.. index:: list index, index, list traversal

Accessing elements
------------------

The syntax for accessing the elements of a list is the same as the syntax for
accessing the characters of a string --- the index operator: ``[]`` (not to
be confused with an empty list). The expression inside the brackets specifies
the index. Remember that the indices start at 0:

    .. sourcecode:: python3
        
        >>> numbers[0]
        17

Any expression evaluating to an integer can be used as an index:

    .. sourcecode:: python3

        >>> numbers[9-8]
        123
        >>> numbers[1.0]
        Traceback (most recent call last):
          File "<interactive input>", line 1, in <module>
        TypeError: list indices must be integers, not float

If you try to access or assign to an element that does not exist, you get a runtime
error:

    .. sourcecode:: python3

        >>> numbers[2]
        Traceback (most recent call last):
          File "<interactive input>", line 1, in <module>
        IndexError: list index out of range

It is common (but wrong!) to use a loop variable as a list index.

    .. sourcecode:: python3
        :linenos:
        
        horsemen = ["war", "famine", "pestilence", "death"]

        for i in [0, 1, 2, 3]:
            print(horsemen[i])


Each time through the loop, the variable ``i`` is used as an index into the
list, printing the ``i``'th element. This pattern of computation is called a
**list traversal**.

The above sample doesn't need or use the index ``i`` for anything besides getting
the items from the list, so this more direct version --- where the ``for`` loop gets
the items --- is much more clear!

    .. sourcecode:: python3
        :linenos:
        
        horsemen = ["war", "famine", "pestilence", "death"]

        for h in horsemen:
            print(h)

        

List length
-----------

The function ``len`` returns the length of a list, which is equal to the number
of its elements. If you are going to use an integer index to access the list,
it is a good idea to use this value as the upper bound of a
loop instead of a constant. That way, if the size of the list changes, you
won't have to go through the program changing all the loops; they will work
correctly for any size list:
 
    .. sourcecode:: python3
        :linenos:
        
        horsemen = ["war", "famine", "pestilence", "death"]
        
        for i in range(len(horsemen)):
            print(horsemen[i])


The last time the body of the loop is executed, ``i`` is ``len(horsemen) - 1``, 
which is the index of the last element. (But the version without the index
looks even better now! The version above is not the right way to do things!)

    .. sourcecode:: python3
        :linenos:
        
        horsemen = ["war", "famine", "pestilence", "death"]
        
        for horseman in horsemen:
            print horseman

Although a list can contain another list, the nested list still counts as a
single element in its parent list. The length of this list is 4:

    .. sourcecode:: python3
        
        >>> len(["car makers", 1, ["Ford", "Toyota", "BMW"], [1, 2, 3]])
        4


List membership
---------------

``in`` and ``not in`` are Boolean operators that test membership in a sequence. We
used them previously with strings, but they also work with lists and
other sequences:

    .. sourcecode:: python3
        
        >>> horsemen = ["war", "famine", "pestilence", "death"]
        >>> "pestilence" in horsemen
        True
        >>> "debauchery" in horsemen
        False
        >>> "debauchery" not in horsemen
        True
    
Using this produces a more elegant version of the nested loop program we previously used 
to count the number of students doing Computer Science
in the section :ref:`nested_data`:  

    .. sourcecode:: python3
        :linenos:
        
        students = [
            ("John", ["CompSci", "Physics"]),
            ("Vusi", ["Maths", "CompSci", "Stats"]),
            ("Jess", ["CompSci", "Accounting", "Economics", "Management"]),
            ("Sarah", ["InfSys", "Accounting", "Economics", "CommLaw"]),
            ("Zuki", ["Sociology", "Economics", "Law", "Stats", "Music"])]
                
        # Count how many students are taking CompSci
        counter = 0
        for name, subjects in students:
            if "CompSci" in subjects:            
                   counter += 1
                   
        print("The number of students taking CompSci is", counter)


List operations
---------------

The ``+`` operator concatenates lists:

    .. sourcecode:: python3
        
        >>> first_list = [1, 2, 3]
        >>> second_list = [4, 5, 6]
        >>> both_lists = first_list + second_list
        >>> both_lists
        [1, 2, 3, 4, 5, 6]

Similarly, the ``*`` operator repeats a list a given number of times:

    .. sourcecode:: python3
        
        >>> [0] * 4
        [0, 0, 0, 0]
        >>> [1, 2, 3] * 3
        [1, 2, 3, 1, 2, 3, 1, 2, 3]

The first example repeats ``[0]`` four times. The second example repeats the
list ``[1, 2, 3]`` three times.


.. index:: slice, sublist

List slices
-----------

The slice operations we saw previously with strings let us work with sublists:

    .. sourcecode:: python3
        
        >>> a_list = ["a", "b", "c", "d", "e", "f"]
        >>> a_list[1:3]
        ['b', 'c']
        >>> a_list[:4]
        ['a', 'b', 'c', 'd']
        >>> a_list[3:]
        ['d', 'e', 'f']
        >>> a_list[:]
        ['a', 'b', 'c', 'd', 'e', 'f']

.. index:: mutable, item assignment, immutable
    
Lists are mutable
-----------------

Unlike strings, lists are **mutable**, which means we can change their
elements. Using the index operator on the left side of an assignment, we can
update one of the elements:

    .. sourcecode:: python3
        
        >>> fruit = ["banana", "apple", "quince"]
        >>> fruit[0] = "pear"
        >>> fruit[2] = "orange"
        >>> fruit
        ['pear', 'apple', 'orange']

The bracket operator applied to a list can appear anywhere in an expression.
When it appears on the left side of an assignment, it changes one of the
elements in the list, so the first element of ``fruit`` has been changed from
``"banana"`` to ``"pear"``, and the last from ``"quince"`` to ``"orange"``. An
assignment to an element of a list is called **item assignment**. Item
assignment does not work for strings:

    .. sourcecode:: python3
        
        >>> my_string = "TEST"
        >>> my_string[2] = "X"
        Traceback (most recent call last):
          File "<interactive input>", line 1, in <module>
        TypeError: 'str' object does not support item assignment

but it does for lists:

    .. sourcecode:: python3
        
        >>> my_list = ["T", "E", "S", "T"]
        >>> my_list[2] = "X"
        >>> my_list
        ['T', 'E', 'X', 'T']


With the slice operator we can update a whole sublist at once:

    .. sourcecode:: python3
        
        >>> a_list = ["a", "b", "c", "d", "e", "f"]
        >>> a_list[1:3] = ["x", "y"]
        >>> a_list
        ['a', 'x', 'y', 'd', 'e', 'f']

We can also remove elements from a list by assigning an empty list to them:

    .. sourcecode:: python3
        
        >>> a_list = ["a", "b", "c", "d", "e", "f"]
        >>> a_list[1:3] = []
        >>> a_list
        ['a', 'd', 'e', 'f']

And we can add elements to a list by squeezing them into an empty slice at the
desired location:

    .. sourcecode:: python3
        
        >>> a_list = ["a", "d", "f"]
        >>> a_list[1:1] = ["b", "c"]
        >>> a_list
        ['a', 'b', 'c', 'd', 'f']
        >>> a_list[4:4] = ["e"]
        >>> a_list
        ['a', 'b', 'c', 'd', 'e', 'f']

.. index:: del statement, statement; del

List deletion
-------------

Using slices to delete list elements can be error-prone.
Python provides an alternative that is more readable.
The ``del`` statement removes an element from a list:

    .. sourcecode:: python3
        
        >>> a = ["one", "two", "three"]
        >>> del a[1]
        >>> a
        ['one', 'three']

As you might expect, ``del`` causes a runtime
error if the index is out of range.

You can also use ``del`` with a slice to delete a sublist:

    .. sourcecode:: python3
        
        >>> a_list = ["a", "b", "c", "d", "e", "f"]
        >>> del a_list[1:5]
        >>> a_list
        ['a', 'f']

As usual, the sublist selected by slice contains all the elements up to, but not including, the second
index.

.. index:: is operator, objects and values

Objects and references
----------------------

After we execute these assignment statements

    .. sourcecode:: python3
        :linenos:
        
        a = "banana"
        b = "banana"

we know that ``a`` and ``b`` will refer to a string object with the letters
``"banana"``. But we don't know yet whether they point to the *same* string object.

There are two possible ways the Python interpreter could arrange its memory:

    .. image:: illustrations/list1.png
       :alt: List illustration 

In one case, ``a`` and ``b`` refer to two different objects that have the same
value. In the second case, they refer to the same object. 

We can test whether two names refer to the same object using the ``is``
operator: 

    .. sourcecode:: python3

        >>> a is b
        True

This tells us that both ``a`` and ``b`` refer to the same object, and that it
is the second of the two state snapshots that accurately describes the relationship. 

Since strings are *immutable*, Python optimizes resources by making two names
that refer to the same string value refer to the same object.

This is not the case with lists:

    .. sourcecode:: python3
        
        >>> a = [1, 2, 3]
        >>> b = [1, 2, 3]
        >>> a == b
        True
        >>> a is b
        False   

The state snapshot here looks like this:

    .. image:: illustrations/mult_references2.png
       :alt: State snapshot for equal different lists 

``a`` and ``b`` have the same value but do not refer to the same object.

.. index:: aliases

Aliasing
--------

Since variables refer to objects, if we assign one variable to another, both
variables refer to the same object:

    .. sourcecode:: python3
        
        >>> a = [1, 2, 3]
        >>> b = a
        >>> a is b
        True
    
In this case, the state snapshot looks like this:

    .. image:: illustrations/mult_references3.png
       :alt: State snapshot for multiple references (aliases) to a list 

Because the same list has two different names, ``a`` and ``b``, we say that it
is **aliased**. Changes made with one alias affect the other:

    .. sourcecode:: python3
        
        >>> b[0] = 5
        >>> a
        [5, 2, 3]

Although this behavior can be useful, it is sometimes unexpected or
undesirable. In general, it is safer to avoid aliasing when you are working
with mutable objects (i.e. lists at this point in our textbook, 
but we'll meet more mutable objects
as we cover classes and objects, dictionaries and sets). 
Of course, for immutable objects (i.e. strings, tuples), there's no problem --- it is
just not possible to change something and get a surprise when you access an alias name.
That's why Python is free to alias strings (and any other immutable kinds of data)
when it sees an opportunity to economize.

.. index:: clone

Cloning lists
-------------

If we want to modify a list and also keep a copy of the original, we need to be
able to make a copy of the list itself, not just the reference. This process is
sometimes called **cloning**, to avoid the ambiguity of the word copy.

The easiest way to clone a list is to use the slice operator:

    .. sourcecode:: python3
        
        >>> a = [1, 2, 3]
        >>> b = a[:]
        >>> b
        [1, 2, 3]

Taking any slice of ``a`` creates a new list. In this case the slice happens to
consist of the whole list.  So now the relationship is like this:

    .. image:: illustrations/mult_references2.png
       :alt: State snapshot for equal different lists 

Now we are free to make changes to ``b`` without worrying that we'll inadvertently be
changing ``a``:

    .. sourcecode:: python3
        
        >>> b[0] = 5
        >>> a
        [1, 2, 3]


.. index:: for loop, enumerate

Lists and ``for`` loops
-----------------------

The ``for`` loop also works with lists, as we've already seen. The generalized syntax of a ``for``
loop is:

    .. sourcecode:: python3
        
        for <VARIABLE> in <LIST>:
            <BODY>

So, as we've seen
        
    .. sourcecode:: python3
        :linenos:

        friends = ["Joe", "Zoe", "Brad", "Angelina", "Zuki", "Thandi", "Paris"]
        for friend in friends:
            print(friend)

It almost reads like English: For (every) friend in (the list of) friends,
print (the name of the) friend.

Any list expression can be used in a ``for`` loop:

    .. sourcecode:: python3
        :linenos:
        
        for number in range(20):
            if number % 3 == 0:
                print(number)
           
        for fruit in ["banana", "apple", "quince"]:
            print("I like to eat " + fruit + "s!")


The first example prints all the multiples of 3 between 0 and 19. The second
example expresses enthusiasm for various fruits.

Since lists are mutable, we often want to traverse a list, changing
each of its elements. The following squares all the numbers in the list ``xs``:

    .. sourcecode:: python3
        :linenos:

        xs = [1, 2, 3, 4, 5]
        
        for i in range(len(xs)):
            xs[i] = xs[i]**2

Take a moment to think about ``range(len(xs))`` until you understand how
it works. 

In this example we are interested in both the *value* of an item, (we want to 
square that value), and its *index* (so that we can assign the new value to that position).
This pattern is common enough that Python provides a nicer way to implement it:

    .. sourcecode:: python3
        :linenos:
        
        xs = [1, 2, 3, 4, 5]
        
        for (i, val) in enumerate(xs):
            xs[i] = val**2

``enumerate`` generates pairs of both (index, value) during
the list traversal. Try this next example to see more clearly how ``enumerate``
works:

    .. sourcecode:: python3
        :linenos:
        
        for (i, v) in enumerate(["banana", "apple", "pear", "lemon"]):
             print(i, v)
    
    .. sourcecode:: pycon
  
        0 banana
        1 apple
        2 pear
        3 lemon


.. index:: parameter

List parameters
---------------

Passing a list as an argument actually passes a reference to the list, not a
copy or clone of the list. So parameter passing creates an alias for you: the caller
has one variable referencing the list, and the called function has an alias, but there
is only one underlying list object.
For example, the function below takes a list as an
argument and multiplies each element in the list by 2:

    .. sourcecode:: python3
        :linenos:
        
        def double_stuff(stuff_list):
            """ Overwrite each element in a_list with double its value. """
            for (index, stuff) in enumerate(stuff_list):
                stuff_list[index] = 2 * stuff

If we add the following onto our script:

    .. sourcecode:: python3
        :linenos:
        
        things = [2, 5, 9]
        double_stuff(things)
        print(things)
    
When we run it we'll get:

    .. sourcecode:: pycon

        [4, 10, 18]


In the function above, the parameter 
``stuff_list`` and the variable ``things`` are aliases for the
same object.  So before any changes to the elements in the list, the state snapshot
looks like this:

    .. image:: illustrations/mult_references4.png
       :alt: State snapshot for multiple references to a list as a parameter
   
Since the list object is shared by two frames, we drew it between them.

If a function modifies the items of a list parameter, the caller sees the change.

.. index:: list; append
    
List methods
------------

The dot operator can also be used to access built-in methods of list objects.  We'll
start with the most useful method for adding something onto the end of an existing list:

    .. sourcecode:: python3
        
        >>> mylist = []
        >>> mylist.append(5)
        >>> mylist.append(27)
        >>> mylist.append(3)
        >>> mylist.append(12)
        >>> mylist
        [5, 27, 3, 12]

``append`` is a list method which adds the argument passed to it to the end of
the list. We'll use it heavily when we're creating new lists.
Continuing with this example, we show several other list methods:

    .. sourcecode:: python3
        
        >>> mylist.insert(1, 12)  # Insert 12 at pos 1, shift other items up
        >>> mylist
        [5, 12, 27, 3, 12]
        >>> mylist.count(12)       # How many times is 12 in mylist?
        2
        >>> mylist.extend([5, 9, 5, 11])   # Put whole list onto end of mylist
        >>> mylist
        [5, 12, 27, 3, 12, 5, 9, 5, 11])
        >>> mylist.index(9)                # Find index of first 9 in mylist
        6
        >>> mylist.reverse()
        >>> mylist
        [11, 5, 9, 5, 12, 3, 27, 12, 5]
        >>> mylist.sort()
        >>> mylist
        [3, 5, 5, 5, 9, 11, 12, 12, 27]   
        >>> mylist.remove(12)             # Remove the first 12 in the list
        >>> mylist
        [3, 5, 5, 5, 9, 11, 12, 27]

Experiment and play with the list methods shown here, and read their documentation until 
you feel confident that you understand how they work.

.. index:: side effect, modifier

.. _pure-func-mod:

Pure functions and modifiers
----------------------------
As seen before, there is a difference between a pure function and one with side-effects. The difference is shown below as lists have some special gotcha's.
Functions which take lists as arguments and change them during execution are
called **modifiers** and the changes they make are called **side effects**.

A **pure function** does not produce side effects. It communicates with the
calling program only through parameters, which it does not modify, and a return
value. Here is ``double_stuff`` written as a pure function:

    .. sourcecode:: python3
        :linenos:
        
        def double_stuff(a_list):
            """ Return a new list which contains 
                doubles of the elements in a_list. 
            """
            new_list = []
            for value in a_list:
                new_elem = 2 * value
                new_list.append(new_elem)
                
            return new_list
      
This version of ``double_stuff`` does not change its arguments:

    .. sourcecode:: python3
        
        >>> things = [2, 5, 9]
        >>> more_things = double_stuff(things)
        >>> things
        [2, 5, 9]
        >>> more_things
        [4, 10, 18]
   
An early rule we saw for assignment said "first evaluate the right hand side, then
assign the resulting value to the variable".  So it is quite safe to assign the function
result to the same variable that was passed to the function:

    .. sourcecode:: python3

        >>> things = [2, 5, 9]
        >>> things = double_stuff(things)
        >>> things
        [4, 10, 18]      

Functions that produce lists
----------------------------

The pure version of ``double_stuff`` above made use of an 
important **pattern** for your toolbox. Whenever you need to
write a function that creates and returns a list, the pattern is
usually:

    .. sourcecode:: python3
        :linenos:
        
        initialize a result variable to be an empty list
        loop
           create a new element 
           append it to result
        return the result

Let us show another use of this pattern.  Assume you already have a function
``is_prime(x)`` that can test if x is prime.  Write a function
to return a list of all prime numbers less than n:

    .. sourcecode:: python3
       :linenos:

       def primes_lessthan(n):
           """ Return a list of all prime numbers less than n. """
           result = []
           for i in range(2, n):
               if is_prime(i):
                  result.append(i)
           return result

.. index:: strings and lists, split, join

Strings and lists
-----------------

Two of the most useful methods on strings involve conversion to
and from lists of substrings.  
The ``split`` method (which we've already seen)
breaks a string into a list of words.  By
default, any number of whitespace characters is considered a word boundary:

    .. sourcecode:: python3
        
        >>> song = "The rain in Spain..."
        >>> words = song.split()
        >>> words
        ['The', 'rain', 'in', 'Spain...']

An optional argument called a **delimiter** can be used to specify which
string to use as the boundary marker between substrings. 
The following example uses the string ``ai`` as the delimiter:

    .. sourcecode:: python3
        
        >>> song.split("ai")
        ['The r', 'n in Sp', 'n...']

Notice that the delimiter doesn't appear in the result.

The inverse of the ``split`` method is ``join``.  You choose a
desired **separator** string, (often called the *glue*) 
and join the list with the glue between each of the elements: 

    .. sourcecode:: python3

        >>> glue = ";"
        >>> phrase = glue.join(words)
        >>> phrase
        'The;rain;in;Spain...'

The list that you glue together (``words`` in this example) is not modified.  Also, as these
next examples show, you can use empty glue or multi-character strings as glue:

    .. sourcecode:: python3

        >>> " --- ".join(words)
        'The --- rain --- in --- Spain...'
        >>> "".join(words)
        'TheraininSpain...'

.. index:: promise, range function
    
``list`` and ``range``
----------------------   
    
Python has a built-in type conversion function called 
``list`` that tries to turn whatever you give it
into a list.  

    .. sourcecode:: python3
        
        >>> letters = list("Crunchy Frog")
        >>> letters
        ["C", "r", "u", "n", "c", "h", "y", " ", "F", "r", "o", "g"]
        >>> "".join(letters)
        'Crunchy Frog'
    
One particular feature of ``range`` is that it 
doesn't instantly compute all its values: it "puts off" the computation,
and does it on demand, or "lazily".  We'll say that it gives a **promise**
to produce the values when they are needed.   This is very convenient if your
computation short-circuits a search and returns early, as in this case: 

    .. sourcecode:: python3
        :linenos:

        def f(n):
            """ Find the first positive integer between 101 and less 
                than n that is divisible by 21 
            """
            for i in range(101, n):
               if (i % 21 == 0):
                   return i
                    
                    
        print(f(110) == 105)
        print(f(1000000000) == 105)

In the second test, if range were to eagerly go about building a list 
with all those elements, you would soon exhaust your computer's available
memory and crash the program.  But it is cleverer than that!  This computation works
just fine, because the ``range`` object is just a promise to produce the elements
if and when they are needed.  Once the condition in the ``if`` becomes true, no
further elements are generated, and the function returns.  (Note: Before Python 3,
``range`` was not lazy. If you use an earlier versions of Python, YMMV!)

    .. admonition:: YMMV: Your Mileage May Vary

        The acronym YMMV stands for *your mileage may vary*.  American car advertisements
        often quoted fuel consumption figures for cars, e.g. that they would get 28 miles per
        gallon.  But this always had to be accompanied by legal small-print
        warning the reader that they might not get the same.  The term YMMV is now used
        idiomatically to mean "your results may differ", 
        e.g. *The battery life on this phone is 3 days, but YMMV.*     
    
You'll sometimes find the lazy ``range`` wrapped in a call to ``list``.  This forces
Python to turn the lazy promise into an actual list: 

    .. sourcecode:: python3

        >>> range(10)           # Create a lazy promise 
        range(0, 10)
        >>> list(range(10))     # Call in the promise, to produce a list.
        [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
 
.. index:: nested list, list; nested
    
Looping and lists
-----------------

Computers are useful because they can repeat computation, accurately and fast.
So loops are going to be a central feature of almost all programs you encounter.

.. admonition:: Tip: Don't create unnecessary lists
   
   Lists are useful if you need to keep data for later computation.  But if you
   don't need lists, it is probably better not to generate them.
   
Here are two functions that both generate ten million random numbers, and return
the sum of the numbers.  They both work. 

    .. sourcecode:: python3
        :linenos:

        import random
        joe = random.Random()
        
        def sum1():
           """ Build a list of random numbers, then sum them """
           xs = []
           for i in range(10000000):
               num = joe.randrange(1000)  # Generate one random number
               xs.append(num)             # Save it in our list
               
           tot = sum(xs)
           return tot     
           
        def sum2():
           """ Sum the random numbers as we generate them """
           tot = 0
           for i in range(10000000):
               num = joe.randrange(1000)
               tot += num
           return tot
           
        print(sum1())
        print(sum2())
    
What reasons are there for preferring the second version here? 
(Hint: open a tool like the Performance Monitor on your computer, and watch the memory
usage. How big can you make the list before you get a fatal memory error in ``sum1``?)

In a similar way, when working with files, we often have an option to read the whole file 
contents into a single string, or we can read one line at a time and process
each line as we read it. Line-at-a-time is the more traditional and perhaps
safer way to do things --- you'll be able to work comfortably no matter how
large the file is. (And, of course, this mode of processing the files was 
essential in the old days when computer memories were much smaller.) 
But you may find whole-file-at-once is sometimes more convenient! 

   


   
Nested lists
------------

A nested list is a list that appears as an element in another list. In this
list, the element with index 3 is a nested list:

    .. sourcecode:: python3
        
        >>> nested = ["hello", 2.0, 5, [10, 20]]

If we output the element at index 3, we get:

    .. sourcecode:: python3

       >>> print(nested[3]) 
       [10, 20]

To extract an element from the nested list, we can proceed in two steps:

    .. sourcecode:: python3
        
        >>> elem = nested[3]
        >>> elem[0]
        10

Or we can combine them:

    .. sourcecode:: python3
        
        >>> nested[3][1]
        20

Bracket operators evaluate from left to right, so this expression gets the
3'th element of ``nested`` and extracts the 1'th element from it.

.. index:: matrix

Matrices
--------

Nested lists are often used to represent matrices. For example, the matrix:

    .. image:: illustrations/matrix2.png

might be represented as:

    .. sourcecode:: python3
        
        >>> mx = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

``mx`` is a list with three elements, where each element is a row of the
matrix. We can select an entire row from the matrix in the usual way:

    .. sourcecode:: python3
        
        >>> mx[1]
        [4, 5, 6]

Or we can extract a single element from the matrix using the double-index form:

    .. sourcecode:: python3
        
        >>> mx[1][2]
        6

The first index selects the row, and the second index selects the column.
Although this way of representing matrices is common, it is not the only
possibility. A small variation is to use a list of columns instead of a list of
rows. Later we will see a more radical alternative using a dictionary.

Glossary
--------

.. glossary::


    aliases
        Multiple variables that contain references to the same object.

    clone
        To create a new object that has the same value as an existing object.
        Copying a reference to an object creates an alias but doesn't clone the
        object.

    delimiter
        A character or string used to indicate where a string should be split.

    element
        One of the values in a list (or other sequence). The bracket operator
        selects elements of a list.  Also called *item*.

    immutable data value
        A data value which cannot be modified.  Assignments to elements or
        slices (sub-parts) of immutable values cause a runtime error.

    index
        An integer value that indicates the position of an item in a list.
        Indexes start from 0. 
        
    item
        See *element*.

    list
        A collection of values, each in a fixed position within the list.
        Like other types ``str``, ``int``, ``float``, etc. there is also a
        ``list`` type-converter function that tries to turn whatever argument 
        you give it into a list. 

    list traversal
        The sequential accessing of each element in a list.

    modifier
        A function which changes its arguments inside the function body. Only
        mutable types can be changed by modifiers.
        
    mutable data value
        A data value which can be modified. The types of all mutable values 
        are compound types.  Lists and dictionaries are mutable; strings
        and tuples are not.

    nested list
        A list that is an element of another list.

    object
        A thing to which a variable can refer.
        
    pattern
        A sequence of statements, or a style of coding something that has
        general applicability in a number of different situations.  Part of
        becoming a mature Computer Scientist is to learn and establish the
        patterns and algorithms that form your toolkit.  Patterns often 
        correspond to your "mental chunking".   

    promise
        An object that promises to do some work or deliver some values if
        they're eventually needed, but it lazily puts off doing the work immediately.
        Calling ``range`` produces a promise.         

    pure function
        A function which has no side effects. Pure functions only make changes
        to the calling program through their return values.

    sequence
        Any of the data types that consist of an ordered collection of elements, with
        each element identified by an index.
        
    side effect
        A change in the state of a program made by calling a function. Side
        effects can only be produced by modifiers.

    step size
        The interval between successive elements of a linear sequence. The
        third (and optional argument) to the ``range`` function is called the
        step size.  If not specified, it defaults to 1.

        
Exercises
---------


#. What is the Python interpreter's response to the following?

       .. sourcecode:: python3
        
           >>> list(range(10, 0, -2))

   The three arguments to the *range* function are *start*, *stop*, and *step*, 
   respectively. In this example, ``start`` is greater than ``stop``.  What
   happens if ``start < stop`` and ``step < 0``? Write a rule for the
   relationships among ``start``, ``stop``, and ``step``.
   
#. Consider this fragment of code: 


       .. sourcecode:: python3
            :linenos:
            
            import turtle
            
            tess = turtle.Turtle()
            alex = tess
            alex.color("hotpink")
   
   Does this fragment create one or two turtle instances?  Does setting
   the color of ``alex`` also change the color of ``tess``?  Explain in detail.
   
#. Draw a state snapshot for ``a`` and ``b`` before and after the third line of
   the following Python code is executed:

       .. sourcecode:: python3
            :linenos:
        
            a = [1, 2, 3]
            b = a[:]
            b[0] = 5

#. What will be the output of the following program?

       .. sourcecode:: python3
           :linenos:
        
           this = ["I", "am", "not", "a", "crook"]
           that = ["I", "am", "not", "a", "crook"]
           print("Test 1: {0}".format(this is that))
           that = this
           print("Test 2: {0}".format(this is that))

   Provide a *detailed* explanation of the results.
     
#. Lists can be used to represent mathematical *vectors*.  In this exercise
   and several that follow you will write functions to perform standard
   operations on vectors.  Create a script named ``vectors.py`` and 
   write Python code to pass the tests in each case.

   Write a function ``add_vectors(vector1,vector2)`` that takes two lists of numbers of
   the same length, and returns a new list containing the sums of the
   corresponding elements of each:
   
        .. sourcecode:: python3
           :linenos:
            
           add_vectors([1, 1], [1, 1]) == [2, 2]
           add_vectors([1, 2], [1, 4]) == [2, 6]
           add_vectors([1, 2, 1], [1, 4, 3]) == [2, 6, 4]
 
#. Write a function ``scalar_mult(scalar, vector)`` that takes a number, ``scalar``, and a
   list, ``vector`` and returns the `scalar multiple
   <http://en.wikipedia.org/wiki/Scalar_multiple>`__ of ``vector`` by ``scalar``. : 

        .. sourcecode:: python3
            :linenos:
            
            scalar_mult(5, [1, 2]) == [5, 10]
            scalar_mult(3, [1, 0, -1]) == [3, 0, -3]
            scalar_mult(7, [3, 0, 5, 11, 2]) == [21, 0, 35, 77, 14]

#. Write a function ``dot_product(vec1,vec2)`` that takes two lists of numbers of
   the same length, and returns the sum of the products of the corresponding
   elements of each (the `dot_product
   <http://en.wikipedia.org/wiki/Dot_product>`__).

       .. sourcecode:: python3
            :linenos:
        
            dot_product([1, 1], [1, 1]) ==  2
            dot_product([1, 2], [1, 4]) ==  9
            dot_product([1, 2, 1], [1, 4, 3]) == 12
      
#. *Extra challenge for the mathematically inclined*: Write a function
   ``cross_product(vec1, vec2)`` that takes two lists of numbers of length 3 and
   returns their
   `cross product <http://en.wikipedia.org/wiki/Cross_product>`__.  You should
   write your own tests.       
             
#. Describe the relationship between ``" ".join(song.split())`` and
   ``song`` in the fragment of code below. 
   Are they the same for all strings assigned to ``song``? 
   When would they be different? 
   
       .. sourcecode:: python3
            :linenos:

            song = "The rain in Spain..."
   
#. Write a function ``replace(s, old, new)`` that replaces all occurrences of
   ``old`` with ``new`` in a string ``s``: 
   
        .. sourcecode:: python3
            :linenos:

            replace("Mississippi", "i", "I") == "MIssIssIppI"
          
            song = "I love spom! Spom is my favorite food. Spom, spom, yum!"
            replace(song, "om", "am") ==
                "I love spam! Spam is my favorite food. Spam, spam, yum!"
        
            replace(song, "o", "a") ==
                "I lave spam! Spam is my favarite faad. Spam, spam, yum!"

   *Hint*: use the ``split`` and ``join`` methods.
          
..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".


|          
    
Dictionaries
############

.. index:: dictionary, mapping type, key, value, key:value pair

.. Key uniqueness isn't talked about in this chapter. The only place its
   mentioned is in the glossary for the term key.

All of the compound data types we have studied in detail so far --- strings,
lists, and tuples --- are sequence types, which use integers as indices to access
the values they contain within them.

**Dictionaries** are yet another kind of compound type. They are Python's
built-in **mapping type**. They map **keys**, which can be any immutable type,
to values, which can be any type (heterogeneous), just like the elements
of a list or tuple. In other languages, they are called associative
arrays since they associate a key with a value.

As an example, we will create a dictionary to translate English words into
Spanish. For this dictionary, the keys are strings.

One way to create a dictionary is to start with the empty dictionary and add
**key:value pairs**. The empty dictionary is denoted ``{}``:

    .. sourcecode:: python3
        
        >>> english_spanish = {}
        >>> english_spanish["one"] = "uno"
        >>> english_spanish["two"] = "dos"

The first assignment creates a dictionary named ``english_spanish``; the other
assignments add new key:value pairs to the dictionary. We can print the current
value of the dictionary in the usual way:

    .. sourcecode:: python3
        
        >>> print(english_spanish)
        {"two": "dos", "one": "uno"}

The key:value pairs of the dictionary are separated by commas. Each pair
contains a key and a value separated by a colon.

.. admonition:: Hashing

    The order of the pairs may not be what was expected. Python uses
    complex algorithms, designed for very fast access, to determine
    where the key:value pairs are stored in a dictionary. For our
    purposes we can think of this ordering as unpredictable.

    You also might wonder why we use dictionaries at all when the same
    concept of mapping a key to a value could be implemented using a
    list of tuples:

    .. sourcecode:: python3

        >>> {"apples": 430, "bananas": 312, "oranges": 525, "pears": 217}
        {'pears': 217, 'apples': 430, 'oranges': 525, 'bananas': 312}
        >>> [('apples', 430), ('bananas', 312), ('oranges', 525), ('pears', 217)]
        [('apples', 430), ('bananas', 312), ('oranges', 525), ('pears', 217)]

    The reason is dictionaries are very fast, implemented using a
    technique called hashing, which allows us to access a value very
    quickly. By contrast, the list of tuples implementation is slow. If
    we wanted to find a value associated with a key, we would have to
    iterate over every tuple, checking the 0th element. What if the key
    wasn't even in the list? We would have to get to the end of it to
    find out.

Another way to create a dictionary is to provide a list of key:value pairs
using the same syntax as the previous output:

    .. sourcecode:: python3
        
        >>> english_spanish = {"one": "uno", "two": "dos", "three": "tres"}

It doesn't matter what order we write the pairs. The values in a dictionary are
accessed with keys, not with indices, so there is no need to care about
ordering.

Here is how we use a key to look up the corresponding value:

    .. sourcecode:: python3
        
        >>> print(english_spanish["two"])
        'dos'

The key ``"two"`` yields the value ``"dos"``.

Lists, tuples, and strings have been called *sequences*, because their items
occur in order.  The dictionary is the first compound type that we've
seen that is not a sequence, so we can't index or slice a dictionary. 

.. index:: del statement, statement; del

Dictionary operations
---------------------

The ``del`` statement removes a key:value pair from a dictionary. For example,
the following dictionary contains the names of various fruits and the number of
each fruit in stock:

    .. sourcecode:: python3
        
        >>> inventory = {"apples": 430, "bananas": 312, "oranges": 525, "pears": 217}
        >>> print(inventory)
        {'pears': 217, 'apples': 430, 'oranges': 525, 'bananas': 312}

If someone buys all of the bananas, we can remove the entry from the dictionary:

    .. sourcecode:: python3
        
        >>> del inventory["bananas"]
        >>> print(inventory)
        {'apples': 430, 'oranges': 525, 'pears': 217}

If we then try to see how many bananas we have, we get an error (because, yes, we have no bananas). (Try this!)

Or if we're expecting more bananas soon, we might just change the value
associated with bananas:

    .. sourcecode:: python3
        
        >>> inventory["bananas"] = 0
        >>> print(inventory)
        {'pears': 217, 'apples': 430, 'oranges': 525, 'bananas': 0}
    
A new shipment of bananas arriving could be handled like this:

    .. sourcecode:: python3
        
        >>> inventory["bananas"] += 200
        >>> print(inventory)
        {'pears': 0, 'apples': 430, 'oranges': 525, 'bananas': 512}

The ``len`` function also works on dictionaries; it returns the number
of key:value pairs:

    .. sourcecode:: python3
        
        >>> len(inventory)
        4


Dictionary methods
------------------

Dictionaries have a number of useful built-in methods.

The ``keys`` method returns what Python 3 calls a **view** of its underlying keys.  
A view object has some similarities to the ``range`` object we saw earlier ---
it is a lazy promise, to deliver its elements when they're needed by the 
rest of the program.  We can iterate over the view, or turn the view into a 
list like this:

    .. sourcecode:: python3
        :linenos:
        
        for key in english_spanish.keys():   # The order of the k's is not defined
           print("Got key", key, "which maps to value", english_spanish[key])     
           
        keys = list(english_spanish.keys())
        print(keys)
    
This produces this output:

    .. sourcecode:: python3
    
        Got key three which maps to value tres
        Got key two which maps to value dos
        Got key one which maps to value uno
        ['three', 'two', 'one']
    
It is so common to iterate over the keys in a dictionary that we can
omit the ``keys`` method call in the ``for`` loop --- iterating over
a dictionary implicitly iterates over its keys:

    .. sourcecode:: python3
        :linenos:
        
        for key in english_spanish:     
           print("Got key", key)     
       

The ``values`` method is similar; it returns a view object which can be turned
into a list:  

    .. sourcecode:: python3
        
        >>> list(english_spanish.values())
        ['tres', 'dos', 'uno']

The ``items`` method also returns a view, which promises a list of tuples --- one 
tuple for each key:value pair:

    .. sourcecode:: python3
        
        >>> list(english_spanish.items())
        [('three', 'tres'), ('two', 'dos'), ('one', 'uno')]
    
Tuples are often useful for getting both the key and the value at the same
time while we are looping:

    .. sourcecode:: python3
       :linenos:
    
       for (key,value) in english_spanish.items():
           print("Got",key,"that maps to",value)
           
This produces:

    .. sourcecode:: python3
    
        Got three that maps to tres
        Got two that maps to dos
        Got one that maps to uno

    
The ``in`` and ``not in`` operators can test if a key is in the dictionary:

    .. sourcecode:: python3
        
        >>> "one" in english_spanish
        True
        >>> "six" in english_spanish
        False
        >>> "tres" in english_spanish    # Note that 'in' tests keys, not values.
        False
     

This method can be very useful, since looking up a non-existent key in a
dictionary causes a runtime error:

    .. sourcecode:: python3
        
        >>> english_spanish["dog"]
        Traceback (most recent call last):
          ...
        KeyError: 'dog'

.. index:: aliases

Aliasing and copying
--------------------

As in the case of lists, because dictionaries are mutable, we need to be 
aware of aliasing.  Whenever
two variables refer to the same object, changes to one affect the other.

If we want to modify a dictionary and keep a copy of the original, use the
``copy`` method. For example, ``opposites`` is a dictionary that contains pairs
of opposites:

    .. sourcecode:: python3
        
        >>> opposites = {"up": "down", "right": "wrong", "yes": "no"}
        >>> alias = opposites
        >>> copy = opposites.copy()  # Shallow copy

``alias`` and ``opposites`` refer to the same object; ``copy`` refers to a
fresh copy of the same dictionary. If we modify ``alias``, ``opposites`` is
also changed:

    .. sourcecode:: python3
        
        >>> alias["right"] = "left"
        >>> opposites["right"]
        'left'

If we modify ``copy``, ``opposites`` is unchanged:

    .. sourcecode:: python3
        
        >>> copy["right"] = "privilege"
        >>> opposites["right"]
        'left'

Counting letters
----------------

In the exercises in Chapter 8 (Strings) we wrote a function that counted the number of occurrences of a
letter in a string. A more general version of this problem is to form a
frequency table of the letters in the string, that is, how many times each letter
appears.

Such a frequency table might be useful for compressing a text file. Because different
letters appear with different frequencies, we can compress a file by using
shorter codes for common letters and longer codes for letters that appear less
frequently.

Dictionaries provide an elegant way to generate a frequency table:

    .. sourcecode:: python3
        
        >>> letter_counts = {}
        >>> for letter in "Mississippi":
        ...     letter_counts[letter] = letter_counts.get(letter, 0) + 1
        ...
        >>> letter_counts
        {'M': 1, 's': 4, 'p': 2, 'i': 4}

We start with an empty dictionary. For each letter in the string, we find the
current count (possibly zero) and increment it. At the end, the dictionary
contains pairs of letters and their frequencies.

It might be more appealing to display the frequency table in alphabetical order. We
can do that with the ``items`` and ``sort`` methods (more precisely, ``sort`` orders
lexicographically):

    .. sourcecode:: python3
        
        >>> letter_items = list(letter_counts.items())
        >>> letter_items.sort()
        >>> print(letter_items)
        [('M', 1), ('i', 4), ('p', 2), ('s', 4)]

Notice in the first line we had to call the type conversion function ``list``.
That turns the promise we get from ``items`` into a list, a step that is 
needed before we can use the list's ``sort`` method. 

Glossary
--------

.. glossary::
       
    call graph 
        A graph consisting of nodes which represent function frames (or invocations), 
        and directed edges (lines with arrows) showing which frames gave
        rise to other frames.       
        
    dictionary
        A collection of key:value pairs that maps from keys to values. The keys
        can be any immutable value, and the associated value can be of any type.

    immutable data value
        A data value which cannot be modified.  Assignments to elements or
        slices (sub-parts) of immutable values cause a runtime error.

    key
        A data item that is *mapped to* a value in a dictionary. Keys are used
        to look up values in a dictionary. Each key must be unique
        across the dictionary.

    key:value pair
        One of the pairs of items in a dictionary. Values are looked up in a
        dictionary by key.
        
    mapping type
        A mapping type is a data type comprised of a collection of keys and
        associated values. Python's only built-in mapping type is the
        dictionary.  Dictionaries implement the
        `associative array <http://en.wikipedia.org/wiki/Associative_array>`__
        abstract data type.

    memo
        Temporary storage of precomputed values to avoid duplicating the same computation.

    mutable data value
        A data value which can be modified. The types of all mutable values 
        are compound types.  Lists and dictionaries are mutable; strings
        and tuples are not.


Exercises
---------

#. Write a program that reads a string and returns a
   table of the letters of the alphabet in alphabetical order which occur in
   the string together with the number of times each letter occurs. Case should 
   be ignored. A sample output of the program when the user enters the data 
   "ThiS is String with Upper and lower case Letters", would look this this::

       a  2
       c  1
       d  1
       e  5
       g  1
       h  2
       i  4
       l  2
       n  2
       o  1
       p  2
       r  4
       s  5
       t  5
       u  1
       w  2

#. Give the Python interpreter's response to each of the following from a
   continuous interpreter session:

   a.
      .. sourcecode:: python3
        
          >>> dictionary = {"apples": 15, "bananas": 35, "grapes": 12} 
          >>> dictionary["bananas"] 

   b.
      .. sourcecode:: python3
        
          >>> dictionary["oranges"] = 20
          >>> len(dictionary) 

   c.
      .. sourcecode:: python3
        
          >>> "grapes" in dictionary
          
   d.
      .. sourcecode:: python3
        
          >>> dictionary["pears"]
          
   e.
      .. sourcecode:: python3
        
          >>> dictionary.get("pears", 0)
          
   f.
      .. sourcecode:: python3
        
          >>> fruits = list(dictionary.keys())
          >>> fruits.sort()
          >>> print(fruits)
          
   g.
      .. sourcecode:: python3
        
          >>> del dictionary["apples"]
          >>> "apples" in dictionary
          

   Be sure you understand why you get each result. Then apply what you
   have learned to fill in the body of the function below:

       .. sourcecode:: python3
           :linenos:
        
           def add_fruit(inventory, fruit, quantity=0): 
                return None
           
           # Make these tests work...
           new_inventory = {}
           add_fruit(new_inventory, "strawberries", 10)
           print("strawberries" in new_inventory)
           print(new_inventory["strawberries"] == 10)
           add_fruit(new_inventory, "strawberries", 25)
           print(new_inventory["strawberries"] == 35)      

#. Write a program called ``alice_words.py`` that creates a text file named
   ``alice_words.txt`` containing an alphabetical listing of all the words, and the
   number of times each occurs, in the text version of `Alice's Adventures in Wonderland`.  
   (You can obtain a free plain text version of the book, along with many others, from 
   http://www.gutenberg.org.) The first 10 lines of your output file should look
   something like this::

        Word              Count
        =======================
        a                 631
        a-piece           1
        abide             1
        able              1
        about             94
        above             3
        absence           1
        absurd            2

   How many times does the word ``alice`` occur in the book?
   
#. What is the longest word in Alice in Wonderland? How many characters does it have?

..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".

|
 





..  Copyright (C)  Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".
    Stuff has been added by Kasper Loopstra, GPL 1.3 or later. 

    
The way of the program
=======================

The goal of this book is to teach you to think like a computer scientist. This
way of thinking combines some of the best features of mathematics, engineering,
and natural science. Like mathematicians, computer scientists use formal
languages to denote ideas (specifically computations). Like engineers, they
design things, assembling components into systems and evaluating tradeoffs
among alternatives.  Like scientists, they observe the behavior of complex
systems, form hypotheses, and test predictions.

The single most important skill for a computer scientist is **problem
solving**. Problem solving means the ability to formulate problems, think
creatively about solutions, and express a solution clearly and accurately. As
it turns out, the process of learning to program is an excellent opportunity to
practice problem-solving skills. That's why this chapter is called, The way of
the program.

On one level, you will be learning to program, a useful skill by itself. On
another level, you will use programming as a means to an end. As we go along,
that end will become clearer.


.. index:: programming language, portable, high-level language,
           low-level language, compile, interpret, PyScripter

The Python programming language
-------------------------------

The programming language you will be learning is Python. Python is an example
of a **high-level language**; other high-level languages you might have heard
of are C++, PHP, Pascal, C#, and Java.

As you might infer from the name high-level language, there are also
**low-level languages**, sometimes referred to as machine languages or assembly
languages. Loosely speaking, computers can only execute programs written in
low-level languages. Thus, programs written in a high-level language have to be
translated into something more suitable before they can run. 

Almost all programs are written in high-level languages because of their advantages.
It is much easier to program in a
high-level language so programs take less time
to write, they are shorter and easier to read, and they are more likely to be
correct. Second, high-level languages are **portable**, meaning that they can
run on different kinds of computers with few or no modifications.  

The engine that translates and runs Python is called the **Python Interpreter**: 
There are two ways to use it: *immediate mode* and *script
mode*. In immediate mode, you type Python expressions into the Python Interpreter window, 
and the interpreter immediately shows the result:

.. image:: illustrations/interpreter_sshot.png
   :alt: Screen shot of interpreter

The ``>>>`` is called the **Python prompt**. The interpreter uses the prompt to indicate that it is ready for
instructions. We typed ``2 + 2``, and the interpreter evaluated our expression, and replied ``4``, 
and on the next line it gave a new prompt, indicating that it is ready for more input.   

Alternatively, you can write a program in a file and use the interpreter to
execute the contents of the file. Such a file is called a **script**.   Scripts have the
advantage that they can be saved to disk, printed, and so on. 
   
Working directly in the interpreter is convenient for testing short bits of code because you
get immediate feedback. Think of it as scratch paper used to help you work out
problems. Anything longer than a few lines should be put into a script.


.. index:: program, algorithm

What is a program?
------------------

A **program** is a sequence of instructions that specifies how to perform a
computation. The computation might be something mathematical, such as solving a
system of equations or finding the roots of a polynomial, but it can also be a
symbolic computation, such as searching and replacing text in a document or
(strangely enough) compiling a program.

The details look different in different languages, but a few basic instructions
appear in just about every language:

input
    Get data from the keyboard, a file, or some other device such as a sensor.

output
    Display data on the screen or send data to a file or other device such as a motor.

math
    Perform basic mathematical operations like addition and multiplication.

conditional execution
    Check for certain conditions and execute the appropriate sequence of
    statements.

repetition
    Perform some action repeatedly, usually with some variation.

Believe it or not, that's pretty much all there is to it. Every program you've
ever used, no matter how complicated, is made up of instructions that look more
or less like these. Thus, we can describe programming as the process of
breaking a large, complex task into smaller and smaller subtasks until the
subtasks are simple enough to be performed with sequences of these basic
instructions.

That may be a little vague, but we will come back to this topic later when we
talk about **algorithms**.


.. index:: debugging, bug

What is debugging?
------------------

Programming is a complex process, and because it is done by human beings, it
often leads to errors. Programming errors are called
**bugs** and the process of tracking them down and correcting them is called
**debugging**.  Use of the term *bug* to describe small engineering difficulties
dates back to at least 1889, when Thomas Edison had a bug with his phonograph.

Three kinds of errors can occur in a program: `syntax errors
<http://en.wikipedia.org/wiki/Syntax_error>`__, `runtime errors
<http://en.wikipedia.org/wiki/Runtime_error>`__, and `semantic errors
<http://en.wikipedia.org/wiki/Logic_error>`__.  It is useful to
distinguish between them in order to track them down more quickly.


.. index:: syntax, syntax error

Syntax errors
-------------

Python can only execute a program if the program is syntactically correct;
otherwise, the process fails and returns an error message.  **Syntax** refers
to the structure of a program and the rules about that structure. For example,
in English, a sentence must begin with a capital letter and end with a period.
this sentence contains a **syntax error**. So does this one  

For most readers, a few syntax errors are not a significant problem, which is
why we can read the poetry of E. E. Cummings without problems.
Python is not so forgiving. If there is a single syntax error anywhere in your
program, Python will display an error message and quit, and you will not be able
to run your program. During the first few weeks of your programming career, you
will probably spend a lot of time tracking down syntax errors. As you gain
experience, though, you will make fewer errors and find them faster.


.. index:: runtime error, exception, safe language

Runtime errors
--------------

The second type of error is a runtime error, so called because the error does
not appear until you run the program. These errors are also called
**exceptions** because they usually indicate that something exceptional (and
bad) has happened.

Runtime errors are rare in the simple programs you will see in the first few
chapters, so it might be a while before you encounter one.


.. index:: semantics, semantic error

Semantic errors
---------------

The third type of error is the **semantic error**. If there is a semantic error
in your program, it will run successfully, in the sense that the computer will
not generate any error messages, but it will not do the right thing. It will do
something else. Specifically, it will do what you told it to do.

The problem is that the program you wrote is not the program you wanted to
write. The meaning of the program (its semantics) is wrong.  Identifying
semantic errors can be tricky because it requires you to work backward by
looking at the output of the program and trying to figure out what it is doing.


.. index::
    single: Holmes, Sherlock
    single: Doyle, Arthur Conan 
    single: Linux

Experimental debugging
----------------------

One of the most important skills you will acquire is debugging.  Although it
can be frustrating, debugging is one of the most intellectually rich,
challenging, and interesting parts of programming.

In some ways, debugging is like detective work. You are confronted with clues,
and you have to infer the processes and events that led to the results you see.

Debugging is also like an experimental science. Once you have an idea what is
going wrong, you modify your program and try again. If your hypothesis was
correct, then you can predict the result of the modification, and you take a
step closer to a working program. If your hypothesis was wrong, you have to
come up with a new one. As Sherlock Holmes pointed out, When you have
eliminated the impossible, whatever remains, however improbable, must be the
truth. (A. Conan Doyle, *The Sign of Four*)

For some people, programming and debugging are the same thing. That is,
programming is the process of gradually debugging a program until it does what
you want. The idea is that you should start with a program that does
*something* and make small modifications, debugging them as you go, so that you
always have a working program.

For example, Linux is an operating system kernel that contains millions of
lines of code, but it started out as a simple program Linus Torvalds used to
explore the Intel 80386 chip. According to Larry Greenfield, one of Linus's
earlier projects was a program that would switch between displaying AAAA and
BBBB. This later evolved to Linux (*The Linux Users' Guide* Beta Version 1).

Later chapters will make more suggestions about debugging and other programming
practices.


.. index:: formal language, natural language, parse, token

Formal and natural languages
----------------------------

**Natural languages** are the languages that people speak, such as English,
Spanish, and French. They were not designed by people (although people try to
impose some order on them); they evolved naturally.

**Formal languages** are languages that are designed by people for specific
applications. For example, the notation that mathematicians use is a formal
language that is particularly good at denoting relationships among numbers and
symbols. Chemists use a formal language to represent the chemical structure of
molecules. And most importantly:

    *Programming languages are formal languages that have been designed to
    express computations.*

Formal languages tend to have strict rules about syntax. For example, ``3+3=6``
is a syntactically correct mathematical statement, but ``3=+6$`` is not.
H\ :sub:`2`\ O is a syntactically correct chemical name, but :sub:`2`\ Zz is
not.

Syntax rules come in two flavors, pertaining to **tokens** and structure.
Tokens are the basic elements of the language, such as words, numbers, parentheses,
commas, and so on. In Python, a statement like ``print("Happy New Year for ",2013)`` 
has 6 tokens: a function name, an open parenthesis (round bracket), a string, a comma, a number, and a close parenthesis.

It is possible to make errors in the way one constructs tokens.  
One of the problems with ``3=+6$`` is that ``$`` is not a
legal token in mathematics (at least as far as we know). Similarly,
:sub:`2`\ Zz is not a legal token in chemistry notation because there is no element with the abbreviation
``Zz``.

The second type of syntax rule pertains to the **structure** of a statement--- that
is, the way the tokens are arranged. The statement ``3=+6$`` is structurally
illegal because you can't place a plus sign immediately after an equal sign.
Similarly, molecular formulas have to have subscripts after the element name,
not before.  And in our Python example, if we omitted the comma, or if we changed the two
parentheses around to say  ``print)"Happy New Year for ",2013(`` our statement would still
have six legal and valid tokens, but the structure is illegal.

When you read a sentence in English or a statement in a formal language, you
have to figure out what the structure of the sentence is (although in a natural
language you do this subconsciously). This process is called **parsing**.

For example, when you hear the sentence, "The other shoe fell", you understand
that the other shoe is the subject and fell is the verb.  Once you have parsed
a sentence, you can figure out what it means, or the **semantics** of the sentence.
Assuming that you know what a shoe is and what it means to fall, you will
understand the general implication of this sentence.

Although formal and natural languages have many features in common --- tokens,
structure, syntax, and semantics --- there are many differences:

.. glossary::

    ambiguity
        Natural languages are full of ambiguity, which people deal with by
        using contextual clues and other information. Formal languages are
        designed to be nearly or completely unambiguous, which means that any
        statement has exactly one meaning, regardless of context.

    redundancy
        In order to make up for ambiguity and reduce misunderstandings, natural
        languages employ lots of redundancy. As a result, they are often
        verbose.  Formal languages are less redundant and more concise.

    literalness
        Formal languages mean exactly what they say.  On the other hand, natural languages 
        are full of idiom and metaphor. If someone says, "The
        other shoe fell", there is probably no shoe and nothing falling.  
        You'll need to find the 
        original joke to understand the idiomatic meaning of the other shoe falling. 
        *Yahoo! Answers* thinks it knows!  

People who grow up speaking a natural language---everyone---often have a hard
time adjusting to formal languages. In some ways, the difference between formal
and natural language is like the difference between poetry and prose, but more
so:

.. glossary::

    poetry
        Words are used for their sounds as well as for their meaning, and the
        whole poem together creates an effect or emotional response. Ambiguity
        is not only common but often deliberate.

    prose
        The literal meaning of words is more important, and the structure
        contributes more meaning. Prose is more amenable to analysis than
        poetry but still often ambiguous.

    program
        The meaning of a computer program is unambiguous and literal, and can
        be understood entirely by analysis of the tokens and structure.

Here are some suggestions for reading programs (and other formal languages).
First, remember that formal languages are much more dense than natural
languages, so it takes longer to read them. Also, the structure is very
important, so it is usually not a good idea to read from top to bottom, left to
right. Instead, learn to parse the program in your head, identifying the tokens
and interpreting the structure.  Finally, the details matter. Little things
like spelling errors and bad punctuation, which you can get away with in
natural languages, can make a big difference in a formal language.


The first program
-----------------

Traditionally, the first program written in a new language is called *Hello,
World!* because all it does is display the words, Hello, World!  In Python, the script
looks like this: (For scripts, we'll show line numbers to the left of the Python statements.)

    .. sourcecode:: python3
        :linenos:
        
        print("Hello, World!")

This is an example of using the **print function**, which doesn't actually print
anything on paper. It displays a value on the screen. In this case, the result shown
is

    .. sourcecode:: pycon
        :linenos:
        
        Hello, World!

The quotation marks in the program mark the beginning and end of the value;
they don't appear in the result.

Some people judge the quality of a programming language by the simplicity of
the Hello, World! program. By this standard, Python does about as well as
possible.



.. index:: comment

Comments
--------

As programs get bigger and more complicated, they get more difficult to read.
Formal languages are dense, and it is often difficult to look at a piece of
code and figure out what it is doing, or why.

For this reason, it is a good idea to add notes to your programs to explain in
natural language what the program is doing.  

A **comment** in a computer program is text that is intended
only for the human reader --- it is completely ignored by the interpreter.

In Python, the `#` token starts a comment.  The rest of the line 
is ignored.   Here is a new version of *Hello, World!*.

    .. sourcecode:: python3
        :linenos:
        
        #---------------------------------------------------
        # This demo program shows off how elegant Python is!
        # Written by Joe Soap, December 2010.
        # Anyone may freely copy or modify this program.
        #---------------------------------------------------
        
        print("Hello, World!")     # Isn't this easy! 
      
You'll also notice that we've left a blank line in the program.  Blank lines
are also ignored by the interpreter, but comments and blank lines can make your
programs much easier for humans to parse.  Use them liberally! 


Glossary
--------

.. glossary::

    algorithm
        A set of specific steps for solving a category of problems.

    bug
        An error in a program.

    comment
        Information in a program that is meant for other programmers (or anyone
        reading the source code) and has no effect on the execution of the
        program.    

    debugging
        The process of finding and removing any of the three kinds of
        programming errors.

    exception
        Another name for a runtime error.

    formal language
        Any one of the languages that people have designed for specific
        purposes, such as representing mathematical ideas or computer programs;
        all programming languages are formal languages.

    high-level language
        A programming language like Python that is designed to be easy for
        humans to read and write.

    immediate mode
        A style of using Python where we type expressions at the command prompt, and 
        the results are shown immediately.  Contrast with **script**, and see the
        entry under **Python shell**.
    
    interpreter
        The engine that executes your Python scripts or expressions.
        
    low-level language
        A programming language that is designed to be easy for a computer to
        execute; also called machine language or assembly language.

    natural language
        Any one of the languages that people speak that evolved naturally.

    object code
        The output of the compiler after it translates the program.

    parse
        To examine a program and analyze the syntactic structure.

    portability
        A property of a program that can run on more than one kind of computer.

    print function
        A function used in a program or script that causes the Python interpreter to 
        display a value on its output device.

    problem solving
        The process of formulating a problem, finding a solution, and
        expressing the solution.

    program
        a sequence of instructions that specifies to a computer actions and
        computations to be performed.

    Python shell
        An interactive user interface to the Python interpreter. The user of a
        Python shell types commands at the prompt (>>>), and presses the return
        key to send these commands immediately to the interpreter for
        processing.  The word *shell* comes from Unix.

    runtime error
        An error that does not occur until the program has started to execute
        but that prevents the program from continuing.

    script
        A program stored in a file (usually one that will be interpreted).

    semantic error
        An error in a program that makes it do something other than what the
        programmer intended.

    semantics
        The meaning of a program.

    source code
        A program in a high-level language before being compiled.

    syntax
        The structure of a program.

    syntax error
        An error in a program that makes it impossible to parse --- and
        therefore impossible to interpret.

    token
        One of the basic elements of the syntactic structure of a program,
        analogous to a word in a natural language.


Exercises
---------

#. Write an English (or Dutch!) sentence with understandable semantics but incorrect
   syntax. Write another English (or Dutch!) sentence which has correct syntax but has 
   semantic errors.
#. Using the Python interpreter, type ``1 + 2`` and then hit return. Python *evaluates* 
   this *expression*, displays the result, and then shows another prompt. ``*``
   is the *multiplication operator*, and ``**`` is the
   *exponentiation operator*. Experiment by entering different expressions and
   recording what is displayed by the Python interpreter. 
#. Type ``1 2`` and then hit return. Python tries to evaluate the expression,
   but it can't because the expression is not syntactically legal. Instead, it
   shows the error message:
   
        .. sourcecode:: python3

              File "<interactive input>", line 1
                1 2
                  ^
            SyntaxError: invalid syntax

   In many cases, Python indicates where the syntax error occurred, but it is
   not always right, and it doesn't give you much information about what is
   wrong.
   
   So, for the most part, the burden is on you to learn the syntax rules.
   
   In this case, Python is complaining because there is no operator between the
   numbers.
   
   See if you can find a few more examples of things that will produce error
   messages when you enter them at the Python prompt. Write down what you enter
   at the prompt and the last line of the error message that Python reports
   back to you. 
   
#. Type  ``print("hello")``. Python executes this, which has the effect
   of printing the letters h-e-l-l-o. Notice that the quotation marks that you
   used to enclose the string are not part of the output.  Now type ``"hello"``
   and describe your result.  Make notes of when you see the quotation marks
   and when you don't.
    
#. Type ``cheese`` without the quotation marks. The output will look
   something like this::

        Traceback (most recent call last):
          File "<interactive input>", line 1, in ?
        NameError: name 'cheese' is not defined

   This is a run-time error; specifically, it is a NameError, and even more
   specifically, it is an error because the name `cheese` is not defined. If
   you don't know what that means yet, you will soon.
#. Type ``6 + 4 * 9`` at the Python prompt and hit enter.  Record what
   happens.
   
   Now create a Python script with the following contents:

       .. sourcecode:: python3
          :linenos:
        
           6 + 4 * 9

   What happens when you run this script? Now change the script contents to:

       .. sourcecode:: python3
           :linenos:
        
           print(6 + 4 * 9)

   and run it again.
   
   What happened this time?
   
   Whenever an *expression* is typed at the Python prompt, it is evaluated
   and the result is *automatically* shown on the line below.  (Like on your calculator,
   if you type this expression you'll get the result 42.)   
   
   A script is different, however.  Evaluations of
   expressions are not automatically displayed, 
   so it is necessary to use the **print** function to make the answer
   show up.  
   
   It is hardly ever necessary to use the print function in immediate mode at the command prompt.  

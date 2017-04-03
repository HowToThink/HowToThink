..  Copyright (C) Peter Wentworth, Jeffrey Elkner, Allen B. Downey and Chris Meyers.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with Invariant Sections being Foreword, Preface, and Contributor List, no
    Front-Cover Texts, and no Back-Cover Texts.  A copy of the license is
    included in the section entitled "GNU Free Documentation License".
 
|    
    
Files
=====

.. index:: file, handle, file handle   
    
About files
-----------

While a program is running, its data is stored in *random access memory* (RAM).
RAM is fast and inexpensive, but it is also **volatile**, which means that when
the program ends, or the computer shuts down, data in RAM disappears. To make
data available the next time the computer is turned on and the program
is started, it has to be written to a **non-volatile** storage medium,
such a hard drive, usb drive, or CD-RW.

Data on non-volatile storage media is stored in named locations
called **files**. By reading and writing files, programs can save information
between program runs.

Working with files is a lot like working with a notebook. To use a notebook,
it has to be opened. When done, it has to be closed.  While the
notebook is open, it can either be read from or written to. In either case,
the notebook holder knows where they are. They can read the whole notebook in its
natural order or they can skip around.

All of this applies to files as well. To open a file, we specify its name and
indicate whether we want to read or write. 

Writing our first file
----------------------

Let's begin with a simple program that writes three lines of text into a file:   

    .. sourcecode:: python3
        :linenos:
        
        with open("test.txt", "w") as myfile:
            myfile.write("My first file written from Python\n")
            myfile.write("---------------------------------\n")
            myfile.write("Hello, world!\n")

Opening a file creates what we call a file **handle**. In this example, the variable ``myfile``
refers to the new handle object.  Our program calls methods on the handle, and this makes
changes to the actual file which is usually located on our disk.  

On line 1, the open function takes two arguments. The first is the name of the file, and
the second is the **mode**. Mode ``"w"`` means that we are opening the file for
writing.

With mode ``"w"``, if there is no file named ``test.txt`` on the disk,
it will be created. If there already is one, it will be replaced by the
file we are writing.

To put data in the file we invoke the ``write`` method on the handle, shown
in lines 2, 3 and 4 above.  In bigger programs, lines 2--4 will usually be
replaced by a loop that writes many more lines into the file.

The file is closed after line 4, at the end of the ``with`` block. A ``with`` block make sure that the file get close even if an error occurs (power outages excluded). 


    .. admonition:: A handle is somewhat like a TV remote control

        We're all familiar with a remote control for a TV.  We perform operations on
        the remote control --- switch channels, change the volume, etc.  But the real action
        happens on the TV.  So, by simple analogy, we'd call the remote control our `handle`
        to the underlying TV.
        
        Sometimes we want to emphasize the difference --- the file handle is not the same
        as the file, and the remote control is not the same as the TV.  
        But at other times we prefer to treat them as a single mental chunk, or abstraction, 
        and we'll just say "close the file", or "flip the TV channel". 



Reading a file line-at-a-time
-----------------------------

Now that the file exists on our disk, we can open it, this time for reading, and read all
the lines in the file, one at a time. This time, the mode argument is ``"r"`` for reading:

    .. sourcecode:: python3
        :linenos:
            
        with open("test.txt", "r") as my_new_handle: 
            for the_line in my_new_handle: 
                # Do something with the line we just read.
                # Here we just print it.
                print(the_line, end="")

This is a handy pattern for our toolbox. In bigger programs, we'd
squeeze more extensive logic into the body of the loop at line 5 ---
for example, if each line of the file contained the name and email address
of one of our friends, perhaps we'd split the line into some pieces and 
call a function to send the friend a party invitation.

On line 5 we suppress the newline character that ``print``
usually appends to our strings with ``end=""``.  Why?  This is because the string already
has its own newline:  the ``for`` statement in line 2 reads everything
up to *and including* the newline character.

If we try to open a file that doesn't exist, we get an error:

    .. sourcecode:: python3
        
        >>> mynewhandle = open("wharrah.txt", "r")
        FileNotFoundError: [Errno 2] No such file or directory: "wharrah.txt"

Turning a file into a list of lines
-----------------------------------

It is often useful to fetch data from
a disk file and turn it into a list of lines. Suppose we have a
file containing our friends and their email addresses, one per line
in the file.  But we'd like the lines sorted into
alphabetical order.  A good plan is to read everything into a
list of lines, then sort the list, and then write the sorted list 
back to another file:

    .. sourcecode:: python3
        :linenos:
              
        with open("friends.txt", "r") as input_file:
            all_lines = input_file.readlines() 
        
        all_lines.sort()
        
        with open("sortedfriends.txt", "w") as output_file:
            for line in all_lines:
                outut_file.write(line)
        
The ``readlines`` method in line 2 reads all the lines and
returns a list of the strings.  

We could have used the template from the previous section to read each line
one-at-a-time, and to build up the list ourselves, but it is a lot easier
to use the method that the Python implementors gave us! 
        
        
Reading the whole file at once
------------------------------        
        
Another way of working with text files is to read the complete
contents of the file into a string, and then to use our string-processing
skills to work with the contents.   

We'd normally use this method of processing files if we were not
interested in the line structure of the file. For example, we've
seen the ``split`` method on strings which can break a string into 
words.  So here is how we might count the number of words in a
file:

    .. sourcecode:: python3
        :linenos:
              
        with open("somefile.txt") as f:
            content = f.read() 
        words = content.split()    
        print("There are {0} words in the file.".format(len(words)))
        
Notice here that we left out the ``"r"`` mode in line 1.
By default, if we don't supply the mode, Python opens the file for reading.       

.. admonition:: Your file paths may need to be explicitly named.

   In the above example, we're assuming that the file ``somefile.txt`` is 
   in the same directory as your Python source code.  If this is
   not the case, you may need to provide a full or a relative path to the file.  On Windows, a full
   path could look like ``"C:\\temp\\somefile.txt"``, while on a Unix system the full path could be
   ``"/home/jimmy/somefile.txt"``.
   
   We'll return to this later in this chapter.
 
An example
----------

Many useful line-processing programs will read a text file line-at-a-time and do some minor
processing as they write the lines to an output file. They might number the
lines in the output file, or insert extra blank lines after every 60 lines to
make it convenient for printing on sheets of paper, or extract some specific
columns only from each line in the source file, or only print lines that 
contain a specific substring.  We call this kind of program a **filter**. 

Here is a filter that copies one file to another, 
omitting any lines that begin with ``#``:

    .. sourcecode:: python3
       :linenos:
        
        def filter(oldfile, newfile):
            with open(oldfile, "r") as infile, open(newfile, "w") as outfile:
                for line in infile:
                    # Put any processing logic here
                    if not line.startswith('#'):
                        outfile.write(line)

On line 2, we open two files: the file to read, and the file to write. From line 3, we read the input file line by line. We write the line in the output file only if the condition on line 5 is true.

.. index:: directory

Directories
-----------

Files on non-volatile storage media are organized by a set of rules known as a
**file system**. File systems are made up of files and **directories**, which
are containers for both files and other directories.

When we create a new file by opening it and writing, the new file goes in the
current directory (wherever we were when we ran the program). Similarly, when
we open a file for reading, Python looks for it in the current directory.

If we want to open a file somewhere else, we have to specify the **path** to
the file, which is the name of the directory (or folder) where the file is
located:

    .. sourcecode:: pycon
        
        >>> wordsfile = open("/usr/share/dict/words", "r")
        >>> wordlist = wordsfile.readlines()
        >>> print(wordlist[:6])
        ['\n', 'A\n', "A's\n", 'AOL\n', "AOL's\n", 'Aachen\n']

This (Unix) example opens a file named ``words`` that resides in a directory named
``dict``, which resides in ``share``, which resides in ``usr``, which resides
in the top-level directory of the system, called ``/``. It then reads in each
line into a list using ``readlines``, and prints out the first 5 elements from
that list.  

A Windows path might be ``"c:/temp/words.txt"`` or ``"c:\\temp\\words.txt"``.
Because backslashes are used to escape things like newlines and tabs, we need 
to write two backslashes in a literal string to get one!  So the length of these two
strings is the same!

We cannot use ``/`` or ``\`` as part of a filename; they are reserved as a **delimiter**
between directory and filenames.

The file ``/usr/share/dict/words`` should exist on Unix-based systems, and
contains a list of words in alphabetical order.


What about fetching something from the web?
-------------------------------------------

The Python libraries are pretty messy in places.  But here is a very
simple example that copies the contents at some web URL to a local file.

    .. sourcecode:: python3
        :linenos:
        
        import urllib.request

        url = "http://xml.resource.org/public/rfc/txt/rfc793.txt" 
        destination_filename = "rfc793.txt"
        
        urllib.request.urlretrieve(url, destination_filename)

The ``urlretrieve`` function --- just one call --- could be used
to download any kind of content from the Internet.
   
We'll need to get a few things right before this works:  
 * The resource we're trying to fetch must exist!  Check this using a browser.
 * We'll need permission to write to the destination filename, and the file will
   be created in the "current directory" - i.e. the same folder that the Python program is saved in.
 * If we are behind a proxy server that requires authentication, 
   (as some students are), this may require some more special handling to work around our proxy.  
   Use a local resource for the purpose of this demonstration! 
  
Here is a slightly different example using the *requests* module. This module is not part of the standard library distributed with python, however it is easier to use and significantly more potent than the *urllib* module distributed with python. Read *requests* documentation on `<http://docs.python-requests.org>`_ to learn how to install and use the module. Here, rather than save the web resource to
our local disk, we read it directly into a string, and we print that string:

.. sourcecode:: python3
    :linenos:
    
    import requests

    url = "http://xml.resource.org/public/rfc/txt/rfc793.txt"
    response = requests.get(url)
    print(response.text)

Opening the remote URL returns the response from the server. That response contains several types of information, and the *requests* module allows us to access them in various ways. On line 5, we get the downloaded document as a single string. We could also read it line by line as follows:

.. sourcecode:: python3
    :linenos:
    
    import requests

    url = "http://xml.resource.org/public/rfc/txt/rfc793.txt"
    response = requests.get(url)
    for line in response:
        print(line)


Glossary
--------

.. glossary::


    delimiter
        A sequence of one or more characters used to specify the boundary
        between separate parts of text.

    directory
        A named collection of files, also called a folder.  Directories can
        contain files and other directories, which are referred to as
        *subdirectories* of the directory that contains them.

    file
        A named entity, usually stored on a hard drive, floppy disk, or CD-ROM,
        that contains a stream of characters.

    file system
        A method for naming, accessing, and organizing files and the data they
        contain. 
        
    handle
        An object in our program that is connected to an underlying resource (e.g. a file).
        The file handle lets our program manipulate/read/write/close the actual 
        file that is on our disk.
            
    mode
        A distinct method of operation within a computer program.  Files in
        Python can be opened in one of four modes: read (``"r"``), write
        (``"w"``), append (``"a"``), and read and write (``"+"``).
     
    non-volatile memory
        Memory that can maintain its state without power. Hard drives, flash
        drives, and rewritable compact disks (CD-RW) are each examples of
        non-volatile memory.

    path
        A sequence of directory names that specifies the exact location of a
        file.
        
    text file
        A file that contains printable characters organized into lines
        separated by newline characters.
        
    volatile memory
        Memory which requires an electrical current to maintain state. The
        *main memory* or RAM of a computer is volatile.  Information stored in
        RAM is lost when the computer is turned off.
 
Exercises
---------
         
#. Write a program that reads a file and writes out a new file 
   with the lines in reversed order
   (i.e. the first line in the old file becomes the last one in the new file.)
   
#. Write a program that reads a file and prints only those lines that contain the 
   substring ``snake``.
   
#. Write a program that reads a text file and produces an output file which is a 
   copy of the file, except the first five columns of each line contain a four
   digit line number, followed by a space. 
   Start numbering the first line in the output file at 1.  Ensure that
   every line number is formatted to the same width in the output file.  Use one
   of your Python programs as test data for this exercise: your output should be 
   a printed and numbered listing of the Python program. 

#. Write a program that undoes the numbering of the previous exercise: it should
   read a file with numbered lines and produce another file without line numbers. 
   
#. Write a program that takes the dictionary used above, and returns some of the 
   words using 1337sp34k
   

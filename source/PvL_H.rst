Plotting data with matplotlib
=============================

Introduction
------------

There are many scientific plotting packages. In this chapter we focus on
``matplotlib``, chosen because it integrates very well with Python.

This is just a short introduction to the ``matplotlib`` plotting package. Its
capabilities and customizations are described at length in the `project's
webpage <http://matplotlib.org/>`_, the `Beginner's Guide <Beginner’s Guide>`_, the ``matplotlib.pyplot``
`tutorial <http://matplotlib.org/users/pyplot_tutorial.html>`_, and the
``matplotlib.pyplot`` `documentation <http://matplotlib.org/api/pyplot_api.html>`_.
(Check in particular the `specific documentation <http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot>`_
of ``pyplot.plot``).

Basic Usage -- ``pyplot.plot``
------------------------------

Simple use of ``matplotlib`` is straightforward:

    .. sourcecode:: python3

        >>> from matplotlib import pyplot as plt
        >>> plt.plot([1,2,3,4])
        [<matplotlib.lines.Line2D at 0x7faa8d9ba400>]
        >>> plt.show()

If you run this code in the interactive Python interpreter, you should get a plot like this:

    .. image:: illustrations/mpl_basic.png
       :width: 300pt

Two things to note from this plot:

* ``pyplot.plot`` assumed our single data list to be the *y*-values;
* in the absence of an *x*-values list, [0, 1, 2, 3] was used instead.


    ..  note::
        ``pyplot`` is commonly used abbreviated as ``plt``, just as ``numpy`` is commonly
        abbreviated as ``np``. The remainder of this chapter uses the abbreviated
        form.

    ..  note::
        Enhanced interactive python interpreters such as IPython can automate
        some of the plotting calls for you. For instance, you can run
        ``%matplotlib`` in IPython, after which you no longer need to run
        ``plt.show`` everytime when calling ``plt.plot``.
        For simplicity, ``plt.show`` will also be left out of the remainder
        of these examples.

If you pass two lists to ``plt.plot`` you then explicitly set the *x* values:

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4])

    .. image:: illustrations/mpl_basic2.png
       :width: 300pt

Understandably, if you provide two lists their lengths must match: 

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4, 5])
        ValueError: x and y must have same first dimension

To plot multiple curves simply call ``plt.plot`` with as many *x*--*y* list
pairs as needed:

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4],
                        [0.1, 0.2, 0.3, 0.4], [1, 4, 9, 16])

    .. image:: illustrations/mpl_basic3.png
       :width: 300pt

Alternaltively, more plots may be added by repeatedly calling
``plt.plot``. The following code snippet produces the same plot as the
previous code example:

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4])
        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 4, 9, 16])

Adding information to the plot axes is straightforward to do:

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4]) 
        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 4, 9, 16])
        >>> plt.xlabel("Time (s)")
        >>> plt.ylabel("Scale (Bananas)")

    .. image:: illustrations/mpl_basic4.png
       :width: 300pt

And adjusting axis ranges can be done by calling ``plt.axis``
with a list of the four axis limits as its argument (in the order
*xmin*, *xmax*, *ymin*, *ymax*):

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4]) 
        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 4, 9, 16])
        >>> plt.xlabel("Time (s)")
        >>> plt.ylabel("Scale (Bananas)")
        >>> plt.axis([0,1,-5,20])

    .. image:: illustrations/mpl_basic4b.png
       :width: 300pt

In addition to *x* and *y* data lists, ``plt.plot`` can also take strings
that define the plotting style:

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4], 'rx') 
        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 4, 9, 16], 'b-.')
        >>> plt.xlabel("Time (s)")
        >>> plt.ylabel("Scale (Bananas)")

    .. image:: illustrations/mpl_basic5.png
       :width: 300pt

The style strings, one per *x*--*y* pair, specify color and shape: 'rx' stands
for red crosses, and 'b-.' stands for blue dash-point line. Check the
`documentation <http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot>`_
of ``pyplot.plot`` for the list of colors and shapes.

Finally, ``plt.plot`` can also, conveniently, take numpy arrays as its arguments.


More plots
----------

While ``plt.plot`` can satisfy basic plotting needs, ``matplotlib``
provides many more plotting functions. Below we try out the ``plt.bar``
function, for plotting bar charts. The full list of plotting functions can be found
in the the ``matplotlib.pyplot`` `documentation <http://matplotlib.org/api/pyplot_api.html>`_.

Bar charts can be plotted using ``plt.bar``, in a similar fashion to ``plt.plot``:

    .. sourcecode:: python3

        >>> plt.bar(range(7), [1, 2, 3, 4, 3, 2, 1])

    .. image:: illustrations/mpl_bar.png
       :width: 300pt

Note, however, that contrary to ``plt.plot`` you must always specify
*x* and *y* (which correspond, in bar chart terms to the left bin edges and the
bar heights). Also note that you can only plot one chart per call. For multiple,
overlapping charts you'll need to call ``plt.bar`` repeatedly.

One of the optional arguments to ``plt.bar`` is ``width``, which lets you
specify the width of the bars. Its default of 0.8 might not be the most suited
for all cases, especially when the *x* values are small:

    .. sourcecode:: python3

        >>> plt.bar(numpy.arange(0., 1.4, .2), [1, 2, 3, 4, 3, 2, 1])

    .. image:: illustrations/mpl_bar2.png
       :width: 300pt

Specifying narrower bars gives us a much better result:

    .. sourcecode:: python3

        >>> plt.bar(numpy.arange(0., 1.4, .2), [1, 2, 3, 4, 3, 2, 1], width=0.2)

    .. image:: illustrations/mpl_bar3.png
       :width: 300pt


Interactivity and saving to file
--------------------------------

If you tried out the previous examples using a Python/IPython console you
probably got for each plot an interactive window. Through the four rightmost
buttons in this window you can do a number of actions:

* Pan around the plot area;
* Zoom in and out;
* Access interactive plot size control;
* Save to file.

The three leftmost buttons will allow you to navigate between different plot
views, after zooming/panning.

    .. image:: illustrations/mpl_buttons2.png
       :width: 300pt

As explained above, saving to file can be easily done from the interactive plot
window. However, the need might arise to have your script write a plot directly
as an image, and not bring up any interactive window. This is easily done by
calling ``plt.savefig``:

    .. sourcecode:: python3

        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 2, 3, 4], 'rx') 
        >>> plt.plot([0.1, 0.2, 0.3, 0.4], [1, 4, 9, 16], 'b-.')
        >>> plt.xlabel("Time (s)")
        >>> plt.ylabel("Scale (Bananas)")
        >>> plt.savefig('the_best_plot.pdf')

    ..  note::
        When saving a plot, you'll want to choose a `vector format
        <https://en.wikipedia.org/wiki/Vector_graphics>`_
        (either pdf, ps, eps, or svg). These are resolution-independent
        formats and will yield the best quality, even if printed at very
        large sizes. Saving as png should be avoided, and saving as jpg
        should be avoided even more.


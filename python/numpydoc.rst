.. note::
   Changes to this document must be approved by the System Architect (`RFC-24 <https://jira.lsstcorp.org/browse/RFC-24>`_).
   To request changes to these standards, please file an :doc:`RFC </communications/rfc>`.

.. _numpydoc-format:

#######################################
Documenting Python APIs with Docstrings
#######################################

We use Python docstrings to create reference documentation for our Python APIs.
Docstrings are read by developers, interactive Python users, and readers of our online documentation.
This page describes how to write these docstrings in Numpydoc, DM's standard format.

**Format reference:**

- :ref:`py-docstring-basics`.
- :ref:`py-docstring-rst`.
- :ref:`py-docstring-sections`.

  1. :ref:`Short Summary <py-docstring-short-summary>`
  2. :ref:`Deprecation Warning <py-docstring-deprecation>`
  3. :ref:`Extended Summary <py-docstring-extended-summary>`
  4. :ref:`Parameters <py-docstring-parameters>`
  5. :ref:`Returns <py-docstring-returns>` or :ref:`Yields <py-docstring-yields>`
  6. :ref:`Other Parameters <py-docstring-other-parameters>`
  7. :ref:`Raises <py-docstring-raises>`
  8. :ref:`See Also <py-docstring-see-also>`
  9. :ref:`Notes <py-docstring-notes>`
  10. :ref:`References <py-docstring-references>`
  11. :ref:`Examples <py-docstring-examples>`

**How to format different APIs:**

- :ref:`py-docstring-module-structure`.
- :ref:`py-docstring-class-structure`.
- :ref:`py-docstring-method-function-structure`.
- :ref:`py-docstring-attribute-constants-structure`.
- :ref:`py-docstring-property-structure`.

**Learn by example:**

- :ref:`py-docstring-example-module`.

Treat the guidelines on this page as an extension of the :doc:`style`.

.. _py-docstring-basics:

Basic Format of Docstrings
==========================

Python docstrings form the ``__doc__`` attributes attached to modules, classes, methods and functions.
See :pep:`257` for background.

.. _py-docstring-triple-double-quotes:

Docstrings MUST be delimited by triple double quotes
----------------------------------------------------

Docstrings **must** be delimited by triple double quotes: ``"""``.
This allows docstrings to span multiple lines.
You may use ``u"""`` for unicode, but it's usually preferable to stick to ASCII.

For consistency, *do not* use triple single quotes: ``'''``.

.. _py-docstring-form:

Docstrings SHOULD begin with ``"""`` and terminate with ``"""`` on its own line
----------------------------------------------------------------------------------

The docstring's summary sentence occurs on the same line as the opening ``"""``.

The terminating ``"""`` should be on its own line, even for 'one-line' docstrings (this is a minor departure from :pep:`257`).
For example, a one-line docstring:

.. code-block:: py

   """Sum numbers in an array.
   """

(*Note:* one-line docstrings are rarely used for public APIs, see :ref:`py-docstring-sections`.)

An example of a multi-paragraph docstring:

.. code-block:: py

   """Sum numbers in an array.

   Parameters
   ----------
   values : iterable
      Python iterable whose values are summed.

   Returns
   -------
   sum : `float`
      Sum of ``values``.
   """

.. _py-docstring-blank-lines:

Docstrings of methods and functions SHOULD NOT be preceded or followed by a blank line
--------------------------------------------------------------------------------------

Inside a function or method, there should be no blank lines surrounding the docstring:

.. code-block:: py

   def sum(values):
       """Sum numbers in an array.

       Parameters
       ----------
       values : iterable
          Python iterable whose values are summed.

       Returns
       -------
       sum : `float`
          Sum of ``values``.
       """
       pass

.. _py-docstring-class-blank-lines:

Docstrings of classes SHOULD be followed, but not preceded, by a blank line
---------------------------------------------------------------------------

Like method and function docstrings, the docstring should immediately follow the class definition, without a blank space.
However, there should be a **single blank line before following code** such as class variables or the ``__init__`` method:

.. code-block:: py

   class Point(object):
       """Point in a 2D cartesian space.

       Parameters
       ----------
       x, y : `float`
          Coordinate of the point.
       """

       def __init__(x, y):
           self.x = x
           self.y = y

.. _py-docstring-indentation:

Docstring content MUST be indented with the code's scope
--------------------------------------------------------

For example:

.. code-block:: py

   def sum(values):
       """Sum numbers in an array.

       Parameters
       ----------
       values : iterable
          Python iterable whose values are summed.
       """
       pass

Not:

.. code-block:: py

   def sum(values):
       """Sum numbers in an array.

   Parameters
   ----------
   values : iterable
      Python iterable whose values are summed.
   """
       pass

.. _py-docstring-rst:

ReStructuredText in Docstrings
==============================

We use reStructuredText to mark up and give semantic meaning to text in docstrings.
ReStructuredText is lightweight enough to read in raw form, such as command line terminal printouts, but is also parsed and rendered with our Sphinx-based documentation build system.
All of the style guidance for using reStructuredText from our :doc:`/restructuredtext/style` applies in docstrings with a few exceptions defined here.

.. _py-docstring-nospace-headers:

No space between headers and paragraphs
---------------------------------------

For docstrings, the Numpydoc_ standard is to omit any space between a header and the following paragraph.

For example

.. code-block:: python

   """A summary

   Notes
   -----
   The content of the notes section directly follows the header, with no blank line.
   """

This :ref:`deviation from the normal style guide <rst-sectioning>` is in keeping with Python community idioms and to save vertical space in terminal help printouts.

.. _py-docstring-section-levels:

Sections are restricted to the Numpydoc section set
---------------------------------------------------

Sections must be from the set of standard Numpydoc sections (see :ref:`py-docstring-sections`).
You cannot introduce new section headers, or use the :ref:`full reStructuredText subsection hierarchy <rst-sectioning>`, since these subsections won't be parsed by the documentation toolchain.

Always use the dash (``-``) to underline sections.
For example:

.. code-block:: python

   def myFunction(a):
       """Do something.

       Parameters
       ----------
       [...]

       Returns
       -------
       [...]

       Notes
       -----
       [...]
       """

.. _py-docstring-subsections:

Simulate subsections with bold text
-----------------------------------

Conventional reStructuredText subsections are not allowed in docstrings, given the :ref:`previous guideline <py-docstring-section-levels>`.
However, you may structure long sections with bold text that simulates subsection headers.
This technique is useful for the :ref:`Notes <py-docstring-notes>` and :ref:`Examples <py-docstring-examples>` Numpydoc sections.
For example:

.. code-block:: python

   def myFunction(a):
       """Do something.

       [...]

       Examples
       --------
       **Example 1**

       [...]

       **Example 2**

       [...]
       """

.. _py-docstring-length:

Line Lengths
------------

Hard-wrap text in docstrings to match the :ref:`docstring line length allowed by the coding standard <style-guide-py-docstring-line-length>`.

.. _py-docstring-parameter-markup:

Marking Up Parameter Names
--------------------------

The default reStructuredText role in docstrings is ``:py:obj:``.
Sphinx automatically generates links when the API names are marked up in single backticks.
For example: ```str``` or ```lsst.pipe.base.Struct```.

You cannot use this role to mark up parameters, however.
Instead, use the code literal role (double backticks) to mark parameters and return variables in monospace type.
For example, the description for ``format`` references the ``should_plot`` parameter:

.. code-block:: rst

   Parameters
   ----------
   should_plot : `bool`
       Plot the fit if `True`.
   plot_format : `str`, optional
       Format of the plot when ``should_plot`` is `True`.

.. _py-docstring-sections:

Numpydoc Sections in Docstrings
===============================

We organize Python docstrings into sections that appear in a common order.
This format follows the `Numpydoc`_ standard (used by NumPy, SciPy, and Astropy, among other scientific Python packages) rather than the format described in :pep:`287`.
These are the sections and their relative order:

.. _Numpydoc: https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt

1. :ref:`Short Summary <py-docstring-short-summary>`
2. :ref:`Deprecation Warning <py-docstring-deprecation>` (if applicable)
3. :ref:`Extended Summary <py-docstring-extended-summary>` (optional)
4. :ref:`Parameters <py-docstring-parameters>` (if applicable; for classes, methods, and functions)
5. :ref:`Returns <py-docstring-returns>` or :ref:`Yields <py-docstring-yields>` (if applicable; for functions, methods, and generators)
6. :ref:`Other Parameters <py-docstring-other-parameters>` (if applicable; for classes, methods, and functions)
7. :ref:`Raises <py-docstring-raises>` (if applicable)
8. :ref:`See Also <py-docstring-see-also>` (optional)
9. :ref:`Notes <py-docstring-notes>` (optional)
10. :ref:`References <py-docstring-references>` (optional)
11. :ref:`Examples <py-docstring-examples>` (optional)

For summaries of how these docstring sections are composed in specific contexts, see:

- :ref:`py-docstring-module-structure`
- :ref:`py-docstring-class-structure`
- :ref:`py-docstring-method-function-structure`
- :ref:`py-docstring-attribute-constants-structure`
- :ref:`py-docstring-property-structure`

.. _py-docstring-short-summary:

Short Summary
-------------

A one-sentence summary that does not use variable names or the function's name:

.. code-block:: python

   def add(a, b):
       """Sum two numbers.
       """
       return a + b

For functions and methods, write in the imperative voice.
That is, the summary is treated a command that the API consumer can give.
Some examples:

- ``Get metadata for all tasks.``
- ``Make an `lsst.pex.config.ConfigurableField` for this task.``
- ``Create a `Measurement` instance from a parsed YAML or JSON document.``

.. _py-docstring-deprecation:

Deprecation Warning
-------------------

A section (where applicable) to warn users that the object is deprecated.
Section contents should include:

1. In what stack version the object was deprecated, and when it will be removed.
2. Reason for deprecation if this is useful information (for example, the object is superseded, or duplicates functionality found elsewhere).
3. New recommended way of obtaining the same functionality.

This section should use the ``note`` Sphinx directive instead of an underlined section header.

.. code-block:: rst

   .. note:: Deprecated in 11_0
             `ndobj_old` will be removed in 12_0, it is replaced by
             `ndobj_new` because the latter works also with array subclasses.

.. _py-docstring-extended-summary:

Extended Summary
----------------

A few sentences giving an extended description.
This section should be used to clarify *functionality*, not to discuss implementation detail or background theory, which should rather be explored in the :ref:`'Notes' <py-docstring-notes>` section below.
You may refer to the parameters and the function name, but parameter descriptions still belong in the :ref:`'Parameters' <py-docstring-parameters>` section.

.. _py-docstring-parameters:

Parameters
----------

*For functions, methods and classes.*

'Parameters' is a description of a function or method's arguments and their respective types.
Parameters should be listed in the same order as they appear in the function or method signature.

For example:

.. code-block:: python

   def calcDistance(x, y, x0=0., y0=0.):
       """Calculate the distance between two points.

       Parameters
       ----------
       x : `float`
           X-axis coordinate.
       y : `float`
           Y-axis coordinate.
       x0 : `float`, optional
           X-axis coordinate for the second point (the origin, by default).
       y0 : `float`, optional
           Y-axis coordinate for the second point (the origin, by default).
       """

Each parameter is declared with a line formatted as ``{name} : {type}`` that is justified to the docstring.
A single space is required before and after the colon (``:``).
The ``name`` corresponds to the variable name in the function or method's arguments.
The ``type`` is described below (:ref:`py-docstring-parameter-types`).
The description is indented by **four** spaces relative to the docstring and appears without a preceding blank line.

Normally parameters are documented consecutively, without blank lines between (see the earlier example).
However, if the descriptions of an individual parameter span multiple paragraphs, or include lists, then you must separate each parameter with a blank line.
For example:

.. code-block:: rst

   Parameters
   ----------
   output_path : `str`
       Filepath where the plot will be saved.

   plot_settings : `dict`, optional
       Settings for the plot that may include these fields:

       - ``'dpi'``: resolution of the plot in dots per inch (`int`).
       - ``'rasterize'``: if `True`, then rasterize the plot. `False` by default.

.. _py-docstring-parameter-types:

Describing Parameter Types
^^^^^^^^^^^^^^^^^^^^^^^^^^

Be as precise as possible when describing parameter types.
The type description is free-form text, making it possible to list several supported types or indicate nuances.
Complex and lengthy type descriptions can be partially moved to the parameter's *description* field.
The following sections will help you deal with the different kinds of types commonly seen.

Concrete types
""""""""""""""

Wrap concrete types in backticks (in docstrings, single backticks are equivalent to ``:py:obj:``) to make a link to either an internal API or an external API that is supported by `intersphinx <http://www.sphinx-doc.org/en/stable/ext/intersphinx.html>`_.
This works for both built-in types and most importable objects:

.. code-block:: rst

   Parameters
   ----------
   filename : `str`
       [...]
   n : `int`
       [...]
   verbose : `bool`
       [...]
   items : `list` or `tuple`
       [...]
   magnitudes : `numpy.ndarray`
       [...]
   struct : `lsst.pipe.base.Struct`
       [...]

In general, provide the full namespace to the object, such as ```lsst.pipe.base.Struct```.
It may be possible to reference objects in the same namespace as the current module without any namespace prefix.
Always check the compiled documentation site to ensure the link worked.

Choices
"""""""

When a parameter can only assume one of a fixed set of values, those choices can be listed in braces:

.. code-block:: rst

   order : {'C', 'F', 'A'}
       [...]

Sequence types
""""""""""""""

When a type is a sequence container (like a `list` or `tuple`), you can describe the type of the contents.
For example:

.. code-block:: rst

   mags : `list` of `float`
       Sequence of magnitudes.

Dictionary types
""""""""""""""""

For dictionaries it is usually best to document the keys and their values in the parameter's description:

.. code-block:: rst

   settings : `dict`
       Settings dictionary with fields:

       - ``color``: Hex colour code (`str`).
       - ``size``: Point area in pixels (`float`).

Array types
"""""""""""

For Numpy arrays, try to include the dimensionality:

.. code-block:: rst

   coords : `numpy.ndarray`, (N, 2)
       [...]
   flags : `numpy.ndarray`, (N,)
       [...]
   image : `numpy.ndarray`, (Ny, Nx)
       [...]

Choose conventional variables or labels to describe dimensions, like ``N`` for the number of sources or ``Nx, Ny`` for rectangular dimensions.

Callable types
""""""""""""""

For callback functions, describe the type as ``callable``:

.. code-block:: rst

   likelihood : callable
       Likelihood function that takes two positional arguments:

       - ``x``: current parameter (`float`).
       - ``extra_args``: additional arguments (`dict`).

.. _py-docstring-optional:

Optional Parameters
^^^^^^^^^^^^^^^^^^^

For keyword arguments with useful defaults, add ``optional`` to the type specification:

.. code-block:: rst

   x : `int`, optional

Optional keyword parameters have default values, which are automatically documented as part of the function or method's signature.
You can also explain defaults in the description:

.. code-block:: rst

   x : `int`, optional
       Description of parameter ``x`` (the default is -1, which implies summation
       over all axes).

.. _py-docstring-shorthand:

Shorthand
^^^^^^^^^

When two or more consecutive input parameters have exactly the same type, shape and description, they can be combined:

.. code-block:: rst

   x1, x2 : array-like
       Input arrays, description of `x1`, `x2`.

.. _py-docstring-returns:

Returns
-------

*For functions and methods*.

'Returns' is an explanation of the returned values and their types, in the same format as :ref:`'Parameters' <py-docstring-parameters>`.

Basic example
^^^^^^^^^^^^^

If a sequence of values is returned, each value may be separately listed, in order:

.. code-block:: python

   def getCoord(self):
       """Get the point's pixel coordinate.

       Returns
       -------
       x : `int`
           X-axis pixel coordinate.
       y : `int`
           Y-axis pixel coordinate.
       """
       return self._x, self._y

Dictionary return types
^^^^^^^^^^^^^^^^^^^^^^^

If a return type is `dict`, ensure that the key-value pairs are documented in the description:

.. code-block:: python

   def getCoord(self):
       """Get the point's pixel coordinate.

       Returns
       -------
       pixelCoord : `dict`
          Pixel coordinates with fields:

          - ``x``: x-axis coordinate (`int`).
          - ``y``: y-axis coordinate (`int`).
        """
        return {'x': self._x, 'y': self._y}


Struct return types
^^^^^^^^^^^^^^^^^^^

``lsst.pipe.base.Struct``\ s, returned by Tasks for example, are documented the same way as dictionaries:

.. code-block:: python

   def getCoord(self):
       """Get the point's pixel coordinate.

       Returns
       -------
       result : `lsst.pipe.base.Struct`
          Result struct with components:

          - ``x``: x-axis coordinate (`int`).
          - ``y``: y-axis coordinate (`int`).
        """
        return lsst.pipe.base.Struct(x=self._x, y=self._y)

Naming return variables
^^^^^^^^^^^^^^^^^^^^^^^

Note that the names of the returned variables do not necessarily correspond to the names of variables.
In the previous examples, the variables ``x``, ``y``, and ``pixelCoord`` never existed in the method scope.
Simply choose a variable-like name that is clear.
Order is important.

If a returned variable is named in the method or function scope, you will usually want to use that name for clarity.
For example:

.. code-block:: python

   def getDistance(self, x, y):
       """Compute the distance of the point to an (x, y) coordinate.

       [...]

       Returns
       -------
       distance : `float`
           Distance, in units of pixels.
       """
       distance = np.hypot(self._x - x, self._y - y)
       return distance

.. _py-docstring-yields:

Yields
------

*For generators.*

'Yields' is used identically to :ref:`'Returns' <py-docstring-yields>`, but for generators.
For example:

.. code-block:: python

   def items(self):
       """Iterate over items in the container.

       Yields
       ------
       key : `str`
           Item key.
       value : obj
           Item value.
       """
       for key, value in self._data.items():
           yield key, value

.. _py-docstring-other-parameters:

Other Parameters
----------------

*For classes, methods and functions.*

'Other Parameters' is an optional section used to describe infrequently used parameters.
It should only be used if a function has a large number of keyword parameters, to prevent cluttering the :ref:`Parameters <py-docstring-parameters>` section.
In practice, this section is seldom used.

.. _py-docstring-raises:

Raises
------

*For classes, methods and functions.*

'Raises' is an optional section for describing the exceptions that can be raised.
You usually cannot document all possible exceptions that might get raised by the entire call stack.
Instead, focus on:

- Exceptions that are commonly raised.
- Exceptions that are unique (custom exceptions, in particular).
- Exceptions that are important to using an API.

The 'Raises' section looks like this:

.. code-block:: rst

   Raises
   ------
   IOError
       Raised if the input file cannot be read.
   TypeError
       Raised if parameter ``example`` is an invalid type.

Don't wrap each exception's name with backticks, as we do when describing types in :ref:`Parameters <py-docstring-parameters>` and :ref:`Returns <py-docstring-returns>`).
No namespace prefix is needed when referring to exceptions in the same module as the API.
Providing the full namespace is often a good idea, though.

The description text is indented by four spaces from the docstring's left justification.
Like the description fields for :ref:`Parameters <py-docstring-parameters>` and :ref:`Returns <py-docstring-returns>`, the description can consist of multiple paragraphs and lists.

Stylistically, write the first sentence of each description in the form:

.. code-block:: text

   Raised if [insert circumstance].

.. _py-docstring-see-also:

See Also
--------

Use the 'See also' section to link to related APIs that the user may not be aware of, or may not easily discover from other parts of the docstring.
Here are some good uses of the 'See also' section:

- If a function wraps another function, you may want to reference the lower-level function.
- If a function is typically used with another API, you can reference that API.
- If there is a family of closely related APIs, you might link to others in the family so a user can compare and choose between them easily.

As an example, for a function such as ``numpy.cos``, we would have:

.. code-block:: rst

   See also
   --------
   sin
   tan

Numpydoc assumes that the contents of the 'See also' section are API names, so don't wrap each name with backticks, as we do when describing types in :ref:`Parameters <py-docstring-parameters>` and :ref:`Returns <py-docstring-returns>`).
No namespace prefix is needed when referring to functions in the same module.
Providing the full namespace is always safe, though, and provides clarity to fellow developers:

.. code-block:: rst

   See also
   --------
   numpy.sin
   numpy.tan

.. _py-docstring-notes:

Notes
-----

*Notes* is an optional section that provides additional information about the code, possibly including a discussion of the algorithm.
Most reStructuredText formatting is allowed in the Notes section, including:

- :ref:`Lists <rst-lists>`
- :ref:`Tables <rst-tables>`
- :ref:`Images <rst-figures>`
- :ref:`Inline and display math <rst-math>`
- :ref:`Links <rst-linking>`

When using images, remember that many developers and users will be reading the docstring in its raw source form.
Images should add information, but the docstring should still be useful and complete without them.

See also :ref:`py-docstring-rst` for restrictions.

.. _py-docstring-references:

References
----------

References cited in the :ref:`'Notes' <py-docstring-notes>` section are listed here.
For example, if you cited an article using the syntax ``[1]_``, include its reference as follows:

.. code-block:: rst

   References
   ----------
   .. [1] O. McNoleg, "The integration of GIS, remote sensing,
      expert systems and adaptive co-kriging for environmental habitat
      modelling of the Highland Haggis using object-oriented, fuzzy-logic
      and neural-network techniques," Computers & Geosciences, vol. 22,
      pp. 585-588, 1996.

Web pages should be referenced with regular inline links.

References are meant to augment the docstring, but should not be required to understand it.
References are numbered, starting from one, in the order in which they are cited.

.. note::

   In the future we may support `bibtex-based references instead <https://github.com/mcmtroffaes/sphinxcontrib-bibtex>`__ instead of explicitly writing bibliographies in docstrings.

.. _py-docstring-examples:

Examples
--------

'Examples' is an optional section for usage examples written in the `doctest <http://docs.python.org/library/doctest.html>`_ format.
These examples do not replace unit tests, but *are* intended to be tested to ensure documentation and code are consistent.
While optional, this section is useful for users and developers alike.

When multiple examples are provided, they should be separated by blank lines.
Comments explaining the examples should have blank lines both above and below them:

.. code-block:: rst

   Examples
   --------
   A simple example:

   >>> np.add(1, 2)
   3

   Comment explaining the second example:

   >>> np.add([1, 2], [3, 4])
   array([4, 6])

For tests with a result that is random or platform-dependent, mark the output as such:

.. code-block:: rst

   Examples
   --------
   An example marked as creating a random result:

   >>> np.random.rand(2)
   array([ 0.35773152,  0.38568979])  #random

It is not necessary to use the doctest markup ``<BLANKLINE>`` to indicate empty lines in the output.

For more information on doctest, see:

- `The official doctest documentation <http://docs.python.org/library/doctest.html>`__.
- `doctest — Testing Through Documentation <https://pymotw.com/3/doctest/>`__ from Python Module of the Week.

.. _py-docstring-module-structure:

Documenting Modules
===================

Sections in Module Docstrings
-----------------------------

Module docstrings contain the following sections:

1. :ref:`Short Summary <py-docstring-short-summary>`
2. :ref:`Deprecation Warning <py-docstring-deprecation>` (if applicable)
3. :ref:`Extended Summary <py-docstring-extended-summary>` (optional)
4. :ref:`See Also <py-docstring-see-also>` (optional)

.. note::

   Module docstrings aren't featured heavily in the documentation we generate and publish with Sphinx.
   Avoid putting important end-user documentation in module docstrings.
   Instead, write introductory and overview documentation in the module's *user guide* (the :file:`doc/` directories of Stack packages).

   Module docstrings can still be useful for developer-oriented notes, though.

Placement of Module Docstrings
------------------------------

Module-level docstrings must be placed as close to the top of the Python file as possible: *below* any ``#!/usr/bin/env python`` and license statements, but *above* imports.
See also: :ref:`style-guide-py-file-order`.

Module docstrings should not be indented.
For example:

.. code-block:: python

   #
   # This file is part of dm_dev_guide.
   #
   # Developed for the LSST Data Management System.
   # This product includes software developed by the LSST Project
   # (http://www.lsst.org).
   # See the COPYRIGHT file at the top-level directory of this distribution
   # for details of code ownership.
   #
   # This program is free software: you can redistribute it and/or modify
   # it under the terms of the GNU General Public License as published by
   # the Free Software Foundation, either version 3 of the License, or
   # (at your option) any later version.
   #
   # This program is distributed in the hope that it will be useful,
   # but WITHOUT ANY WARRANTY; without even the implied warranty of
   # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   # GNU General Public License for more details.
   #
   # You should have received a copy of the GNU General Public License
   # along with this program.  If not, see <http://www.gnu.org/licenses/>.
   #
   """Summary of MyModule.

   Extended discussion of my module.
   """

   import lsst.afw.table as afwTable
   # [...]

.. _py-docstring-class-structure:

Documenting Classes
===================

Class docstrings are placed directly after the class definition, and serve to document both the class as a whole *and* the arguments passed to the ``__init__`` constructor.

Sections in Class Docstrings
----------------------------

Class docstrings contain the following sections:

1. :ref:`Short Summary <py-docstring-short-summary>`
2. :ref:`Deprecation Warning <py-docstring-deprecation>` (if applicable)
3. :ref:`Extended Summary <py-docstring-extended-summary>` (optional)
4. :ref:`Parameters <py-docstring-parameters>` (if applicable)
5. :ref:`Other Parameters <py-docstring-other-parameters>` (if applicable)
6. :ref:`Raises <py-docstring-raises>` (if applicable)
7. :ref:`See Also <py-docstring-see-also>` (optional)
8. :ref:`Notes <py-docstring-notes>` (optional)
9. :ref:`References <py-docstring-references>` (optional)
10. :ref:`Examples <py-docstring-examples>` (optional)

Placement of Class Docstrings
-----------------------------

Class docstrings must be placed directly below the declaration, and indented according to the code scope:

.. code-block:: python

   class MyClass(object):
       """Summary of MyClass.

       Parameters
       ----------
       a : `str`
          Documentation for the ``a`` parameter.
       """

       def __init__(self, a):
           pass

The ``__init__`` method never has a docstring since the class docstring documents the constructor.

Examples of Class Docstrings
----------------------------

Here's an example of a more comprehensive class docstring with :ref:`Short Summary <py-docstring-short-summary>`, :ref:`Parameters <py-docstring-parameters>`, :ref:`Raises <py-docstring-raises>`, :ref:`See Also <py-docstring-see-also>`, and :ref:`Examples <py-docstring-examples>` sections:

.. code-block:: python

   class SkyCoordinate(object):
       """Equatorial coordinate on the sky as Right Ascension and Declination.

       Parameters
       ----------
       ra : `float`
          Right ascension (degrees).
       dec : `float`
          Declination (degrees).
       frame : {'icrs', 'fk5'}, optional
          Coordinate frame.

       Raises
       ------
       ValueError
           Raised when input angles are outside range.

       See also
       --------
       lsst.example.GalacticCoordinate

       Examples
       --------
       To define the coordinate of the M31 galaxy:

       >>> m31_coord = SkyCoordinate(10.683333333, 41.269166667)
       SkyCoordinate(10.683333333, 41.269166667, frame='icrs')
       """

       def __init__(self, ra, dec, frame='icrs'):
           pass

.. _py-docstring-method-function-structure:

Documenting Methods and Functions
=================================

Sections in Method and Function Docstring Sections
--------------------------------------------------

Method and function docstrings contain the following sections:

1. :ref:`Short Summary <py-docstring-short-summary>`
2. :ref:`Deprecation Warning <py-docstring-deprecation>` (if applicable)
3. :ref:`Extended Summary <py-docstring-extended-summary>` (optional)
4. :ref:`Parameters <py-docstring-parameters>` (if applicable)
5. :ref:`Returns <py-docstring-returns>` or :ref:`Yields <py-docstring-yields>` (if applicable)
6. :ref:`Other Parameters <py-docstring-other-parameters>` (if applicable)
7. :ref:`Raises <py-docstring-raises>` (if applicable)
8. :ref:`See Also <py-docstring-see-also>` (optional)
9. :ref:`Notes <py-docstring-notes>` (optional)
10. :ref:`References <py-docstring-references>` (optional)
11. :ref:`Examples <py-docstring-examples>` (optional)

Placement of Module and Function Docstrings
-------------------------------------------

Class, method, and function docstrings must be placed directly below the declaration, and indented according to the code scope:

.. code-block:: python

   class MyClass(object):
       """Summary of MyClass.

       Extended discussion of MyClass.
       """

       def __init__(self):
           pass

       def myMethod(self):
           """Summary of method.

           Extended Discussion of myMethod.
           """
           pass


   def my_function():
       """Summary of my_function.

       Extended discussion of my_function.
       """
       pass

Again, the :ref:`class docstring <py-docstring-class-structure>` takes the place of a docstring for the ``__init__`` method.
``__init__`` methods don't have docstrings.

Dunder Methods
--------------

Special "dunder" methods on classes only need to have docstrings if they are doing anything non-standard.
For example, if a ``__getslice__`` method cannot take negative indices, that should be noted.
But if ``__ge__`` returns true if ``self`` is greater than or equal to the argument, that need not be documented.

Examples of Method and Function Docstrings
------------------------------------------

Here's an example function:

.. code-block:: python

   def check_unit(self, quantity):
       """Check that a `~astropy.units.Quantity` has equivalent units to
       this metric.

       Parameters
       ----------
       quantity : `astropy.units.Quantity`
           Quantity to be tested.

       Returns
       -------
       is_equivalent : `bool`
           `True` if the units are equivalent, meaning that the quantity
           can be presented in the units of this metric. `False` if not.

       See also
       --------
       astropy.units.is_equivalent

       Examples
       --------
       Check that a quantity in arcseconds is compatible with a metric defined in arcminutes:

       >>> import astropy.units as u
       >>> from lsst.verify import Metric
       >>> metric = Metric('example.test', 'Example', u.arcminute)
       >>> metric.check_units(1.*u.arcsecond)
       True

       But mags are not a compatible unit:

       >>> metric.check_units(21.*u.mag)
       False
       """
       if not quantity.unit.is_equivalent(self.unit):
           return False
       else:
           return True

.. _py-docstring-attribute-constants-structure:

Documenting Constants and Class Attributes
==========================================

Sections in Constant and Class Attribute Docstrings
---------------------------------------------------

Constants in modules and attributes in classes are all documented similarly.
At a minimum, they should have a summary line that includes the type.
They can also have a more complete structure with these sections:

1. :ref:`Short Summary <py-docstring-short-summary>`
2. :ref:`Deprecation Warning <py-docstring-deprecation>` (if applicable)
3. :ref:`Extended Summary <py-docstring-extended-summary>` (optional)
4. :ref:`Notes <py-docstring-notes>` (optional)
5. :ref:`References <py-docstring-references>` (optional)
6. :ref:`Examples <py-docstring-examples>` (optional)

Placement of Constant and Class Attribute Docstrings
----------------------------------------------------

Docstrings for module-level variables and class attributes appear directly below their first declaration.
For example:

.. code-block:: python
   :emphasize-lines: 1-3,10-12

   MAX_ITER = 10
   """Maximum number of iterations (`int`).
   """


   class MyClass(object):
       """Example class for documenting attributes.
       """

       x = None
       """Description of x attribute.
       """

Examples of Constant and Class Attribute Docstrings
---------------------------------------------------

Minimal constant or attribute example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Include the attribute or constant's type in parentheses at the end of the summary line:

.. code-block:: py

   NAME = 'LSST'
   """Name of the project (`str`)."""

Multi-section docstrings
^^^^^^^^^^^^^^^^^^^^^^^^

Multi-section docstrings keep the type information in the summary line.
For example:

.. code-block:: py

   PA1_DESIGN = 5. * u.mmag
   """PA1 design specification (`astropy.units.Quantity`).

   Notes
   -----
   The PA1 metric [1]_ is defined so that the rms of the unresolved source
   magnitude distribution around the mean value (repeatability) will not
   exceed PA1 millimag (median distribution for a large number of sources).

   References
   ----------
   .. [1] Z. Ivezic and the LSST Science Collaboration. 2011, LSST Science
      Requirements Document, LPM-17, URL https://ls.st/LPM-17
   """

Attributes set in \_\_init\_\_ methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In many classes, public attributes are set in the ``__init__`` method.
The best way to document these public attributes is by declaring the attribute at the class level and including a docstring with that declaration:

.. code-block:: python
   :emphasize-lines: 14-20

   class Metric(object):
       """Verification metric.

       Parameters
       ----------
       name : `str`
           Name of the metric.
       unit : `astropy.units.Unit`
           Units of the metric.
       package : `str`, optional
           Name of the package where this metric is defined.
       """

       name = None
       """Name of the metric (`str`).
       """

       unit = None
       """Units of the metric (`astropy.units.Unit`).
       """

       def __init__(self, name, unit, package=None):
           self.name = name
           self.unit = unit
           self._package = package

Notice that the :ref:`parameters <py-docstring-parameters>` to the ``__init__`` method are documented separately from the class attributes (highlighted).

.. _py-docstring-property-structure:

Documenting Class Properties
============================

Properties are documented like :ref:`class attributes <py-docstring-attribute-constants-structure>` rather than methods.
After all, properties are designed to appear to the user like simple attributes.

For example:

.. code-block:: python

   class Measurement(object):

       @property
       def quantity(self):
           """The measurement quantity (`astropy.units.Quantity`).
           """
           # ...

       @quantity.setter
       def quantity(self, q):
           # ...

       @property
       def unit(self):
           """Units of the measurement (`astropy.units.Unit`, read-only).
           """
           # ...

Note:

- Do not use the :ref:`Returns section <py-docstring-returns>` in the property's docstring.
  Instead, include type information in the summary, as is done for :ref:`class attributes <py-docstring-attribute-constants-structure>`.
- Only document the property's "getter" method, not the "setter" (if present).
- If a property does not have a "setter" method, include the words ``read-only`` after the type information.

.. _py-docstring-example-module:

Complete Example Module
=======================

.. literalinclude:: examples/numpydocExample.py
   :language: python

Acknowledgements
================

These docstring guidelines are derived/adapted from the `NumPy <https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt>`_ and `Astropy <http://docs.astropy.org/en/stable/_sources/development/docrules.txt>`_ documentation.
The example module is adapted from the `Napoleon documentation <http://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html#example-numpy>`_.

NumPy is Copyright © 2005-2013, NumPy Developers.

Astropy is Copyright © 2011-2015, Astropy Developers.

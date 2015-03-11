

Better Fonts in PDF Output
==========================

One thing I like a lot about Sphinx is that it can generate PDF files for your documentation,
complete with table of contents, chapters and section numbers.

Read the Docs also automatically generate PDF documentations for you.

But there is one thing: the styling.
While the HTML documentation looks nice and modern,
thanks to `Read the Docs' theme`_,
the PDF documentation looks ugly and dull.

.. _Read the Docs' theme: http://ericholscher.com/blog/2013/nov/4/new-theme-read-the-docs/

I think the major problem is the **typeface**.
On HTML documentation, we have *Lato* and *Roboto Slab* font,
which gives it a modern feel.
For code, various modern monospace fonts are used (*Consolas*, *Andale Mono*, and so on.)

On PDF documentation, however, the default typefaces are *Helvetica*, *Times*, and *Courier*.

**Are you serious?!** Helvetica and Times makes your documentation look ancient and uninteresting.
*Courier* is fat and thin and is kind of hard-to-read.
This needs to change.

Sphinx uses pdfLaTeX to generate PDF documentation,
and Read the Docs uses TeX Live in its build infrastructure.
With TeX Live comes `several nice fonts`_, which you can safely use in Read the Docs environment.

.. _several nice fonts: http://tex.stackexchange.com/a/59405

To use them, simply include the package, according to each font package's documentation, in the preamble option.
For example, here is my configuration::

  latex_elements = {
  # The paper size ('letterpaper' or 'a4paper').
      'papersize': 'letterpaper',

  # The font size ('10pt', '11pt' or '12pt').
      'pointsize': '11pt',

  # Additional stuff for the LaTeX preamble.
      'preamble': r'''
          \usepackage{charter}
          \usepackage[defaultsans]{lato}
          \usepackage{inconsolata}
      ''',
  }

In this configuration,
**Charter** is used as serif font,
**Lato** as sans-serif font,
and **Inconsolata** as monospace font.
Even though the colors and layout don't change,
changing the typeface can give your PDF documentation a radically different feel.
`such wow. many modern. <http://readthedocs.org/projects/protips/downloads/pdf/latest/>`_

:Date:   2015-03-11
:Author: Thai Pangsakulyanont


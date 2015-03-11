
Creating Custom Link Roles
==========================

I think it'd be cool -- especially in the developer's documentation -- to link to the corresponding source code on GitHub.

In my project's docs, inside the architecture section, for example,
describes the directory structure in the project.
Each item links to corresponding GitHub source code:

  **Directory Structure**

  :tree:`assets`
    Image assets for use in the game.
    These assets can be referred from webpack code by ``require('assets/...')``.
  :tree:`bin`
    Useful scripts for routine work.
    Examples include setting up Git commit hooks and releasing a new version.
  :tree:`config`
    Configuration code for webpack and other things.

And the corresponding source code::

  **Directory Structure**

  :tree:`assets`
    Image assets for use in the game.
    These assets can be referred from webpack code by ``require('assets/...')``.
  :tree:`bin`
    Useful scripts for routine work.
    Examples include setting up Git commit hooks and releasing a new version.
  :tree:`config`
    Configuration code for webpack and other things.


As you can see, a custom role, ``:tree:``, is created to link to source code.
To do that, you have to write a Sphinx extension.

Fortunately, this is easy.
There are tutorials on `defining custom roles in Sphinx`_ and a more in-depth guide about `creating a text role from Docutils`_.

.. _defining custom roles in Sphinx:    http://doughellmann.com/2010/05/09/defining-custom-roles-in-sphinx.html
.. _creating a text role from Docutils: http://docutils.sourceforge.net/docs/howto/rst-roles.html

First, you have to add a custom search path to ``conf.py``, so that your extension may be loaded::

  sys.path.insert(0, os.path.abspath('.') + '/_extensions')

Next, create the extension file. I call it ``_extensions/bemuse.py``.
It looks like this::

  from docutils import nodes

  def setup(app):
      app.add_role('github', autolink('https://github.com/%s'))
      app.add_role('module', autolink('https://github.com/bemusic/bemuse/tree/master/src/%s'))
      app.add_role('tree', autolink('https://github.com/bemusic/bemuse/tree/master/%s'))

  def autolink(pattern):
      def role(name, rawtext, text, lineno, inliner, options={}, content=[]):
          url = pattern % (text,)
          node = nodes.reference(rawtext, text, refuri=url, **options)
          return [node], []
      return role

The ``setup`` function is called by Sphinx for an extension to add custom roles.

Since each role does similar thing: turn texts into links using some predefined pattern, I created a function, called ``autolink``, that returns a function that turns text into link using some specified pattern.

Finally, add that extension to the configuration file::

  # Add any Sphinx extension module names here, as strings. They can be
  # extensions coming with Sphinx (named 'sphinx.ext.*') or your custom
  # ones.
  extensions = ['bemuse']

:Date:   2015-03-11
:Author: Thai Pangsakulyanont


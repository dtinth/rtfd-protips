
:Authors: - Thai Pangsakulyanont

Inferring Release Number from Git Tags
=======================================

When starting a Sphinx documentation using ``sphinx-quickstart``,
it asks you for version number of the docs.
This already makes me question about the maintenance burden.

The version number is already stored in the project as Git tags and ``package.json``
(but npm has a built-in command to increment the version number and also create a Git tag).
Having to specify the version number in Sphinx documentation doesn't seem right to me.

After finding out that the configuration file is just a Python script,
I modified it to infer the version number from Git tags::

    import re
    # The full version, including alpha/beta/rc tags.
    release = re.sub('^v', '', os.popen('git describe').read().strip())
    # The short X.Y version.
    version = release

This code uses ``git describe`` to generate a version number based on current Git commit.
If we are on a tag, such as ``v0.10.1``, git describe returns that tag name::

    $ git describe
    v0.10.1

However, if we are not on the tag,
``git describe`` will find the closest tag to this commit,
and append the number of commits since that tag, along with the commit ID::

    $ git describe
    v0.10.1-53-gd8be3ff

So there you have it.
When building, the version number will describe exactly what commit the documentation is being built for.


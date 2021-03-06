Getting started
###############

Installing Pelican
==================

Pelican currently runs best on Python 2.7.x; earlier versions of Python are
not supported. There is provisional support for Python 3.3, although there may
be rough edges, particularly with regards to optional 3rd-party components.

You can install Pelican via several different methods. The simplest is via
`pip <http://www.pip-installer.org/>`_::

    $ pip install pelican

If you don't have ``pip`` installed, an alternative method is
``easy_install``::

    $ easy_install pelican

(Keep in mind that operating systems will often require you to prefix the above
commands with ``sudo`` in order to install Pelican system-wide.)

While the above is the simplest method, the recommended approach is to create
a virtual environment for Pelican via virtualenv_ before installing Pelican.
Assuming you have virtualenv_ installed, you can then open a new terminal
session and create a new virtual environment for Pelican::

    $ virtualenv ~/virtualenvs/pelican
    $ cd ~/virtualenvs/pelican
    $ . bin/activate

Once the virtual environment has been created and activated, Pelican can be
be installed via ``pip install pelican`` as noted above. Alternatively, if
you have the project source, you can install Pelican using the distutils
method::

    $ cd path-to-Pelican-source
    $ python setup.py install

If you have Git installed and prefer to install the latest bleeding-edge
version of Pelican rather than a stable release, use the following command::

    $ pip install -e git+https://github.com/getpelican/pelican.git#egg=pelican

If you plan on using Markdown as a markup format, you'll need to install the
Markdown library as well::

    $ pip install Markdown

If you want to use AsciiDoc_ you need to install it from `source
<http://www.methods.co.nz/asciidoc/INSTALL.html>`_ or use your operating
system's package manager.

Basic usage
-----------

Once Pelican is installed, you can use it to convert your Markdown or reST
content into HTML via the ``pelican`` command, specifying the path to your
content and (optionally) the path to your settings file::

$ pelican /path/to/your/content/ [-s path/to/your/settings.py]

The above command will generate your site and save it in the ``output/``
folder, using the default theme to produce a simple site. The default theme
consists of very simple HTML without styling and is provided so folks may use
it as a basis for creating their own themes.

You can also tell Pelican to watch for your modifications, instead of
manually re-running it every time you want to see your changes. To enable this,
run the ``pelican`` command with the ``-r`` or ``--autoreload`` option.

Pelican has other command-line switches available. Have a look at the help to
see all the options you can use::

    $ pelican --help

Continue reading below for more detail, and check out the Pelican wiki's
`Tutorials <https://github.com/getpelican/pelican/wiki/Tutorials>`_ page for
links to community-published tutorials.

Viewing the generated files
---------------------------

The files generated by Pelican are static files, so you don't actually need
anything special to view them. You can use your browser to open the generated
HTML files directly::

    firefox output/index.html

Because the above method may have trouble locating your CSS and other linked
assets, running a simple web server using Python will often provide a more
reliable previewing experience::

    cd output && python -m SimpleHTTPServer

Once the ``SimpleHTTPServer`` has been started, you can preview your site at
http://localhost:8000/

Upgrading
---------

If you installed a stable Pelican release via ``pip`` or ``easy_install`` and
wish to upgrade to the latest stable release, you can do so by adding
``--upgrade`` to the relevant command. For pip, that would be::

    $ pip install --upgrade pelican

If you installed Pelican via distutils or the bleeding-edge method, simply
perform the same step to install the most recent version.

Dependencies
------------

When Pelican is installed, the following dependent Python packages should be
automatically installed without any action on your part:

* `feedgenerator <http://pypi.python.org/pypi/feedgenerator>`_, to generate the
  Atom feeds
* `jinja2 <http://pypi.python.org/pypi/Jinja2>`_, for templating support
* `pygments <http://pypi.python.org/pypi/Pygments>`_, for syntax highlighting
* `docutils <http://pypi.python.org/pypi/docutils>`_, for supporting
  reStructuredText as an input format
* `pytz <http://pypi.python.org/pypi/pytz>`_, for timezone definitions
* `blinker <http://pypi.python.org/pypi/blinker>`_, an object-to-object and
  broadcast signaling system
* `unidecode <http://pypi.python.org/pypi/Unidecode>`_, for ASCII
  transliterations of Unicode text
* `six <http://pypi.python.org/pypi/six>`_,  for Python 2 and 3 compatibility
  utilities
* `MarkupSafe <http://pypi.python.org/pypi/MarkupSafe>`_, for a markup safe
  string implementation

If you want the following optional packages, you will need to install them
manually via ``pip``:

* `markdown <http://pypi.python.org/pypi/Markdown>`_, for supporting Markdown as
  an input format
* `typogrify-web <http://pypi.python.org/pypi/typogrify-web>`_, for
  typographical enhancements

Kickstart your site
===================

Once Pelican has been installed, you can create a skeleton project via the
``pelican-quickstart`` command, which begins by asking some questions about
your site::

    $ pelican-quickstart

Once you finish answering all the questions, your project will consist of the
following hierarchy (except for "pages", which you can optionally add yourself
if you plan to create non-chronological content)::

    yourproject/
    ├── content
    │   └── (pages)
    ├── output
    ├── develop_server.sh
    ├── fabfile.py
    ├── Makefile
    ├── pelicanconf.py       # Main settings file
    └── publishconf.py       # Settings to use when ready to publish

The next step is to begin to adding content to the *content* folder that has
been created for you. (See the **Writing content using Pelican** section below
for more information about how to format your content.)

Once you have written some content to generate, you can use the ``pelican``
command to generate your site, which will be placed in the output folder.

Automation tools
================

While the ``pelican`` command is the canonical way to generate your site,
automation tools can be used to streamline the generation and publication
flow. One of the questions asked during the ``pelican-quickstart`` process
described above pertains to whether you want to automate site generation and
publication. If you answered "yes" to that question, a ``fabfile.py`` and
``Makefile`` will be generated in the root of your project. These files,
pre-populated with certain information gleaned from other answers provided
during the ``pelican-quickstart`` process, are meant as a starting point and
should be customized to fit your particular needs and usage patterns. If you
find one or both of these automation tools to be of limited utility, these
files can deleted at any time and will not affect usage of the canonical
``pelican`` command.

Following are automation tools that "wrap" the ``pelican`` command and can
simplify the process of generating, previewing, and uploading your site.

Fabric
------

The advantage of Fabric_ is that it is written in Python and thus can be used
in a wide range of environments. The downside is that it must be installed
separately. Use the following command to install Fabric, prefixing with
``sudo`` if your environment requires it::

    $ pip install Fabric

Take a moment to open the ``fabfile.py`` file that was generated in your
project root. You will see a number of commands, any one of which can be
renamed, removed, and/or customized to your liking. Using the out-of-the-box
configuration, you can generate your site via::

    $ fab build

If you'd prefer to have Pelican automatically regenerate your site every time a
change is detected (which is handy when testing locally), use the following
command instead::

    $ fab regenerate

To serve the generated site so it can be previewed in your browser at
http://localhost:8000/::

    $ fab serve

If during the ``pelican-quickstart`` process you answered "yes" when asked
whether you want to upload your site via SSH, you can use the following command
to publish your site via rsync over SSH::

    $ fab publish

These are just a few of the commands available by default, so feel free to
explore ``fabfile.py`` and see what other commands are available. More
importantly, don't hesitate to customize ``fabfile.py`` to suit your specific
needs and preferences.

Make
----

A ``Makefile`` is also automatically created for you when you say "yes" to
the relevant question during the ``pelican-quickstart`` process. The advantage
of this method is that the ``make`` command is built into most POSIX systems
and thus doesn't require installing anything else in order to use it. The
downside is that non-POSIX systems (e.g., Windows) do not include ``make``,
and installing it on those systems can be a non-trivial task.

If you want to use ``make`` to generate your site, run::

    $ make html

If you'd prefer to have Pelican automatically regenerate your site every time a
change is detected (which is handy when testing locally), use the following
command instead::

    $ make regenerate

To serve the generated site so it can be previewed in your browser at
http://localhost:8000/::

    $ make serve

Normally you would need to run ``make regenerate`` and ``make serve`` in two
separate terminal sessions, but you can run both at once via::

    $ make devserver

The above command will simultaneously run Pelican in regeneration mode as well
as serve the output at http://localhost:8000. Once you are done testing your
changes, you should stop the development server via::

    $ ./develop_server.sh stop

When you're ready to publish your site, you can upload it via the method(s) you
chose during the ``pelican-quickstart`` questionnaire. For this example, we'll
use rsync over ssh::

    $ make rsync_upload

That's it! Your site should now be live.

(The default ``Makefile`` and ``devserver.sh`` scripts use the ``python`` and
``pelican`` executables to complete its tasks. If you want to use different
executables, such as ``python3``, you can set the ``PY`` and ``PELICAN``
environment variables, respectively, to override the default executable names.)


Writing content using Pelican
=============================

Articles and pages
------------------

Pelican considers "articles" to be chronological content, such as posts on a
blog, and thus associated with a date.

The idea behind "pages" is that they are usually not temporal in nature and are
used for content that does not change very often (e.g., "About" or "Contact"
pages).

.. _internal_metadata:

File metadata
-------------

Pelican tries to be smart enough to get the information it needs from the
file system (for instance, about the category of your articles), but some
information you need to provide in the form of metadata inside your files.

If you are writing your content in reStructuredText format, you can provide
this metadata in text files via the following syntax (give your file the
``.rst`` extension)::

    My super title
    ##############

    :date: 2010-10-03 10:20
    :modified: 2010-10-04 18:40
    :tags: thats, awesome
    :category: yeah
    :slug: my-super-post
    :author: Alexis Metaireau
    :summary: Short version for index and feeds

Pelican implements an extension to reStructuredText to enable support for the
``abbr`` HTML tag. To use it, write something like this in your post::

    This will be turned into :abbr:`HTML (HyperText Markup Language)`.

You can also use Markdown syntax (with a file ending in ``.md``,
``.markdown``, ``.mkd``, or ``.mdown``). Markdown generation requires that you
first explicitly install the ``Markdown`` package, which can be done via ``pip
install Markdown``. Metadata syntax for Markdown posts should follow this
pattern::

    Title: My super title
    Date: 2010-12-03 10:20
    Modified: 2010-12-05 19:30
    Category: Python
    Tags: pelican, publishing
    Slug: my-super-post
    Author: Alexis Metaireau
    Summary: Short version for index and feeds

    This is the content of my super blog post.

Conventions for AsciiDoc_ posts, which should have an ``.asc`` extension, can
be found on the AsciiDoc_ site.

Pelican can also process HTML files ending in ``.html`` and ``.htm``. Pelican
interprets the HTML in a very straightforward manner, reading metadata from
``meta`` tags, the title from the ``title`` tag, and the body out from the
``body`` tag::

    <html>
        <head>
            <title>My super title</title>
            <meta name="tags" content="thats, awesome" />
            <meta name="date" content="2012-07-09 22:28" />
            <meta name="modified" content="2012-07-10 20:14" />
            <meta name="category" content="yeah" />
            <meta name="author" content="Alexis Métaireau" />
            <meta name="summary" content="Short version for index and feeds" />
        </head>
        <body>
            This is the content of my super blog post.
        </body>
    </html>

With HTML, there is one simple exception to the standard metadata: ``tags`` can
be specified either via the ``tags`` metadata, as is standard in Pelican, or
via the ``keywords`` metadata, as is standard in HTML. The two can be used
interchangeably.

Note that, aside from the title, none of this article metadata is mandatory:
if the date is not specified and ``DEFAULT_DATE`` is set to ``fs``, Pelican
will rely on the file's "mtime" timestamp, and the category can be determined
by the directory in which the file resides. For example, a file located at
``python/foobar/myfoobar.rst`` will have a category of ``foobar``. If you would
like to organize your files in other ways where the name of the subfolder would
not be a good category name, you can set the setting ``USE_FOLDER_AS_CATEGORY``
to ``False``.  When parsing dates given in the page metadata, Pelican supports
the W3C's `suggested subset ISO 8601`__.

__ `W3C ISO 8601`_

``modified`` should be last time you updated the article, and defaults to ``date`` if not specified.
Besides you can show ``modified`` in the templates, feed entries in feed readers will be updated automatically
when you set ``modified`` to the current date after you modified your article.

If you do not explicitly specify summary metadata for a given post, the
``SUMMARY_MAX_LENGTH`` setting can be used to specify how many words from the
beginning of an article are used as the summary.

You can also extract any metadata from the filename through a regular
expression to be set in the ``FILENAME_METADATA`` setting. All named groups
that are matched will be set in the metadata object. The default value for the
``FILENAME_METADATA`` setting will only extract the date from the filename. For
example, if you would like to extract both the date and the slug, you could set
something like: ``'(?P<date>\d{4}-\d{2}-\d{2})_(?P<slug>.*)'``

Please note that the metadata available inside your files takes precedence over
the metadata extracted from the filename.

Pages
-----

If you create a folder named ``pages`` inside the content folder, all the
files in it will be used to generate static pages, such as **About** or
**Contact** pages. (See example filesystem layout below.)

You can use the ``DISPLAY_PAGES_ON_MENU`` setting to control whether all those
pages are displayed in the primary navigation menu. (Default is ``True``.)

If you want to exclude any pages from being linked to or listed in the menu
then add a ``status: hidden`` attribute to its metadata. This is useful for
things like making error pages that fit the generated theme of your site.

.. _ref-linking-to-internal-content:

Linking to internal content
---------------------------

From Pelican 3.1 onwards, it is now possible to specify intra-site links to
files in the *source content* hierarchy instead of files in the *generated*
hierarchy. This makes it easier to link from the current post to other posts
and images that may be sitting alongside the current post (instead of having
to determine where those resources will be placed after site generation).

To link to internal content (files in the ``content`` directory), use the
following syntax: ``{filename}path/to/file``::


    website/
    ├── content
    │   ├── article1.rst
    │   ├── cat/
    │   │   └── article2.md
    │   └── pages
    │       └── about.md
    └── pelican.conf.py

In this example, ``article1.rst`` could look like::

    The first article
    #################

    :date: 2012-12-01 10:02

    See below intra-site link examples in reStructuredText format.

    `a link relative to content root <{filename}/cat/article2.rst>`_
    `a link relative to current file <{filename}cat/article2.rst>`_

and ``article2.md``::

    Title: The second article
    Date: 2012-12-01 10:02

    See below intra-site link examples in Markdown format.

    [a link relative to content root]({filename}/article1.md)
    [a link relative to current file]({filename}../article1.md)

Embedding non-article or non-page content is slightly different in that the
directories need to be specified in ``pelicanconf.py`` file. The ``images``
directory is configured for this by default but others will need to be added
manually::

    content
    ├── images
    │   └── han.jpg
    └── misc
        └── image-test.md

And ``image-test.md`` would include::

    ![Alt Text]({filename}/images/han.jpg)

Any content can be linked in this way. What happens is that the ``images``
directory gets copied to ``output/`` during site generation because Pelican
includes ``images`` in the ``STATIC_PATHS`` setting's list by default. If
you want to have another directory, say ``pdfs``, copied from your content to
your output during site generation, you would need to add the following to
your settings file::

    STATIC_PATHS = ['images', 'pdfs']

After the above line has been added, subsequent site generation should copy the
``content/pdfs/`` directory to ``output/pdfs/``.

You can also link to categories or tags, using the ``{tag}tagname`` and
``{category}foobar`` syntax.

For backward compatibility, Pelican also supports bars (``||``) in addition to
curly braces (``{}``). For example: ``|filename|an_article.rst``,
``|tag|tagname``, ``|category|foobar``. The syntax was changed from ``||`` to
``{}`` to avoid collision with Markdown extensions or reST directives.

Importing an existing blog
--------------------------

It is possible to import your blog from Dotclear, WordPress, and RSS feeds using
a simple script. See :ref:`import`.

Translations
------------

It is possible to translate articles. To do so, you need to add a ``lang`` meta
attribute to your articles/pages and set a ``DEFAULT_LANG`` setting (which is
English [en] by default). With those settings in place, only articles with the
default language will be listed, and each article will be accompanied by a list
of available translations for that article.

Pelican uses the article's URL "slug" to determine if two or more articles are
translations of one another. The slug can be set manually in the file's
metadata; if not set explicitly, Pelican will auto-generate the slug from the
title of the article.

Here is an example of two articles, one in English and the other in French.

The English article::

    Foobar is not dead
    ##################

    :slug: foobar-is-not-dead
    :lang: en

    That's true, foobar is still alive!

And the French version::

    Foobar n'est pas mort !
    #######################

    :slug: foobar-is-not-dead
    :lang: fr

    Oui oui, foobar est toujours vivant !

Post content quality notwithstanding, you can see that only item in common
between the two articles is the slug, which is functioning here as an
identifier. If you'd rather not explicitly define the slug this way, you must
then instead ensure that the translated article titles are identical, since the
slug will be auto-generated from the article title.

If you do not want the original version of one specific article to be detected
by the ``DEFAULT_LANG`` setting, use the ``translation`` metadata to specify
which posts are translations::

    Foobar is not dead
    ##################

    :slug: foobar-is-not-dead
    :lang: en
    :translation: true

    That's true, foobar is still alive!


.. _internal_pygments_options:

Syntax highlighting
-------------------

Pelican is able to provide colorized syntax highlighting for your code blocks.
To do so, you have to use the following conventions inside your content files.

For reStructuredText, use the code-block directive::

    .. code-block:: identifier

       <indented code block goes here>

For Markdown, include the language identifier just above the code block,
indenting both the identifier and code::

    A block of text.

        :::identifier
        <code goes here>

The specified identifier (e.g. ``python``, ``ruby``) should be one that
appears on the `list of available lexers <http://pygments.org/docs/lexers/>`_.

When using reStructuredText the following options are available in the
code-block directive:

=============   ============  =========================================
Option          Valid values  Description
=============   ============  =========================================
anchorlinenos   N/A           If present wrap line numbers in <a> tags.
classprefix     string        String to prepend to token class names
hl_lines        numbers       List of lines to be highlighted.
lineanchors     string        Wrap each line in an anchor using this
                              string and -linenumber.
linenos         string        If present or set to "table" output line 
                              numbers in a table, if set to
                              "inline" output them inline. "none" means
                              do not output the line numbers for this 
                              table.
linenospecial   number        If set every nth line will be given the 
                              'special' css class.
linenostart     number        Line number for the first line.
linenostep      number        Print every nth line number.
lineseparator   string        String to print between lines of code,
                              '\n' by default.
linespans       string        Wrap each line in a span using this and
                              -linenumber.
nobackground    N/A           If set do not output background color for
                              the wrapping element
nowrap          N/A           If set do not wrap the tokens at all.
tagsfile        string        ctags file to use for name definitions.
tagurlformat    string        format for the ctag links.
=============   ============  =========================================

Note that, depending on the version, your Pygments module might not have
all of these options available. Refer to the *HtmlFormatter* section of the
`Pygments documentation <http://pygments.org/docs/formatters/>`_ for more
details on each of the options.

For example, the following code block enables line numbers, starting at 153,
and prefixes the Pygments CSS classes with *pgcss* to make the names
more unique and avoid possible CSS conflicts::

    .. code-block:: identifier
        :classprefix: pgcss
        :linenos: table
        :linenostart: 153

       <indented code block goes here>

It is also possible to specify the ``PYGMENTS_RST_OPTIONS`` variable in your
Pelican settings file to include options that will be automatically applied to
every code block.

For example, if you want to have line numbers displayed for every code block
and a CSS prefix you would set this variable to::

    PYGMENTS_RST_OPTIONS = {'classprefix': 'pgcss', 'linenos': 'table'}

If specified, settings for individual code blocks will override the defaults in
your settings file.

Publishing drafts
-----------------

If you want to publish an article as a draft (for friends to review before
publishing, for example), you can add a ``Status: draft`` attribute to its
metadata. That article will then be output to the ``drafts`` folder and not
listed on the index page nor on any category or tag page.

.. _virtualenv: http://www.virtualenv.org/
.. _W3C ISO 8601: http://www.w3.org/TR/NOTE-datetime
.. _Fabric: http://fabfile.org/
.. _AsciiDoc: http://www.methods.co.nz/asciidoc/

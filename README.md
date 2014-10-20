<<<<<<< HEAD
<<<<<<< HEAD
2014-10-20
==========

SWC-OSG bootcamp materials
=======
SWC-OSG
=======
>>>>>>> 7fae064d06015c8fc97824a8d00840c945c176a8
=======
==================
Software Carpentry - Open Science Grid (SWC-OSG) Workshop 
============================

This repository's `gh-pages` branch is the starting point for a bootcamp 
website: it contains a template for the bootcamp's home page and the shared 
lesson materials developed by software carpentry.

The sections below explain:
*   how do we edit the workshop front page
*   how do we add or edit the course materails.  

**Note:**

**Table of Contents**

*   [Editing the workshop front page](#background)
*   [Edit Course Material](#getting-started)

Background




4.   a new branch in the local clone named `gh-pages`.

    ~~~
    $ git checkout -b gh-pages
    ~~~

5.  Pull content from the template repository's `gh-pages` branch into your desktop repository:

    ~~~
    $ git pull swc gh-pages
    ~~~

    This may take a minute or two.

6.  Remove the `swc` remote so that you don't accidentally try
    to push your changes to the main `bc` repository:

    ~~~
    $ git remote rm swc
    ~~~

7.  Edit `index.html` to create the bootcamp home page.
    In particular,
    double-check
    [the variables in the page's header](#variables),
    as these are used to update the main website,
    and make sure the [website content](#website-content) is correct.
    You can use the script `./bin/swc_index_validator.py`
    to check `index.html` for problems
    by running the command `make check`.

8.  Edit `_includes/setup.html` to provide software installation instructions for bootcamp attendees.
    This is described in more detail in the section on [website content](#website-content).


10. Replace the content of this `README.md` file with a line or two describing your bootcamp.

11. Push content to your YYYY-MM-DD-site repository:

    ~~~
    $ git push origin gh-pages
    ~~~

As soon as your repo has been pushed to GitHub, GitHub will render your pages
at the url:

~~~
http://{your-github-username}.github.io/YYYY-MM-DD-site/
~~~

You may update your bootcamp's website whenever you want.


~~~

Variables
---------

Your bootcamp's `index.html` page
(which uses the `bootcamp.html` layout from the `_layouts` directory)
*must* define the following values in its YAML header:

*   `layout` must be `bootcamp`.
*   `root` is the path to the repository's root directory.
    This is '.' if the page is in the root directory
    (which `index.html` is).
    In other pages,
    `root` is '..' if the page is one directory down,
    '../..' if it is two levels down,
    and so on.
*   `venue` is the name of the institution or group hosting the bootcamp.
*   `address` is the bootcamp's street address.
*   `country` must be a hyphenated country name like 'United-States'.
    This is used to look up flags for display in the main web site;
    see the `assets/flags` directory in the `site` repo for a full list of valid names.
*   `latlng` is the latitude and longitude of the bootcamp site
    (so we can put a pin on our map).
*   `humandate` is the human-friendly dates for the bootcamp (e.g., Jul 3-4, 2015).
    Please use three- or four-letter month names and abbreviations
    (e.g., `Dec` instead of `December`).
*   `startdate` is the bootcamp's starting date in YYYY-MM-DD format.
*   `enddate` is the bootcamp's ending date in the same format.
    If your bootcamp is only one day long,
    the `enddate` field can be deleted.
*   `registration` is `open` (if anyone is allowed to sign up)
    or `restricted` (if only some people are allowed to take part).
    Please do *not* put HTML or links in here to explain
    who's allowed to enrol or how to go about doing it;
    that should go in the main body of your page.
*   `instructor` is a comma-separated list of instructor names.
    This must be enclosed in square brackets,
    as in `["Alan Turing","Grace Hopper"]`
*   `helper` is a comma-separated list of helper names.
    This must be enclosed in square brackets,
    as in `["John von Neumann"]`
*   `contact` is the contact email address to use for your bootcamp.


Website Content
---------------

The body of `index.html` contains
an explanation of what a bootcamp is and how it runs,
followed by setup instructions for our standard software.
There is an explanatory comment for each section of this page;
reorganize, rewrite, or delete the material as you think best.

`index.html` depends on five HTML files in the `_includes` directory:

*   `header.html`: material for the page's head.
*   `banner.html`: the generic banner with the Software Carpentry logo.
*   `footer.html`: the generic footer with links to Software Carpentry's web presence.
*   `javascript.html`: JQuery and Bootstrap Javascript.
*   `setup.html`: common setup instructions.

You normally won't need to worry about the first four ---
they're included in the right places by our standard layouts ---
but you will probably want to edit the fifth.
In particular,
if you are teaching a Python bootcamp,
you should delete the instructions for installing R,
and vice versa.

Lesson Material
---------------

1.  The current material for novices is in the directories under `novice`.
    The shell and Git materials are written in Markdown,
    while the Python and SQL use the IPython Notebook.
2.  New material for intermediate learners is currently under development
    in directories under `intermediate`.
3.  Our old lesson material
    is in the `lessons` directory.
    We plan to retire it in Spring 2014.

As explained [below](#building-things),
you can use `make` to compile this material in the way that GitHub does
when changes are committed to the `gh-pages` branch.

Building Things
---------------

GitHub automatically runs Jekyll
to regenerate the pretty HTML versions of our content
every time changes are pushed to the `gh-pages` branch of this repository.
We use `make` to imitate that process locally
so that people can preview changes before committing.
We also use `make` to automate a handful of other tasks,
such as converting IPython Notebooks from `.ipynb` format to Markdown (`.md`)
so that Jekyll can convert them to HTML.

Most of the commands to rebuild things are in `Makefile`;
run the command `make` on its own to get a list of targets,
and `make site` to re-run Jekyll to preview your site
(which Jekyll will put in the `_site` directory).
You can also run `make check` to run a Python script
that checks whether `index.html`'s variables are formatted correctly,
and `make clean` to clean up all generated files.

The commands used to turn IPython Notebooks into Markdown files
are stored in a separate Makefile called `ipynb.mk`.
This separation ensures that people can rebuild the site
even if they don't have IPython installed
(which R instructors might not);
it also guarantees that `make` won't try to regenerate Markdown after a Git pull
(which might change the timestamps on files,
but not actually change their contents).
If we add more languages and file formats in future,
we may also create separate Makefiles for them.

Site Map
--------

The most important files and directories are **highlighted**.

*   CITATION - how to cite Software Carpentry.
*   CONTRIBUTING.md - how to contribute new material.
*   LICENSE.md - our license.
*   **Makefile** - rebuild this site (type `make` on its own for a list of targets).
*   **README.md** - how to use this site.
*   _config.yml - Jekyll configuration directives.
*   _includes/ - snippets of HTML that can be included in other files by Jekyll.
*   _layouts/ - Jekyll page layouts.
*   **_site/** - output directory (created when building the site locally).
*   _templates/ - template files for conversion of IPython Notebooks to Markdown.
    Templates for other conversion systems (e.g., Pandoc) should go here, too.
*   bib.md - bibliography.
*   bin/ - miscellaneous tools used in building the site.
*   book.md - generated when compiling the website locally.
*   contents.md - site map used in place of `index.html` on the main web site.
*   css/ - CSS files for this site.
*   data/ - miscellaneous data files used by examples.
*   etherpad.txt - starter text for the bootcamp's Etherpad.
*   gloss.md - glossary of terms.
*   img/ - images used throughout this site.
*   **index.html** - template for bootcamp home pages.
*   intro.md - introduction to book version of this site.
*   ipynb.mk - Makefile for turning IPython Notebooks into Markdown.
*   js/ - Javascript files used in this site.
*   lessons/ - old lesson material.
*   novice/ - novice lesson material.
*   rules.md - the rules of programming (used in the book version of this site).
*   setup.md - placeholder for bootcamp setup instructions.
*   setup/ - setup tools for installing bootcamp software.
*   team.md - who we are.

FAQ
---

*   *Where can I get help?*
    <br/>
    Mail us at [admin@software-carpentry.org](mailto:admin@software-carpentry.org),
    come chat with us on [our IRC channel](irc://moznet/sciencelab),
    or join our [discussion list](http://software-carpentry.org/contrib/discuss.html)
    and ask for help there.

*   *Why does the bootcamp repository have to be created from scratch? Why not fork `bc` on GitHub?*
    <br/>
    Because any particular user can only have one fork of a repository,
    but instructors frequently need to work on several bootcamps at once.

*   *Why use Jekyll?  Why not some other markup language and some other converter?*
    <br/>
    Because it's the default on GitHub.
    If we're going to teach people to use that site,
    we should teach them to use it as it is,
    not as we wish it was.

*   *Why does `make site` take so long?*
    <br/>
    We know this problem happens with pandoc >= 1.2 and <= 1.12.3.3. If you are
    using one of this versions you can (a) update or (b) downgrade pandoc.

    On a MacBook Air with pandoc 1.11.1 and Jekyll 1.3.0,
    making the site from scratch takes approximately 24 seconds,
    half of which is spent converting IPython Notebooks.

*   *What do I do if I see a `invalid byte sequence in ...` error when I run `make check`?*
    <br/>
    Declare the `en_US.UTF-8` locale in your shell:

    ~~~
    $ export LC_ALL=en_US.UTF-8
    $ export LANG=en_US.UTF-8
    ~~~

*   *What do I do if I see a `Conversion error` when I run `make check`?*
    <br/>
    The error message may look something like this:

    ~~~
    Configuration file: d:/OpenCourses/swc/2013-10-17-round6.4/_config.yml
            Source: d:/OpenCourses/swc/2013-10-17-round6.4
       Destination: _site
      Generating... c:/Ruby193/lib/ruby/gems/1.9.1/gems/posix-spawn-0.3.6/lib/posix/spawn.rb:162: wa
    rning: cannot close fd before spawn
    Conversion error: There was an error converting 'lessons/misc-biopython/fastq.md'.
    done.
    ~~~
        
    This is a [problem in Pygments.rb](http://stackoverflow.com/questions/17364028/jekyll-on-windows-pygments-not-working)
    Uninstall pygments.rb 0.5.1 or 0.5.2, install 0.5.0.  For example, here's how you would
    uninstall pygments 0.5.2 and restore version 0.5.0:

    ~~~
    $ gem uninstall pygments.rb --version "=0.5.2"
    $ gem install pygments.rb --version "=0.5.0"
    ~~~

*   *What do I do if I see a `File not found: u'nbconvert'` when I run `make check`?*
    <br/>
    The output of `make check` looks like this:

    ~~~
    WARNING: Unrecognized alias: 'output', it will probably have no effect.[TerminalIPythonApp] File not found: u'nbconvert'
    cp tmp/python/novice/01-numpy.html _site/python/novice/01-numpy.html
    cp: cannot stat ‘tmp/python/novice/01-numpy.html’: No such file or directory
    ~~~
    
    This means you don't have a recent enough version of IPython (below 1.0) and you should install a newer version.
    Installing a local version can be done with:
    
    ~~~
    $ pip install --upgrade --user ipython
    ~~~

    You might need `pip` that can be installed (under Ubuntu and alike) with:

    ~~~
    $ sudo apt-get install python-pip
    ~~~

*   *What if I get some missing packages messages when I run `make check`?*
    <br/>
    Some additional packages are required. They can be installed (under Ubuntu and alike) with:
    
    ~~~
    $ sudo apt-get install pandoc
    ~~~

*   *Where should pages go if multiple boot camps are running at a site simultaneously?*
    <br/>
    Use subdirectories like `2013-07-01-esu/beginners`,
    so that main directory names always follow our four-part convention.

>>>>>>> 2b9974c303a9b8258f4aae8ed6480849adf01fb4
>>>>>>> 548d764c923c07447bf310af68c214f52d577798

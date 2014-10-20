<<<<<<< HEAD
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


1.   clone the repo 

    ~~~
    $ git clone https://github.com/SWC-OSG-Workshop/2014-10-20.git


1.   a new branch in the local clone named `gh-pages`.

    ~~~
    $ git checkout -b gh-pages
    ~~~

2.  Pull content from the template repository's `gh-pages` branch into your desktop repository:

    ~~~
    $ git pull origin gh-pages
    ~~~

    This may take a minute or two.


*   [Editing the workshop front page](#background)

Editing workshop front page involves editing html pages. Two html files are of 
primary interest to us. One is the "index.html" and other is "_includes/setup.html".


1.  Edit `index.html` to make any changes to the bootcamp home page.
    In particular, double-check
    [the variables in the page's header](#variables),
    as these are used to update the main website,
    and make sure the [website content](#website-content) is correct.
    You can use the script `./bin/swc_index_validator.py` to 
    check `index.html` for problems
    by running the command `make check`.

2.  Edit `_includes/setup.html` to provide software installation instructions for bootcamp attendees.


3. Push content to the repository:

    ~~~
    $ git push origin gh-pages
    ~~~

As soon as the repo has been pushed to GitHub, GitHub will render the pages
at the url:

~~~
http://dmbala.github.io/SWC-OSG/
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


<<<<<<< HEAD
==================
Software Carpentry - Open Science Grid (SWC-OSG) Workshop 
============================

This repository's `gh-pages` branch is the starting point for a workshop 
website: it contains a template for the workshop's home page and the shared 
lesson materials developed by software carpentry.

The sections below explain:
*   how do we edit the workshop front page.
*   how do we add or edit the course materails.  

**Note:**

**Table of Contents**

*   [Getting Started](#getting-started)
*   [Workshop Front Page](#workshop-frontpage)
*   [Lesson Material](#lession-material)
*   [Site Map](#site-map)

##Getting Started##
---------------

1.   clone the repo 

    ~~~
    $ git clone https://github.com/SWC-OSG-Workshop/2014-10-20.git
    ~~~


1.   a new branch in the local clone named `gh-pages`.

    ~~~
    $ git checkout -b gh-pages
    ~~~

2.  Pull content from the template repository's `gh-pages` branch into your desktop repository:

    ~~~
    $ git pull origin gh-pages
    ~~~

    This may take a minute or two.


##Workshop Front Page##
-------------------

Editing workshop front page involves editing html pages. Two html files are of 
primary interest to us. One is the "index.html" and other is "_includes/setup.html".


Edit `index.html` to make any changes to the workshop home page.
    In particular, double-check
    [the variables in the page's header](#variables),
    as these are used to update the main website,
    and make sure the [website content](#website-content) is correct.
    You can use the script `./bin/swc_index_validator.py` to 
    check `index.html` for problems
    by running the command `make check`.


Once finished editing the index.html, push content to the repository:

    ~~~
    $ git add index.html
    $ git commit -m "some message here about the changes " 
    $ git push origin gh-pages
    ~~~

As soon as the repo has been pushed to GitHub, GitHub will render the pages
at the url:

~~~
http://swc-osg-workshop.github.io/2014-10-20/
~~~

Below is the detail about the page header (#variables) and the website
contents (#website-content) to design the "index.html" page. 

**Variables**
---------

The workshop's `index.html` page
(which uses the `workshop.html` layout from the `_layouts` directory)
*must* define the following values in its header:

*   `layout` must be `workshop`.
*   `root` is the path to the repository's root directory.
    This is '.' if the page is in the root directory
    (which `index.html` is).
    In other pages,
    `root` is '..' if the page is one directory down,
    '../..' if it is two levels down,
    and so on.
*   `venue` is the name of the institution or group hosting the workshop.
*   `address` is the workshop's street address.
*   `country` must be a hyphenated country name like 'United-States'.
    This is used to look up flags for display in the main web site;
    see the `assets/flags` directory in the `site` repo for a full list of valid names.
*   `latlng` is the latitude and longitude of the workshop site
    (so we can put a pin on our map).
*   `humandate` is the human-friendly dates for the workshop (e.g., Jul 3-4, 2015).
    Please use three- or four-letter month names and abbreviations
    (e.g., `Dec` instead of `December`).
*   `startdate` is the workshop's starting date in YYYY-MM-DD format.
*   `enddate` is the workshop's ending date in the same format.
    If your workshop is only one day long,
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
*   `contact` is the contact email address to use for your workshop.


**Website Content**
---------------

The body of `index.html` contains
an explanation of what a workshop is and how it runs,
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
but you will probably want to edit the fifth - `setup.html`.

##Lesson Material##
---------------

The current material for novices is in the directories under `novice`.
    The shell and Git materials are written in Markdown, while the Python and 
SQL use the IPython Notebook.


##Site Map##
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
*   etherpad.txt - starter text for the workshop's Etherpad.
*   gloss.md - glossary of terms.
*   img/ - images used throughout this site.
*   **index.html** - template for workshop home pages.
*   intro.md - introduction to book version of this site.
*   ipynb.mk - Makefile for turning IPython Notebooks into Markdown.
*   js/ - Javascript files used in this site.
*   lessons/ - old lesson material.
*   novice/ - novice lesson material.
*   rules.md - the rules of programming (used in the book version of this site).
*   setup.md - placeholder for workshop setup instructions.
*   setup/ - setup tools for installing workshop software.
*   team.md - who we are.


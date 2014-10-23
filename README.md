============================
#Software Carpentry - Open Science Grid (SWC-OSG) Workshop#
============================
The current repository contains the basic learning modules and necessary 
tools to set up a new workshop website. 


The sections below explain:
*   How to set up the git repo on your local machine.
*   How do we add or edit the course materails.  
*   How do we edit the workshop front page.

**Note:**

**Table of Contents**

*   [Getting Started](#getting-started)
*   [Lesson Material](#lession-material)
*   [Workshop Front Page](#workshop-frontpage)

##Getting Started##
---------------

First, a note about why we are working on `gh-pages` branch. Because the HTML and markdown 
files are rendered as webpages by GitHub.  For example,  the current page is rendered at 
the url: CurrentWebPage(http://swc-osg-workshop.github.io/general/). 

Now let us see how to make a copy of the existing repo on your locale machine.  In the 
desktop or laptop, clone the repo 

    ~~~
    $ git clone https://github.com/SWC-OSG-Workshop/general.git
    ~~~

and create new branch named `gh-pages`.

    ~~~
    $ git checkout -b gh-pages
    ~~~

Now pull the content repository's `gh-pages` branch into your desktop repository:

    ~~~
    $ git pull origin gh-pages
    ~~~
##Lesson Material##
---------------

The current material is in the directories under `novice`. The shell and Git materials are 
written in Markdown, while the Python and SQL use the IPython Notebook. The material 
related to OSG is in the directory DHTC and are written in Markdown. 

Once finished editing or adding material under novice/DHTC directory push the 
content to the repository:

    ~~~
    $ git add filename.md
    $ git commit -m "some message here about the changes " 
    $ git push origin gh-pages
    ~~~



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


Edit `_includes/setup.html` to provide software installation instructions for the workshop attendees.

Once finished editing the index.html, push content to the repository:

    ~~~
    $ git add index.html
    $ git commit -m "some message here about the changes " 
    $ git push origin gh-pages
    ~~~

As soon as the repo has been pushed to GitHub, GitHub will render the pages
at the url(http://swc-osg-workshop.github.io/general)


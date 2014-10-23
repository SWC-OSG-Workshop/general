============================
#Software Carpentry - Open Science Grid (SWC-OSG) Workshop#
============================
The current repository contains the basic learning modules and necessary 
tools to set up a new workshop website. 


The sections below explain:
*   How to set up the current git repo on your local machine.
*   How do we add or edit the course materails.  
*   How do we edit the workshop front page.
*   How do we generate a new repo for a new workshop.


**Table of Contents**

*   [Getting Started](#getting-started)
*   [Lesson Material](#lession-material)
*   [Workshop Front Page](#workshop-frontpage)
*   [New Repo for a New Workshop](#new-repo)

##Getting Started##
---------------

We work on `gh-pages` branch. Because GitHub renders the webpage when the HTML and markdown 
files are located at `gh-pages` branch. For example, the HTML and 
markdown files of the current `gh-pages` branch is rendered at the 
url: CurrentWebPage(http://swc-osg-workshop.github.io/general/). 

Now let us see how to make a copy of the existing repo on your locale machine.  In your local 
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

##New Repo for a New Workshop##

As soon as the workshop date is finalized, create a repo by the name
"YYYY-MM-DD", where YYYY is the year, MM is the month and DD is the
date of the workshop. The generated repository needs to have the
course material and web content for a workshop. Since we have all
the materials under the general repo, we copy the course material and web
content from the general repo. In your local Desktop or Laptop, type
the followings
~~~
 git clone https://github.com/SWC-OSG-Workshop/YYYY-MM-DD.git
 cd YYYY-MM-DD 
 git checkout -b gh-pages 
 git remote add general https://github.com/SWC-OSG-Workshop/general.git 
 git pull general gh-pages 
 git remote remove general 
~~~

Now you will have to change the workshop front page in the current repo `YYYY-MM-DD.git` as 
outlined in the previous section.  


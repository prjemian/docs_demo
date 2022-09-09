.. Documentation Repository documentation master file, created by
   sphinx-quickstart on Fri Sep  9 00:16:57 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Documentation Repository's documentation!
====================================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

Steps
-----

Create a new GitHub repository (or use an existing one).

On the Linux workstation, change to your directory with software projects.
Perhaps that is ``~/Documents``:

    cd ~/Documents

Clone the repository from GitHub:

    git clone https://github.com/USERNAME/REPONAME
    cd reponame

Tell git to ignore a directory, then commit this change to git::

    touch .gitignore
    echo "build/" > .gitignore
    git commit -ma "ignore local Sphinx build products"

Set up the Sphinx structure in a ``docs/`` subdirectory::

    mkdir docs
    pushd docs
    sphinx-quickstart

Answer the questions from ``sphinx-quickstart``::

   Welcome to the Sphinx 5.1.1 quickstart utility.

   Please enter values for the following settings (just press Enter to
   accept a default value, if one is given in brackets).

   Selected root path: .

   You have two options for placing the build directory for Sphinx output.
   Either, you use a directory "_build" within the root path, or you separate
   "source" and "build" directories within the root path.
   > Separate source and build directories (y/n) [n]: y

   The project name will occur in several places in the built documentation.
   > Project name: Documentation Repository
   > Author name(s): Pete Jemian
   > Project release []: 0.0.1

   If the documents are to be written in a language other than English,
   you can select a language here by its language code. Sphinx will then
   translate text that it generates into that language.

   For a list of supported codes, see
   https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language.
   > Project language [en]: 

   Creating file /home/prjemian/Documents/projects/prjemian/docs/source/conf.py.
   Creating file /home/prjemian/Documents/projects/prjemian/docs/source/index.rst.
   Creating file /home/prjemian/Documents/projects/prjemian/docs/Makefile.
   Creating file /home/prjemian/Documents/projects/prjemian/docs/make.bat.

   Finished: An initial directory structure has been created.

   You should now populate your master file /home/prjemian/Documents/projects/prjemian/docs/source/index.rst and create other documentation
   source files. Use the Makefile to build the docs, like so:
      make builder
   where "builder" is one of the supported builders, e.g. html, latex or linkcheck.

Switch back to the repository root directory, then commit to git::

   popd
   git commit -ma "create documentation directory"

Test that Sphinx can build the documentation.  To view use ``chromium``, ``firefox``,
``google-chrome``, whatever::

   make -C docs clean html
   chromium ./docs/build/html/index.html &

Create the GitHub workflow directory::

   mkdir -p ./.github/workflows

Download the workflow from the template repository::

   pushd ./.github/workflows
   wget https://raw.githubusercontent.com/prjemian/docs_demo/main/.github/workflows/publish-docs.yml
   popd
   git commit -ma "add documentation publishing workflow"

Push these commits back to GitHub (you'll need your github username and
authentication token)::

   git push

Configure the repository's GitHub Pages settings on the GitHub web site
(https://github.com/USERNAME/REPONAME/settings/pages), and make these settings
in the Build and Deployment section:

* Source: _Deploy from a branch_
* Branch: select ``gh-pages`` from the drop down, and press ``Save``

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

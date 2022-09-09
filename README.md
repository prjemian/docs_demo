# docs_demo

Demonstration of Sphinx documentation repository on GitHub.

## Initial Setup

<details>
<summary>TODO</summary>
TODO: Make new repo or use existing
</details>

Create a new GitHub repository (or use an existing one).

On the Linux workstation, change to your directory with software
projects. Perhaps that is `~/Documents`:

    cd ~/Documents

Clone the repository from GitHub (replace `USERNAME` and `REPONAME` with your
own details):

    git clone https://github.com/USERNAME/REPONAME
    cd reponame

Tell git to ignore a directory, then commit this change to git:

    touch .gitignore
    echo "build/" > .gitignore
    git commit -ma "ignore local Sphinx build products"

Set up the Sphinx structure in a `docs/` subdirectory:

<details>
<summary>TODO</summary>
TODO: Explain `pushd` and `popd` and how they relate to `cd`
</details>

    mkdir docs
    pushd docs
    sphinx-quickstart

Answer the questions from `sphinx-quickstart`:

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

    Creating file ~/Documents/REPONAME/docs/source/conf.py.
    Creating file ~/Documents/REPONAME/docs/source/index.rst.
    Creating file ~/Documents/REPONAME/docs/Makefile.
    Creating file ~/Documents/REPONAME/docs/make.bat.

    Finished: An initial directory structure has been created.

    You should now populate your master file ~/Documents/REPONAME/docs/source/index.rst and create other documentation
    source files. Use the Makefile to build the docs, like so:
       make builder
    where "builder" is one of the supported builders, e.g. html, latex or linkcheck.

Switch back to the repository root directory, then commit to git:

    popd
    git commit -ma "create documentation directory"

Test that Sphinx can build the documentation. Using a WEBBROWSER (`*`) on
the local workstation:

    make -C docs clean html
    WEBBROWSER ./docs/build/html/index.html &

Create the GitHub workflow directory:

    mkdir -p ./.github/workflows

Download the workflow from the template repository:

    pushd ./.github/workflows
    wget https://raw.githubusercontent.com/prjemian/docs_demo/main/.github/workflows/publish-docs.yml
    popd
    git commit -ma "add documentation publishing workflow"

Push these commits back to GitHub (you\'ll need your github username and
authentication token). This will start the workflow that should create
the HTML documentation and push it to the `gh-pages` branch:

<details>
<summary>TODO</summary>
TODO: Explain the github username and authentication token
Caution about not putting the token in a repository and NOT to push it to GitHub.  How to remove such a push from the repo entirely (not just undo in later commit)
Teach good management of this credential.
git credentials cache is added info
</details>

    git push

Check the Actions logs on the the repository\'s GitHub web site
(<https://github.com/USERNAME/REPONAME/actions>) to confirm the
`Publish Sphinx Docs to GitHub Pages` workflow completed successfully.

<details>
<summary>TODO</summary>
TODO: Explain about errors in the Actions and how to diagnose
What could go wrong?  Not likely to be your source code if you built it locally.
More likely to be:

* software versions
* missing packages
* YAML file errors
* random brownout in GitHub Actions service
</details>

Configure the repository\'s GitHub Pages settings on the GitHub web site
(<https://github.com/USERNAME/REPONAME/settings/pages>), and make these
settings in the Build and Deployment section:

- Source: `Deploy from a branch`
- folder: select `/` (root) from the drop down
- Branch: select `gh-pages` from the drop down, and press `Save`

Go back to the Actions logs
(<https://github.com/USERNAME/REPONAME/actions>). A new workflow called
`pages-build-deployment` will run (or be running already). Once this is
complete (and successful), the new docs will be published to
<https://USERNAME.github.io/REPONAME>. Note that the first time, it
takes a couple minutes before this page appears.

## When you update the documentation

Edit content in the `docs/source` directory as desired. Consult the
Sphinx web site (`**`) for more information.

To rebuild the documentation locally (from the repository\'s root
directory):

    make -C docs html

View (`**`) the HTML documentation again:

    WEBBROWSER ./docs/build/html/index.html &

Once you are satisfied with the changes locally, commit and push to
GitHub. This will trigger a new run of the publishing workflow. If that
is successful, then the revised documentation will be published.

    git commit -am "explain the changes, briefly"
    git push

------

`*`: your choice of WEBBROWSER might be `chromium`, `firefox`,
    `google-chrome`, `brave`, `opera`, whatever

`**`: Sphinx web site: <https://www.sphinx-doc.org>

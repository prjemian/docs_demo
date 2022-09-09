# Procedure Sphinx + GitHub Pages

Demonstration of Sphinx documentation repository publishing to [GitHub
Pages](https://pages.github.com/) using a [GitHub
Actions](https://github.com/features/actions) workflow.

Procedure

- [Procedure Sphinx + GitHub Pages](#procedure-sphinx--github-pages)
  - [Initial Setup](#initial-setup)
    - [Checkout Repository from GitHub](#checkout-repository-from-github)
    - [Some new files do not belong in git](#some-new-files-do-not-belong-in-git)
    - [Create directory for documentation source](#create-directory-for-documentation-source)
    - [Add workflow instructions](#add-workflow-instructions)
    - [Push to GitHub](#push-to-github)
      - [GitHub Actions](#github-actions)
      - [GitHub Pages](#github-pages)
  - [When you update the documentation](#when-you-update-the-documentation)
  - [Add URL to Repo page](#add-url-to-repo-page)
  - [</details>](#details)

## Initial Setup

Create a new GitHub repository (or use an existing one).
For this example, we'll call it `https://github.com/USERNAME/REPONAME`
where you will replace `USERNAME` and `REPONAME` with your
own details.

### Checkout Repository from GitHub

On the Linux workstation, change to your directory with software
projects. Perhaps that local directory is `~/Documents`, you choose:

    cd ~/Documents

Clone the repository from GitHub:

    git clone https://github.com/USERNAME/REPONAME
    cd reponame

### Some new files do not belong in git

Tell git to ignore a directory (and anything in it), then commit this change to
git:

    touch .gitignore
    echo "build/" > .gitignore
    git commit -ma "ignore local Sphinx build products"

### Create directory for documentation source

Set up the Sphinx structure in a `docs/` subdirectory:

<details>
<summary>pushd vs. cd</summary>
Here, we use `pushd` to change directories, since later, `popd` will remember
the directory where we started.  Think of `pushd` and `popd` as a smart
implementation of `cd` that remembers.
</details>

    mkdir docs
    pushd docs
    sphinx-quickstart

Answer the questions from `sphinx-quickstart`:

<details>
<summary>example</summary>
<code>sphinx-quickstart</code> is a <em>question and answer</em> process that configures your documentation
with details specific to your project.  The results are stored in the
<code>source/conf.py</code> and <code>index.rst</code> files.
They can be changed by you as you choose.

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
</details>

Switch back to the repository root directory, then commit this new directory to git:

    popd
    git commit -ma "create documentation directory"

Test that Sphinx can build the documentation. Using a WEBBROWSER (`*`) on
the local workstation:

    make -C docs clean html
    WEBBROWSER ./docs/build/html/index.html &

### Add workflow instructions

Create the GitHub workflow directory:

    mkdir -p ./.github/workflows

Download the workflow instructions from the template repository:

    pushd ./.github/workflows
    wget https://raw.githubusercontent.com/prjemian/docs_demo/main/.github/workflows/publish-docs.yml
    popd
    git commit -ma "add documentation publishing workflow"

### Push to GitHub

Push these commits back to GitHub (you\'ll need your github username and [access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)). This will start the workflow that should create
the HTML documentation and push it to the `gh-pages` branch:

<details>
<summary>more about access tokens</summary>
Instead of passwords, GitHub now requires remote users to setup and use a <em>personal access token</em> for authentication during <code>git push</code> and others steps that require credentials.

A web search for <em>GitHub personal access token</em> should return many links to help on this.  One such link, https://www.howtogeek.com/devops/how-to-set-up-https-personal-access-tokens-for-github-authentication/, provides a good explanation about this PAT and how to manage it locally.

CAUTION: Do <b color="red">NOT</b> put the token in any file that will push it to GitHub.  This would publish your credentials for any hacker anywhere to take over your GitHub account!

But ..., we've all done something like that at one time or another.  Or committed a really big file that should not remain in the repository.

<b>Q</b>: How to remove such a push from the repo entirely (not just undo in later commit)?

<b>A</b>: Web search for <em>git revert commit</em>.  One link: https://www.theserverside.com/tutorial/How-to-git-revert-a-commit-A-simple-undo-changes-example
</details>

    git push


#### GitHub Actions

Check the Actions logs on the the repository\'s GitHub web site
(<https://github.com/USERNAME/REPONAME/actions>) to confirm the
`Publish Sphinx Docs to GitHub Pages` workflow completed successfully.

<details>
<summary>logs and errors</summary>
TODO: Explain about errors in the Actions and how to diagnose
What could go wrong?  Not likely to be your source code if you built it locally.
More likely to be:

* software versions
* missing packages
* YAML file errors
* random brownout in GitHub Actions service
</details>

#### GitHub Pages

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

## Add URL to Repo page

Configure the main repository page to show the new documentation URL.

<details>
<summary>TODO</summary>
</details>
------

`*`: your choice of WEBBROWSER might be `chromium`, `firefox`,
    `google-chrome`, `brave`, `opera`, whatever

`**`: Sphinx web site: <https://www.sphinx-doc.org>

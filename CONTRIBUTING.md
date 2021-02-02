## Getting in touch
We often hang out on [this Telegram chat](https://t.me/opensuse_docs), which by the way is bridged to the IRC `#docs:opensuse.org` and to `#docs` on [this Discord channel](https://discord.gg/opensuse).
## Workflow
_Outline of the general workflow_:
```
new document ------> review on structure and contents #1 ------> review on language, style and punctuation #1
                                                                                        |
                                                                                        |
marked "ready for release" <--- review on language #2 <--- review on contents #2  <------
```
_Collaboration workflow_:
* If you are not familiar with the fork -> pull request workflow, please refer to [this](https://jarv.is/notes/how-to-pull-request-fork-github/).
* When adding a new article make sure it lands in `/project/docs`
* Please try to comply with the guidelines under 'Commits' and 'Pull Requests' below.
* Check out on the [table of contents](https://github.com/openSUSE/openSUSE-docs-revamped/blob/dev/ToC.md) to track every document's status and discussions.
### Branches
* The default branch -- the working branch -- is not `main` or `master` but `dev`. I will merge from one milestone to the other.
### Commits
4 types of commits. PRs should whenever possible concern just one type of commit:
 * `structure` (how the textual and multimedia contents breaks down into different parts)
 * `design` (web and non-web visuals)
 * `web-functions` (functionalities invoked from the web release of the docs)
 * `contents` (textual and multimedia contents)

New features should be proposed or discussed using `dev` as baseline, unless they refer to a particular `structure`-, `design`- or `contents`- commit.
### Pull Requests (PRs)
When lodging a PR please make sure to give us permission to commit to the Pull Request branch by checking the `Allow edits from maintainers` checkbox on the Pull Request. Otherwise we won't be able to work with you on your PR.
### Style
_Structure_. Each document should start with an intro stating the end goal, the important presuppositions (typically about pre-requirements) that the document is making, and an outline of the main steps on the path to the goal.

_Technical jargon_. Important and unavoidable jargon should be defined (typically inlined, with info boxes). Overall the document should be understanble by a teenager (think secondary school textbook).

_Format_:
* reference points and path items in italics, ex: "_Settings_ > _Energy Saving_"
* action in bold, ex. "click __Yes__"
* code instructions part of a stepwise recipe between line breaks in code, ex. 
```
$ sudo zypper dup
```
* short snippets of code or not part of a stepwise recipe can be inlined as in "... run `sudo zypper dup` before anything else".
## Building and serving the docs
* You can either install mkdocs from pip or from a virtual environment.
* It's highly recommended to use a virtual environment and not pip, so that the dependencies of this project won't mess with your system-wide python packages / modules. You still need to use pip to install pipenv though ;).
* Personnally I am using pipenv, which you install on openSUSE distributions with: `pip3 install --user pipenv`. Then you'll need to add `~/.local/bin` to your PATH. The best method for that depends on your shell:
    * for `bash` add `PATH=$PATH:/home/your-user-name/.local/bin` to `.bashrc`
    * for `fish` run the following command once from a fish shell: `set -Ua fish_user_paths /home/your-user-name/.local/bin`
    * for `zsh` add `export PATH=$PATH/home/your-user-name/.local/bin` to `.zshrc`

* Then 
    1. clone this repo where you want in your home folder
    2. `cd` to it and run `pipenv install` to install the dependencies, and then `pipenv shell` to run the environment. 
    3. finally `cd` to `project` and run you `mkdocs` and `mkdocs-versioning` commands from there, i.e. `mkdocs build` to generate the web content and `mkdocs serve` to serve it (by default at http://127.0.0.1:8000/) (replace `mkdocs` with `mkdocs-versioning` if you want to produce a multi-version build instead). The built-in dev-server allows you to preview your documentation as you're writing it. It will even auto-reload and refresh your browser whenever you save your changes.
* Available commands & documentation on mkdocs: https://www.mkdocs.org/

## Repo structure
```
...
Pipfile     # dependencies listed in pipenv format
requirements.txt    # dependencies listed in pip format (added for compatibility purposes)
project/    # where you run your mkdocs commands from (while the pipenv commands are to be run from the root directory)
    docs/   # contents files that mkdocs injects when building the website
    site/   # the source files of the generated website after each 'mkdocs build' command.
    ...
...
```
`.gitignore` should not let you commit any `build` directory. Please make sure that is the case.

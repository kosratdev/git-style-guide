# Git Style Guide

# Contents
0. [Introduction](#introduction)
1. [Setup Workspace](#setup workspace)
2. [Branches](#branches)
3. [Commits](#commits)
  1. [Messages](#messages)
4. [Merging](#merging)
5. [References](#references)

## 0. Introduction
This is Git Style Guide inspired by the [git man pages](http://git-scm.com/doc) and various practices popular among the community. If you are not good at using Git look at [this cheat sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) or take [this course](https://www.udacity.com/course/how-to-use-git-and-github--ud775)

## 1. Setup Workspace
Setup your terminal with a colorful custom bash prompt to see git status easily look at my custom prompt:

![terminal](/files/img.png)

If you want to obtain this custom prompt do the followings:
  1. #### Downloading necessary files
    * Save [this file]('/files/git-completion.bash') in your home directory with the name `git-completion.bash`.
    * Save [this file]('/files/git-prompt.sh') in your home directory with the name `git-prompt.sh`.
    * Download [this file (bash_profile_file)]('/files/bash_profile_file').
    * If you already have a file in your home directory named `.bash_profile`, copy the content from `bash_profile_file` and paste it at the bottom of `.bash_profile`. Otherwise, move `bash_profile_file` to your home directory and rename it to `.bash_profile`. If you use Linux, you may need to name this file `.bashrc` instead of `.bash_profile`.(If you're curious to learn more about how bash prompts work, see [this page](http://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html).)
  2. #### Making Git configurations
    Run the following Git configuration commands. The first one will need to be modified if you are using a text editor other than Vim. See [this page](https://help.github.com/articles/associating-text-editors-with-git/) for the correct command for a couple of other popular text editors. For any other editor, you'll need to enter the command you use to launch that editor from Git Bash.
    ```shell
    git config --global core.editor "vim"
    git config --global push.default upstream
    git config --global merge.conflictstyle diff3
    ```
    You'll need to close and re-open the terminal before all your changes take effect.

## 1. Repository Name
Good naming convention would be :
* Use lowercase
* Separate word with hyphens `-`.

There is no standard convention about letter case for naming Git repositories. But a good reason to stick to lowercase is that repository names are often seen in URLs that may be case insensitive or even converted to lower case (it happened to GitLab or Jira users for example in the past).

## 1. Branches
* Choose *short* and *descriptive* names:

  ```shell
  # good
  $ git checkout -b oauth-migration

  # bad - too vague
  $ git checkout -b login_fix
  ```
* Use *dashes* to separate words.

* Identifiers from corresponding tickets in an external service (eg. a GitHub
  issue) are also good candidates for use in branch names. For example:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```



* When several people are working on the *same* feature, it might be convenient
  to have *personal* feature branches and a *team-wide* feature branch.
  Use the following naming convention:

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria  # Maria's personal branch
  $ git checkout -b feature-a/nick   # Nick's personal branch
  ```

  Merge at will the personal branches to the team-wide branch (see ["Merging"](#merging)).
  Eventually, the team-wide branch will be merged to "master".

* Delete your branch from the upstream repository after it's merged, unless
  there is a specific reason not to.

  Tip: Use the following command while being on "master", to list merged
  branches:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## 2. Commits

  * Each commit should be a single *logical change*. Don't make several *logical changes* in one commit. For example, if a patch fixes a bug and optimizes the performance of a feature, split it into two separate commits.

  *Tip: Use `git add -p` to interactively stage specific portions of the
  modified files.*

  * Don't split a single *logical change* into several commits. For example, the implementation of a feature and the corresponding tests should be in the same commit.

  * Commit *early* and *often*. Small, self-contained commits are easier to understand and revert when something goes wrong.

  * Commits should be ordered *logically*. For example, if *commit X* depends on changes done in *commit Y*, then *commit Y* should come before *commit X*.

  Note: While working alone on a local branch that *has not yet been pushed*, it's fine to use commits as temporary snapshots of your work. However, it still holds true that you should apply all of the above *before* pushing it.

### Messages
* Use the editor, not the terminal, when writing a commit message:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  Committing from the terminal encourages a mindset of having to fit everything in a single line which usually results in non-informative, ambiguous commit messages.
* If a *commit A* depends on *commit B*, the dependency should be stated in the message of *commit A*. Use the SHA1 when referring to commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should also be stated in the message of *commit A*.

* If a commit is going to be squashed to another commit use the `--squash` and `--fixup` flags respectively, in order to make the intention clear:

    ```shell
    $ git commit --squash f387cab2
    ```

    *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be squashed automatically.)*

#### Message Structure
A commit messages consists of three distinct parts separated by a blank line: the title, an optional body and an optional footer. The layout looks like this:
```
type: subject

body

footer
```
The title consists of the type of the message and subject.

##### The Type
The type is contained within the title and can be one of these types:
* **feat:** a new feature
* **fix:** a bug fix
* **docs:** changes to documentation
* **style:** formatting, missing semi colons, etc; no code change
* **refactor:** refactoring production code
* **test:** adding tests, refactoring test; no production code change
* **chore:** updating build tasks, package manager configs, etc; no production code change

##### The Subject
Subjects should be no greater than 50 characters, should begin with a capital letter and do not end with a period.Use an imperative tone to describe what a commit does, rather than what it did. For example, use **change**; not changed or changes.

##### The Body
Not all commits are complex enough to warrant a body, therefore it is optional and only used when a commit requires a bit of explanation and context. Use the body to explain the **what** and **why** of a commit, not the how.

When writing a body, the blank line between the title and the body is required and you should limit the length of each line to no more than 72 characters.

*tip:* Add this line to your `~/.vimrc` to add spell checking and automatic wrapping at the recommended 72 columns to you commit messages.
```shell
autocmd Filetype gitcommit setlocal spell textwidth=72
```
##### The Footer
The footer is optional and is used to reference issue tracker IDs.

Example Commit Message
```shell
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here is the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```
Following this format will create consistent and professional commits. When you look back at past projects you'll have no trouble remembering what you were doing if you continue to follow these guidelines.

Another good reason to follow this commit style guide is that it allows you to later automatically generate CHANGELOG.md from your commit messages. An example of CHANGELOG exporter script is [available here](https://github.com/rafinskipg/git-changelog). If interested, check out this blog for more information on [CHANGELOG](http://keepachangelog.com/).

### Merging

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "master" in order to merge it later.

  That said, *never rewrite the history of the "master" branch* or any other
  special branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

      ```shell
      [my-branch] $ git fetch
      [my-branch] $ git rebase origin/master
      # then merge
      ```

      This results in a branch that can be applied directly to the end of the
      "master" branch and results in a very simple history.

      *(Note: This strategy is better suited for projects with short-running
      branches. Otherwise it might be better to occassionally merge the
      "master" branch instead of rebasing onto it.)*

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

## References
* [Udacity Git Commit Message Style Guide](https://udacity.github.io/git-styleguide/)
* [agis-/git-style-guide](https://github.com/agis-/git-style-guide)
* [How to Write a Git Commit Message](http://chris.beams.io/posts/git-commit/)
* [5 Useful Tips For A Better Commit Message](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message)
* [A Note About Git Commit Messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
* [Udacity - How to Use Git and GitHub](https://www.udacity.com/course/how-to-use-git-and-github--ud775)
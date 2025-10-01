# git_workshop
**Git setup and standard procedure, with shell commands and Rstudio equivalents**

 + _Italicized text pertains to Rstudio_.
 + If you're on Windows, this guide assumes you will interact with git through git bash, not cmd or Powershell.

## Setup and configuration
There are no Rstudio equivalents for most of these steps.

1. [install git](https://git-scm.com/downloads)
2. Create an account on [github.com](https://github.com/).
2. _Install [R](https://www.r-project.org/) and [Rstudio](https://posit.co/downloads/). Make sure Rstudio is updated_
2. In terminal (Mac, Linux) or git bash (Windows), configure git with your name and email
   + `git config --global user.name '<your name>'`
   + `git config --global user.email '<your email>'`
3. Optional: set your default text editor, if you didn't do that during installation.
   + optional: `git config --global core.editor <nano/vim/emacs>`
4. If you run into trouble with line endings when collaborating, you may need these:
   + Windows users: `git config --global core.autocrlf true`
   + macOS/Linux/Unix users: `git config --global core.autocrlf input`
5. _Make sure Rstudio knows where to find the git executable_
   + _Tools → Global Options → Git/SVN; The path listed under "Git executable" should end with "git" or "git.exe". If it doesn't:_
     + _Mac/Linux terminal: `which git` to reveal the path to the executable; browse to that path in Rstudio_
     + _Windows git bash terminal: `cygpath -w "$(which git)"` to reveal the path to the executable; browse to git.exe in Rstudio_
6. Optional: [Establish secure connection to GitHub from your machine](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys).
   + _This can also be done from Rstudio: Tools → Global Options → Git/SVN → Create SSH Key_

## Create a "remote repository"
**i.e. a project folder hosted on github.com**

1. make a new repository on GitHub
   1. From https://github.com/your_username, click Repositories → New
   2. Name it "git\_demo" and give a short description
   3. Add a README (or do so later)
   4. Optional: give it a default `.gitignore` file for the primary language of the repo. This will prevent git from tracking files that don't need to be version-controlled.
   5. Give your repo a license
      + MIT gives free rein.
      + GNU GPL-3 is "copyleft": all derivatives must also be GPL-3.
   6. Click "Create repository"
   7. Copy the repository URL: click the green "Code" button → HTTPS (unless you've set up SSH) → copy to clipboard

## Simple version control (shell)

2. Clone the repo to your machine
   2. navigate locally to where repo will live, e.g. `cd ~/projects`
   3. `git clone <paste>`
   4. Press enter. now you should have `~/projects/git_demo`
   5. `cd git_demo`
3. Add a new file to the repo
   1. You could use your graphical file explorer, or `nano new_file.txt`
   2. Add some text to the file
   3. Save the file
4. Use `git status` to see what has changed
5. Use `git add new_file.txt` to add the file to the staging area, or `git add .` to recursively add all files in the current directory.
   + It's good practice to verify each action with `git status`, to make sure you're tracking only the files you mean to
6. Use `git commit -m "some descriptive message about what you're committing"` to commit the file. Think of this as a checkpoint. You can return to it any time.
   + `git status` again
7. Use `git push origin main` to send your commit to GitHub.
   + "origin" is the default name of your remote repository. You can view its url with `git remote -v`

## _Simple version control (Rstudio)_

2. Clone the repo to your machine
   2. File → New Project → Version Control → Git → paste the repository URL
3. Add a new file to the repo
   1. New File → R Script → choose filename → OK
   2. Add some text to the file
   2. Save the file
5. In the Git tab (usually in the same pane as Environment and History), check the box next to the new file to add it to the staging area.
6. Click "Commit", and a window will appear. In the "Commit message" box, type a descriptive message about what this update incudes. In our case, it can be, "created first file". Think of this as a checkpoint. You can return to it any time.
7. Click "Push" to send your commit to GitHub.

## Branch-based collaboration

More versatile, but you need to be aware of which branch you're working in. Can be managed entirely through Rstudio.

1. Make sure you don't have any unstaged changes (`git status` or _check the Git tab_).
2. Create a new branch with your name, e.g. `git checkout -b mike`, _or click New Branch, type your name, click Create_
5. Everyone create a file named `\<your name\>.txt` . Add a few lines of text to it. Then add, commit, push.
6. Everyone initiate a pull request from your branch to the main branch on GitHub.
   1. Select the new branch from the dropdown all the way to the left of the green Code button.
   1. Click "Contribute" → "Open pull request"
   2. Provide title and description. You can automatically close issues with e.g. "closes #5" in your description ([details](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/using-keywords-in-issues-and-pull-requests)).
   3. "Create pull request". Confirm.
7. Everyone merge someone else's PR.
8. Everyone `git pull upstream main` _or switch back to the main branch and click Pull_ to update local code.

## Fork-based collaboration

Generally safer for new users, because you don't have to remember which branch you're on, but harder to do through Rstudio. 

1. Optional: create a GitHub organization to centralize your upstream repo. Otherwise a supervisor or lead team member can create the upstream repo.
2. Collaborators fork the repo on GitHub: Click "Fork" → "Create Fork"
3. Clone the fork just like you cloned git\_demo above.
4. Establish connection to upstream.
   1. Green "Code" button → copy to clipboard
   2. In your repo: `git remote add upstream <paste>`
   3. Verify origin and upstream urls with `git remote -v`
5. Everyone create a file named `\<your name\>.txt`. Add a few lines of text to it. Then add, commit, push to origin.
6. Everyone initiate a pull request from your fork on GitHub.
   1. Click "Contribute" → "Open pull request"
   2. Provide title and description. You can automatically close issues with e.g. "closes #5" in your description ([details](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/using-keywords-in-issues-and-pull-requests)).
   3. "Create pull request". Confirm.
7. Everyone merge someone else's PR.
8. Everyone `git pull upstream main` to update local code.

## Merge conflict resolution

Merge conflicts can arise when people are working on the same file at the same time. It's unlikely that you'll encounter one outside of a collaboration, but not impossible.

1. Commit a file, but don't push it.
2. Edit that same file on GitHub (by the way, there's never a need to do this! Recognize that we're only using this feature to _create_ a merge conflict. That's about all it's good for. 
3. Now pull. Merge conflict!
4. Resolution routine depends on editor/IDE. But in general, git adds text to your file indicating which lines are in conflict. Conflicted sections appear like this:

```
<<<<<<< ...
some code you wrote, or that a collaborator wrote
=======
some other code
that conflicts with it
>>>>>>> ....
```

Your job is to delete one variant of the conflicted code, and delete all of the <<</===/>>> lines that were inserted by git, then save, add, commit. Conflict resolved!

### Notes

 + **Warning**: Do not let git keep track of files that contain secrets, like passwords or API keys. 
 + **Notice**: Git is not intended for keeping track of large (>100MB) files. Put these in e.g. `data/` and then add a line with `"data/" to the `.gitignore` file.
 + Other git commands you will find useful:
   + `git help`
   + `git log`
   + `git diff`
   + `git show`
   + `git merge`
   + `git checkout`
 + You can almost always undo changes. That's the main reason for using git. For example, to undo your last commit and return all changes to the stage, use `git reset --soft HEAD~1`
 + Aliases make it easier to type common or arcane commands. Here are some good ones:

 `git config --global alias.slog log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cD) %C(bold blue)<%an>%Creset' --abbrev-commit`
 `git config --global alias.c  "commit -m"`

 After running those config lines, you can type `git slog` instead of `git log` to show a compact, informative log. You can also type `git c "message"` instead of `git commit -m "message"`. 

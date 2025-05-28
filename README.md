# git_workshop
A repo for git demonstrations

## Setup

1. [install git](https://github.com/git-guides/install-git)
2. Configure git with your name and email, and enable clean handling of OS-specific line endings
   + `git config --global user.name '<your name>'`
   + `git config --global user.email '<your email>'`
   + Windows users: `git config --global core.autocrlf true`
   + macOS/Linux/Unix users: `git config --global core.autocrlf input`
   + optional: `git config --global core.editor <nano/vim/emacs>`
3. [Establish secure connection to GitHub from your machine](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

## Simple version control

1. make a new repo (on GitHub)
   1. From https://github.com/your_username, click Repositories -> New
   2. Name it "git\_demo" and give a short description
   3. Add a README (or do so later)
   4. Optional: give it a default `.gitignore` file for the primary language of the repo. This will prevent git from tracking files that don't need to be version-controlled.
   5. Give your repo a license
      + MIT gives free rein.
      + GNU GPL-3 is "copyleft": all derivatives must also be GPL-3.
   6. Click "Create repository"
2. Clone the repo to your machine
   1. green "Code" button -> copy to clipboard
   2. navigate locally to where repo will live, e.g. `cd ~/projects`
   3. `git clone <paste>`
   4. Press enter. now you should have `~/projects/git_demo`
   5. `cd git_demo`
3. Add a new file to the repo
   1. You could use your graphical file explorer, or `nano new_file.txt`
   2. save the file
4. Use `git status` to see what has changed
5. Use `git add new_file.txt` to add the file to the staging area, or `git add .` to recursively add all files in the current directory.
   + It's good practice to verify each action with `git status`, to make sure you're tracking only the files you mean to
6. Use `git commit -m "some descriptive message about what you're committing"` to commit the file. Think of this as a checkpoint. You can return to it any time.
   + `git status` again
7. Use `git push origin main` to send your commit to GitHub.
   + "origin" is the default name of your remote repository. You can view its url with `git remote -v`

## Fork-based collaboration

Note that branch-based collaboration is more common in professional teams of developers, as it enables easier access to other people's development code. For new git users, or teams that don't need access to each other's development code, forks are safer.

1. Optional: create a GitHub organization to centralize your upstream repo. Otherwise a supervisor or lead team member can create the upstream repo.
2. Collaborators fork the repo on GitHub: Click "Fork" -> "Create Fork"
3. Clone the fork: green "Code" button -> copy to clipboard, navigate locally to where repo will live, `git clone <paste>`
4. Establish connection to upstream.
   1. Green "Code" button -> copy to clipboard
   2. In your repo: `git remote add upstream <paste>`
   3. Verify origin and upstream urls with `git remote -v`
5. Everyone create a file named \<your name\>. Add a few lines of text to it. Then add, commit, push to origin.
6. Everyone initiate a pull request from your fork on GitHub.
   1. Click "Contribute" -> "Open pull request"
   2. Provide title and description. You can automatically close issues with e.g. "closes #5" in your description ([details](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/using-keywords-in-issues-and-pull-requests)).
   3. "Create pull request". Confirm.
7. Everyone merge someone else's PR.
8. Everyone `git pull upstream main` to update local code.
9. **Merge conflict demo**: merge conflicts can arise when people are working ont the same file at the same time.
   1. First we have to generate a conflict. Everyone edit the same line in one person's file. Save, add, commit.
   2. The supervisor should also edit that line, then push and PR the edit. Anyone can accept the PR.
   3. Now everyone `git pull upstream main`. Merge conflict!
   4. Resolution routine depends on editor/IDE. Walk through together.


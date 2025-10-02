## Installing Git
### Windows
- The simplest and easiest way to install Git on windows is through the official Git website: https://git-scm.com/downloads
- Alternatively, if you are more comfortable using the Windows terminal:
	1. Press the windows key
	2. Type `cmd` and press `Enter`
	3. The terminal window will open and you can then run the following command:
```PowerShell
  winget install -e --id Git.Git
  ```
- If you installed Git by downloading the `.exe` file from the website, follow the installation instructions, and in the Git installer, keep all the installation options default. (Just press next for everything). Don't change any of the options.
- Once Git is installed, the rest of the commands in this tutorial will be run in the 'Git Bash' program.
- You can run this program by:
	1. Pressing the windows key
	2. Typing `Git Bash` and pressing `Enter` or by clicking on the 'Git Bash' program.
### Linux
- If you are using Linux you can install Git through your Operating Systems' package manager.
- Download the `git` package.
- For example, on Debian/Ubuntu based systems:
```zsh
sudo apt install git
```
### MacOS
- If you are using MacOS then you can install Git by:
	1. Pressing `Command/cmd` + `Space`
	2. Typing `terminal` and pressing enter.
	3. Then when the terminal window appears, type:
```zsh
brew install git
```
- If you get a `homebrew not found` or `homebrew not installed` error you can run the following command to install it:
```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
- (don't worry if you don't know what it means)

## Configuring Git
- After installing Git, we must configure it a little. For now all we need to provide, is a name, and an email. This is so git knows who the 'author' of our operations is. This is useful later down the line if you are working with a team, as we have to know who does what to our repository.
- We can set our name and email in our git config (configuration) with the following commands. Replace with your email and your name.
- The `--global` just means set this option for all of your git projects.
```zsh
git config --global user.email "example@gmail.com"
git config --global user.name "John Doe"
```
## Creating a Git repository
- A Git repository is a directory that has its files tracked by Git.
- We can create a repository with the following commands and all the following commands are run inside the terminal:
```zsh

# create a directory we want to have our repository in
mkdir -p my-git-repository

# change the current working directory into our new directory
cd my-git-repository

# now we run the 'git init' command to intialise a git repository in the directory we are in
git init

# now if we list all the directories (files/folders) in the directory we are in
ls

# we notice that the directory appears empty
# however there is a hidden `.git` file that tracks everything git does to our directory
# we can find this file by running
ls -a

# (this means list all the directories, even hidden ones like `.git`)
```

## Adding files to be tracked by Git
- 'Tracking' a file in Git means that git notices the changes made to the file and keeps track of them.
- We can use the following commands to add files to be tracked by Git.
```zsh
# create a file to be tracked (is empty initially)
touch myFile.txt

# add some text to the file (alternatively you could open it in an editor such as Visual Studio Code or even Notepad)
echo "Hello World!" >> myFile.txt

# currently our file is not being tracked even if it's in the same directory as our git repository
# we must tell git that we want the file to be tracked
git add myFile.txt

# or alternatively, we can tell git that we want all the files and folders in our directory to be tracked at once!
git add .

# the '.' means the directory and all the subdirectories
```

## Getting the status of our Git repository
- At this stage we have one file in our directory and it is being tracked by Git
- We can get the status of the Git repository with the following command:
```zsh
git status
```
- After running this `git status` command we should see the output:
```zsh
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   myFile.txt
```
- This tells us a few things:
	1. We are on the master branch (we can worry about this later)
	2. We have not made any commits yet (also comes later)
	3. Changes to be committed, and here we can see our `myFile.txt`.
- Here we introduce the concept of staging
	- Staged files are files that are being actively tracked by Git.
	- However, when we added our file to be tracked by Git. We are only telling Git to track the file in it's current state when we run the command.

## Git `diff`
- The `git diff` command allows us to see the difference between the staged versions of our files (the snapshots) and the working tree (the current state of the files, possibly newer than the staged versions if we haven't run `git add` yet)
- For instance, if we were to change (add, modify, or remove) text from our file. In this case add:
```zsh
echo "Some more text" >> myFile.txt
```
- Then run:
```zsh
git diff
```
- We have the following output:
```txt
diff --git a/myFile.txt b/myFile.txt
index 980a0d5..faffb51 100644
--- a/myFile.txt
+++ b/myFile.txt
@@ -1 +1,2 @@
 Hello World!
+Some more text
```
- What does this tell us?
	1. The first line tells us we are getting the difference between the file and itself, (what we have changed within the file)
	2. The only lines that matter to us are the second last and last lines. We can see the `"Hello World!"` line we originally created, along with `+Some more text`.
- We didn't type `+` so where did this come from?
	1. This is Git telling us that the difference between the version it has tracked and the state it is right now, is the addition of the line `Some more text`.
	2. Git works on tracking a line by line difference so it relays added lines with `+` and removed lines with `-`. If we change a line it treats it as us removing the line, then adding a new line with the changes we made.
- If we now check the status of our repository using `git status`:
```zsh
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   myFile.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   myFile.txt
```
- This new section at the bottom is telling us that we have some changes that haven't been staged yet.
- To fix this we run `git add myFile.txt` to stage this second change.
- When we run `git diff` we do not get an output. This means that there is no difference in the staged version of `myFile.txt` and the file we have in our working tree.

## Git Commit
- So far we have 'staged' our changes but we haven't 'saved' them. In Git we save a snapshot of our changes using the command `git commit`
- We can use the commit command as follows:
```zsh
git commit -m "Your message for the commit goes here"
```
- The `-m` in our command means message and the following string after it contains the message we want to associate with our commit. It usually explains the changes made in our commit.
- In our case we can run:
```zsh
git commit -m "adding myFile"
```
- If we were to now run `git status` we would get the output:
```txt
On branch master
nothing to commit, working tree clean
```
- The last line means that there are no changes between the 'committed' version of our directory and the working tree.
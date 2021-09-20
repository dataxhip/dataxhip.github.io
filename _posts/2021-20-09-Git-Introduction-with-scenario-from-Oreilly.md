

# Git Init
To store a directory under version control you need to create a repository. With Git you initialise a repository in the top-level directory for a project.

### git init 

##Git Status
#When a directory is part of a repository it is called a Working Directory. A working directory contains the latest downloaded version from the repository together with any changes that have yet to be committed. 
#As you're working on a project, all changes are made in this working directory.

### git status

All files are *untracked* by Git until it's been told otherwise. The details of how is covered in the next step.

### Git Add
To save, or commit, files into your Git repository you first need to add them to the staging area. 
Git has three areas, a working directory, a staging area and the repository itself.
Users move, otherwise referred to as promote, changes from the working directory, to a staging area before committing them into the repository.

One of the key approaches with Git is that commits are focused, small and frequent. 
The staging area helps to maintain this workflow by allowing you to only promote certain files at a time instead of all the changes in your working directory

### Git Ignore
Sometimes there are particular files or directories you never want to commit, such as local development configuration. To ignore these files you create a .gitignore file in the root of the repository.

The .gitignore file allows you to define wildcards for the files you wish to ignore, for example *.tmp will ignore all files with the extension *.tmp*
Any files matching a defined wildcard will not be displayed in a git status output and be ignored when attempting the git add command

```shell
echo "* .tmp" > .gitignore ***
```


```git
git add .gitignore
git commit -m "gitignore file"*
```


# SCENARIO 2



```
git status
git add <file>... to update what will be committed)
git checkout -- <file>...  to discard changes in working directory)
```


**Git Diff**
The command git diff enables you to compare changes in the working directory against a previously committed version. By default the command compares the working directory and the HEAD commit.


If you wish to compare against an older version then provide the commit hash as a parameter, for example git diff <commit>. Comparing against commits will output the changes for all of the files modified. 
If you want to compare the changes to a single file then provide the name as an argument such as git diff committed.js.

By default the output is in the combined diff format. The command git difftool will load an external tool of your choice to view the differences

If you rename or delete files then you need to specify these files in the add command for them to be tracked. 
Alternatives you can use **git mv** and **git rm** for git to perform the action and include update the staging area.


## Staged Differences
Once the changes are in the staging area they will not show in the output from git diff. By default, git diff will only compare the working directory and not the staging area.

To compare the changes in the staging area against the previous commit you provide the staged parameter *git diff --staged*. This enables you to ensure that you have correctly staged all your changes.


## Git Log
The command git log allows you to view the history of the repository and the commit log.

#### Protip
The format of the log output is very flexible. For example to output each commit on a single line the command is *git log --pretty=format:"%h %an %ar - %s"*. More details can be found in the git log man page accessed using git log --help

## Git Show
While git log tells you the commit author and message, to view the changes made in the commit you need to use the the command git show

Like with other commands, by default it will show the changes in the HEAD commit. Use git show <commit-hash> to view older changes.



# SCENARIO 3, Working Remotely

With Git being a distributed version control system it means the local repository contains all the logs, files and changes made since the repository was initialised. To ensure that everyone is working on the most recent version changes need to be shared. When sharing these changes with other repositories only the differences will be synced making the process extremely fast.

## Git Remote
Remote repositories allow you to share changes from or to your repository. Remote locations are generally a build server, a team members machine or a centralised store such as Github.com. Remotes are added using the git remote command with a friendly name and the remote location, typically a HTTPS URL or a SSH connection for example https://github.com/OcelotUproar/ocelite.git or git@github.com:/OcelotUproar/ocelite.git.


The friendly name allows you to refer to the location in other commands. Your local repository can reference multiple different remote repositories depending on your scenario.

### Task
This environment has a remote repository location of /s/remote-project/1. Using git remote, add this remote location with the name origin.

```git
git remote add origin /s/remote-project/1
```


## Git Push
When you're ready to share your commits you need to push them to a remote repository via git push. A typical Git workflow would be to perform multiple small commits as you complete a task and push to a remote at relevant points, such as when the task is complete, to ensure synchronisation of the code within the team.

The git push command is followed by two parameters. The first parameter is the friendly name of the remote repository we defined in the first step. The second parameter is the name of the branch. By default all git repositories have a master branch where the code is worked on.

### Task
Push the commits in the master branch to the origin remote.
```
git push origin master
```



## Git Pull
Where **git push** allows you to push your changes to a remote repository, **git pull** works in the reverse fashion. git pull allows you to sync changes from a remote repository into your local version.

The changes from the remote repository are automatically merge into the branch you're currently working on.

### Task
Pull the changes from the remote into your master branch.

```
git pull origin master
```

## Git log
As described in the previous scenario you can use the git log command to see the history of the repository. The git show command will allow you to view the changes made in each commit.

In this example, the output from git log shows a new commit by "DifferentUser@JoinScrapbook.com" with the message "Fix for Bug #1234". The output of git show highlights the new lines added to the file in green.

### Protip
Use the command git log --grep="#1234" to find all the commits containing #1234


## Git Fetch
The command git pull is a combination of two different commands, **git fetch** and **git merge**. Fetch downloads the changes from the remote repository into a separate branch named remotes/<remote-name>/<remote-branch-name>. The branch can be accessed using git checkout.

Using git fetch is a great way to review the changes without affecting your current branch. The naming format of branches is flexible enough that you can have multiple remotes and branches with the same name and easily switch between them.

The following command will merge the fetched changes into master.

git merge remotes/<remote-name>/<remote-branch-name> master
We'll cover merging in more detail in a future scenario.

Task
Additional changes have been made in the origin repository. Use git fetch to download the changes and then checkout the branch to view them.

Protip
You can view a list of all the remote branches using the command **git branch -r**

```
git fetch

git checkout remotes/origin/master

```

# SCENARIO 4

## Git Checkout
When working with Git, a common scenario is to undo changes in your working directory. The command git checkout will replace everything in the working directory to the last committed version.

If you want to replace all files then use a dot (.) to indicate the current directory, otherwise a list the directories/files separated by spaces.

Task
Use git checkout to clear any changes in the working directory.

```
git checkout .

```

## Git Reset
If you're in the middle of a commit and have added files to the staging area but then changed your mind then you'll need to use the git reset command. git reset will move files back from the staging area to the working directory. If you want to reset all files then use a . to indicate current directory, otherwise list the files separated by spaces.

This is very useful when trying to keep your commits small and focused as you can move files back out of the staging area if you've added too many.

````
git reset HEAD .

````

## Git Reset Hard
A git reset --hard will combine both git reset and git checkout in a single command. The result will be the files removed from the staging area and the working directory is taken back to the state of the last commit.

### Task
Remove the changes from both the staging area and working directory using git reset

### Protip
Using HEAD will clear the state back to the last commit, using git reset --hard <commit-hash> allows you to go back to any commit state. Remember, HEAD is an alias for the last commit-hash of the branch.


git reset --hard HEAD

## Git Revert
If you have already committed files but realised you made a mistake then the command git revert allows you to undo the commits. The command will create a new commit which has the inverse affect of the commit being reverted.

If you haven't pushed your changes then **git reset HEAD 1** has the same affect and will remove the last commit.

### Task
Use git revert to revert the changes in the last commit.

Note, this will open an Vim editor session to create a commit message for each commit. To save the commit message and quit vim type the command :wq for each Vim session.

### Protip
The motivation behind creating new commits is that re-writing history in Git is an anti-pattern. If you have pushed your commits then you should create new commits to undo the changes as other people might have made commits in the meantime.

```
git revert HEAD --no-edit
```

## Git Revert
To revert multiple commits at once we use the character ~ to mean minus. For example, HEAD tild 2 is two commits from the head. This can be combined with the characters ... to say between two commits.

### Task
Use the command git revert HEAD...HEAD~2 to revert the commits between HEAD and HEAD~2.

### Protip
You can use the command git log --oneline for a quick overview of the commit history.

````
git revert HEAD...HEAD~2 --no-edit
````

# Scenario 5 - Fixing Merge Conflicts

## Git Merge
The git fetch command downloads changes into a separate branch which can be checked out and merge. During a merge Git will attempt to automatically combine the commits.

When no conflicts exist then the merge will be 'fast-forwarded' and you won't have to do anything. If a conflict does exist then you will retrieve an error and the repository will be in a merging state.

### Task
In your environment the changes from a remote repository has been fetched.

You now need to merge the changes from origin/master.

This will result in a merge conflict. The conflict indicates the merge failed because both repositories added the file. We'll resolve this in following next steps.

### Protip
By keeping commits small and focused you reduce the likelihood of a merge conflict.

The command git pull is a combination of fetch and merge.

```
git merge remotes/origin/master

```

## 5.2. Viewing Conflict
When a conflict occurs the changes from both the local and remote will appear in the same file in the unix diff format. This is the same format used by git diff.

To read the format, the local changes will appear at the top between <<<<<<< HEAD and ======= with the remote changes being underneath between ======= and >>>>>>> remotes/origin/master.

To resolve the conflict the files need to be edited to match our desired end state. We'll demonstrate this in the next step.

#### 5.2.1 Protip
Git supports different command line and visual merge tools to make resolving conflicts easier. The command **git mergetool** will launch an external tool, we're big fans of **kdiff3**.


## 5.3 Resolving Conflict
The simplest way to fix a conflict is to pick either the local or remote version using git checkout --ours staging.txt or git checkout --theirs staging.txt. If you need to have more control then you can manually edit the file(s) like normal.

Once the files are in the state desired, either manually or using git checkout, then you need stage and commit the changes. When committing a default commit message will be created with details of the merge and which files conflicted.

### Task
Resolving the conflict by selecting the remote changes and complete the merge using git add followed by git commit.

### Protip
If you want to revert in the middle of a merge and try again then use the command git reset --hard HEAD; to go back to your previous state.

Use git commit --no-edit when you wish to use the default commit message.

```
git checkout --theirs staging.txt

git add staging.txt

git commit --no-edit

```


## 5.4 Non-Fast Forward

To simulate a non-fast forward merge the following has occurred.

1) Developer A pulls the latest changes from Developer B.
2) Developer B commits changes to their local repository.
3) Developer A commits non-conflicting changes to their local repository.
4) Developer A pulls the latest changes from Developer B.

In this scenario Git is unable to fast-forward the changes from Developer B because Developer A has made a number of changes.

When this happens, Git will attempt to auto-merge the changes. If no conflicts exist then the merge will be completed and a new commit will be created to indicate the merge happening at that point in time.

The default commit message for merges is "Merge branch '' of ". These commits can be useful to indicate synchronisation points between repositories but also produce a noisy commit log. In the next step we'll investigate alternative approaches.

### Task
Pull the changes from the remote repository and use the default commit message using the command below.

git pull --no-edit origin master

You can view the commits with git log --all --decorate --oneline


## 5.5 Git Rebase
The merge commit messages can be useful to indicate synchronisation points but they can also produce a lot of noise. For example if you're working against local branches and haven't pushed then this additional information is meaningless, and confusing, to other developers looking at the repository.

To solve this you can use git rebase instead of git merge. A rebase will unwind the changes you've made and replay the changes in the branch, applying your changes as if they happened all on the same branch. The result is a clean history and graph for the merge.

Important As rebase will replay the changes instead of merging, each commit will have a new hash id. If you, or other developers, have pushed/pulled the repository then changing the history can git to lose commits. As such you shouldn't rebase commits that have been made public, for example pushing commits then rebasing in older commits from a different branch. The result will be previously public commits having different hash ids. More details can be found at The Perils of Rebasing.

## 5.6 Rebasing Pull Requests

This approach also applies when working with remote branches and can be applied when issuing a pull request using:

git pull --rebase
This will act as if you had done a pull request before each of your commits.



### Scenario 6 - Experiments Using Branches

A branch allows you to work, in effectively, a brand new working directory. The result is that a single Git repository can have multiple different versions of the code-base, each of which can be swapped between without changing directories.

The default branch in Git is called master. Additional branches allow you to perform the same operations and commands as you would on master, such as committing, merging and pushing changes. As additional branches work in the same way as master they are ideal for prototyping and experiments as any changes can be merged to master if required.

When you switch a branch, Git changes the contents of the working directory. This means you don't need to change any configurations or settings to reflect different branches or locations.

## 6.1 Git Branch
Branches are created based on another branch, generally master. The command git branch <new branch name> <starting branch> takes an existing branch and creates a separate branch to work in. At this point both branches are identical.

To switch to a branch you use the git checkout <new branch name> command.

### Task
Create and checkout a new branch called 'new_branch'

### Protip
The command git checkout -b <new branch name> will create and checkout the newly created branch.

```
git branch new_branch master

git checkout new_branch

```

## 6.2 List Branches
To list all the branches use the command git branch.

The additional argument -a will include remote branches while including -v will include the HEAD commit message for the branch

### Task
List all the branches with their last commit message using git branch -va

```
git branch -va
```

## 6.3 Merge To Master
A commit has been made to the new branch. To merge this into master you would first need to checkout the target branch, in this case master, and then use the 'git merge' command to merge in the commits from a branch.

### Task
Merge the commits in your new branch back into master

```
git checkout master

git merge new_branch
```

## 6.4 Push Branches
As we've discussed in previous courses, if you want to push a branch to a remote then use the command git push <remote_name> <branch_name>

## 6.5 Clean Up Branches
Cleaning up branches is important to remove the amount of noise and confusion. To delete a branch you need to provide the argument -d, for example git branch -d <branch_name>

### Task
Now the branch has been merged into master it is no longer required. Delete your new branch to keep your repository clean and understandable.

```
git branch -d new_branch
```


## 7. Scenario 7 - Finding Bugs
Software bugs have been a problem for as long as software has existed. As Git records all the commits to the repository then it becomes a great source of information and diagnostic tool when identifying how issues were introduced.

In this scenario we'll explore the different ways you can find which commit introduced the problem. The environment has been initialised with a Git repository containing a single HTML file which renders a list of items.


## 7.1 Git Diff Two Commits
The git diff command is the simplest to compare what's changed between commits. It will output the differences between the two commits.

Example
You can visually any two commits by providing the two commits hash-ids or pointers (blobs)
```
git diff HEAD~2 HEAD
```


## 7.2 Git log
While git log helps you see the commit messages but by default it does not output what actually changed. Thankfully the command is extremely flexible and the additional options provide useful insights into the history of the repository.

Examples
To see the overview of the commits in a short view use the command git log --oneline

To output the commit information with the differences of what changed you need to include the -p prompt such as git log -p

This will output the entire history. You can filter it with a number of different options. The -n <number> specifies a limit of commits to display from the HEAD. For example 
```
git log -p -n 2 displays HEAD and HEAD~1.
```

If you know the time period then you can use a time period to between commits before a particular date using --since="2 weeks ago" and --until="1 day ago"

As with most Git commands, we can output a range of commits using HEAD...HEAD 1 as shown in the terminal

Use the command git log --grep="Initial" will output all the commits which include the word "Initial" in their commit message. This is useful if you tag commits with bug-tracking numbers.

### Protip
As we discussed in the merging scenario, your commit history can become noisy due to use merge notification commits. To remove them provide the argument -m with git log.


## 7.3 Git Bisect

The git bisect commands allows you to do a binary search of the repository looking for which commit introduced the problem and the regression. In this step we'll find the commit which forgot HTML tags in list.html.

Git bisect takes a number of steps, execute the steps in order to see the results.

Steps
- 1 To enter into bisect mode you use the command git bisect start.

- 2 Once in bisect mode you define your current checkout as bad using git bisect bad. This indicates that it contains the problem your searching to see when it was introduced.

- 3 We've defined where a bad commit happened, we now need to define when the last known good commit was using git bisect good HEAD ~ 5. In this case it was five commits ago.

- 4 Step 3 will checkout the commit in-between bad and good commits. You can then check the commit, run tests etc to see if the bug exists. In this example you can check the contents using cat list.html

- 5 This commit looks good as everything has correct HTML tags. We tell Git we're happy using git bisect good. This will automatically check out the commit in the middle of the last known good commit, as defined in step 5 and our bad commit.

- 6 As we did before we need to check to see if the commit is good or bad. cat list.html

- 7 This commit has missing HTML tags. Using git bisect bad will end the search and output the the related commit id.


The result is that instead of searching five commits, we only searched two. On a much larger timescale bisect can save you signifant time.


## 7.4 Git Blame
While having a "blame" culture isn't desirable, it can be useful to know who worked on certain sections of the file to help with improvements in future. This is where git blame can help.

git blame <file> shows the revision and author who last modified each line of a file.

Example
Running blame on a file will output who last touched each line.
```
git blame list.html
```

If we know the lines which we're concerned with then we can use the -L parameter to provide a range of lines to output.
```
git blame -L 6,8 list.html
```


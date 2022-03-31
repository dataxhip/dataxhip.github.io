# Jira Version Control with Git 


# Git Branches

A branches is a sets of commits that trace back to the project first commit.

To create a branch, we create a reference, it is a piece of file.

```bash
git branch 
# list existing branches

# now creating a branch
git branch <nameOfTheBranch>
# then by launching
git branch
# we can list branches


```
## Git Checkout

It helps to update branch by making it point to the newly created branch

It is necessary to checkout if you want to work on branch after creating it.

It pushes the HEAD to reference to feature (the newly created branch)

It also updates the working tree with the commit's file (the branch ref file)

```bash
# swicth to a branch, let's say featureX
git checkout featureX

# now by executing git branch
# we can see that the HEAD point to featureX instead

# to combine checkout and creating a branch , use the -b command

git checkout -b featureX

# used only for new branches, it fails instead



```
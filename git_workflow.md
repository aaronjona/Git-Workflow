# Git Workflow

By Aaron Chen

Created following the tutorial on <https://youtu.be/uj8hjLyEBmU?si=qnSXlqa-vzmBkCYO> 

## Introduction

The motivation of Git Workflow is to allow simultaneous modifications (e.g. bug fixing or adding new features) of the __*main*__ code in production while keeping the __*main*__ code clean. 

Git Workflow follows three main components: 

1. **Remote** main 
2. __Local__ Git 
3. __Disk__ codes

> __Remote__ branch is the main code in production (running) where we want minimal interactions with. 
>
> __Local__ branch is a connector between __Remote__ and __Disk__ controlled by Git, all modifications to the __Remote__ branch goes through __Local__. 
>
> __Disk__ branch is your local files and codes opened and modified in the IDE. 

## General Workflow  (Code)

```shell
git clone https://github.com/example/example.git 
git checkout -b my-feature					# Create a branch my-feature in Local
## Code in local machine (Disk)
git add <modified_files> 					# Buffer changes
git commit 									# Push all buffered changes into Local
git push origin my-feature 					# Create a copy of my-feature in Remote

### if main branch is modified
git checkout main 							# Revert disk to main branch
git pull origin master 						# Pull the new changes
git checkout my-feature 					# Revert disk to my-feature
git rebase main 					# Note: changes need if there is rebase conflict
git push -f origin my-feature 				# Create a copy of rebased my-feature in Remote
### Resolved

### Create a Pull Request
# Annoy the Senior Engineer with shit code
### Pull Request Approved and Merged

git checkout main							# Revert disk to main branch
git branch -D my-feature					# Delete my-feature branch in Local
git pull origin master						# Pull changes 
```

## General Workflow (Explained)

1. Download production code (__Remote__) to your local machine (__Local__ and __Disk__)

   `git clone https://github.com/example/example.git`

2. Create a feature Branch

   `git checkout -b my-feature`

   This creates a feature branch called my-features exactly the same as the main branch. 

3. Write your code in local IDE (__Disk__)

   You can see the difference in code comparing to the **my-feature** branch with `git diff`.

   Note: All modifications of the code is done locally in your own machine, Git does not yet know in the __Local my-feature__ branch. 

4. Commit the changes into __Local__

   `git add <modified_files>` - notified Git about the files to commit

   `git commit` - changes the files you added in __Local my-feature__ branch

5. Push changes into Github __Remote__

   `git push origin my-feature`

   This pushes the __Local my-feature__ branch into the __Remote__, creating a __Remote my-feature__ branch (with the new code committed)

*If  __Remote__ main branch is modified

6. If the __Remote__ main branch is modified (i.e. someone else merged their new feature)

   We need to test out new features on the newly modified __Remote__ main branch. 

   1. Revert the __Disk__ code back to main

      `git checkout main`

   2. Pull the changes of __Remote__ main to the __Local__ and __Disk__

      `git pull origin master`

      Updates both the main branch in __Local__ and __Disk__ we just reverted. 

      Now __Local__ has two branches, __main__ (with new modification) and __my-feature__ (with my modification committed)

   3. Revert the __Disk__ again to the __my-feature__ branch

      `git checkout my-feature`

   4. Update base code of __my-feature__ with modified __main__ 

      `git rebase main`

      This changes the __my-feature__ commit on top of the modified __main__. 

      Note: Need to manually change code if there is a *rebase conflict*

   5. Push changes into Github (again)

      `git push -f origin my-feature`

7. Ready to Merge into __Remote__ main branch (__Pull Request__)

   Create a __Pull Request__

8. Senior __Approve__ and __Merge__

9. Delete __Remote__ __my-feature__ branch

10. Update __Disk__ file

    `git checkout main`

11. Remove __my-feature__ branch in __Local__

    `git branch -D my-feature`

12. Update the new changes in both __Local__ and __Disk__

    `git pull origin master`




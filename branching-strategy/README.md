Branching strategy
To collaborate on your Terraform code, we recommend using the GitHub flow. This approach uses short-lived branches to help your team quickly review, test, and merge changes to your code. To make changes to your code, you would:

Create a new branch from your main branch
Write, commit, and push your changes to the new branch
Create a pull request
Review the changes with your team
Merge the pull request
Delete the branch
HCP Terraform and Terraform Enterprise can run speculative plans for pull requests. These speculative plans run automatically when you create or update a pull request, and you can use them to see the effect that your changes will have on your infrastructure before you merge them to your main branch. When you merge your pull request, HCP Terraform will start a new run to apply these changes.
https://developer.hashicorp.com/terraform/language/style

The golden rule of rebasing states that you should not rebase a branch that other people may 
have based work on. For example, if you have pushed a branch to the remote repository, then 
it is considered a public branch. This means that other collaborators may also be working 
on this branch in their local repositories, or they may be pushing work to this branch in the 
remote repository.
In this case, you should refrain from rebasing the branch. 

 If there is a possibility that someone else has worked on the branch, then it is recom
mended that you avoid rebasing

If the branch you have merged in a pull request is a topic branch, it is common to delete it 
after the pull request is merged because you can assume that the work on that branch has 
been completed. For new pieces of work, you will make new branches and go through the 
pull request process again. This keeps the remote repository organized and uncluttered by 
old branches

Whenever you create a new branch, it is recommended that you base it off the most up-to-date 
version of the relevant remote branch in the remote repository. Often this will be the main 
branch (or whichever branch your team uses as the primary line of development). However, 
depending on your team’s Git workflow, it may be another branch. 
If you’re working on an existing branch on your own, it is recommended to update your 
branch with the changes made to the relevant remote branch in the remote repository by 
merging. And if you find yourself in a situation where you’re working on the same branch 
as someone else, then you must always make sure to fetch and merge any changes that have 
been made on that branch in the remote repository before continuing to work on it.

“commit early, commit often.” / “Write code atomically” usually means: Make changes in small, self-contained, indivisible units where each change is complete and safe on its own.

 The general rule is to group related changes 
together. This allows you to keep your commits more organized. As you will see in the next 
section, every commit also has an associated commit message. This can be used to provide a 
description of what was updated in a specific commit.

If you’re working on a Git project with other people, you should 
check with your collaborators so you’re sure what they expect you to include in these messages.


"learning git"
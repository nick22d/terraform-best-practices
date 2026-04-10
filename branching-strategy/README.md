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
"learning git"
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
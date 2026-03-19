- linting
- security / tfsec
- terratest / smoke tests
- compliance tests

* running some of those tests in parallel might be possible in order to speed up the CI/CD pipeline

Integration and unit testing
Terraform tests let you validate your modules and catch breaking changes. We recommend that you write tests for your Terraform modules and run them just as you run your tests for your application code, such as pre-merge check in your pull requests or as a prerequisite step in your automated CI/CD pipeline.

Tests differ from validation methods such as variable validation, preconditions, postconditions, and check blocks. These features focus on verifying the infrastructure deployed by your code, while tests validate the behavior and logic of your code itself. For more information, refer to the Terraform test documentation and the Write Terraform tests tutorial.
https://developer.hashicorp.com/terraform/language/style
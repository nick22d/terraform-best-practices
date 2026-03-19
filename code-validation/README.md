Code validation
The terraform validate command checks that your configuration is syntactically valid and internally consistent. The validate command does not check if argument values are valid for a specific provider, but it will verify that they are the correct type. It does not evaluate any existing state.

The terraform validate command is safe to run automatically and frequently. You can configure your text editor to run this command as a post-save check, define it as a pre-commit hook in a Git repository, or run it as a step in a CI/CD pipeline.

For more information, refer to the Terraform validate documentation.
https://developer.hashicorp.com/terraform/language/style


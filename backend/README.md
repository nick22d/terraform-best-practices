# Best practices on Terraform's backend

By default, Terraform stores state locally in a file named terraform.tfstate. When working with Terraform in a team, use of a local file makes Terraform usage complicated because each user must make sure they always have the latest state data before running Terraform and make sure that nobody else runs Terraform at the same time.
https://developer.hashicorp.com/terraform/language/state/remote

1) Set up a remote backend to allow for a collaborative environment (i.e. an S3 bucket or Terraform Cloud). By default, Terraform's backend is local.

2) In an enterprise where an entire team would be expected to deploy resources with Terraform, there is the risk of encountering conflicting changes. To prevent this, enable state locking (i.e. an S3 bucket with a DynamoDB table).

3) Enable versionining on the backend in case it is need to restore to a previous state (i.e. versioning on the S3 bucket hosting the terraform state).

4) Host the terraform state in a dedicated account for complete isolation (i.e. an AWS account labelled as 'infra' or 'shared services'). The terraform state doesn't need to be in the same account in which resources will be provisioned and nor should it be.

5) Ensure that the terraform state is encrypted at rest for privacy (i.e. enabling encryption on the S3 bucket).

6) Do not version control the state file. Simply add it to .gitignore.

7) Ensure that traffic to the state file is encrypted in transit (i.e. an S3 bucket policy that accepts TLS connections only).

8) Ensure that only authorised parties are allowed to take any actions against the terraform state file repository (i.e. be very specific with a bucket policy as to which entities can take which actions against the bucket)/ check for MFA.

9) Make sure that the backend repository is not publicly accessible.

10) Protect the availability of the terraform state file's repository by preventing its deletion (i.e. prevent_destroy in terraform resource, SCP deny policies etc).

11) Use audit logs to track state access over time (https://developer.hashicorp.com/terraform/language/manage-sensitive-data)

12) Both of these protections can be bypassed with the -force flag if you're confident you're making the right decision. Even if using the -force flag, we recommend making a backup of the state with terraform state pull prior to forcing the overwrite. 
terraform state pull > terraform.tfstate.backup
(https://developer.hashicorp.com/terraform/language/state/backends)

For highly regulated environments, also consider keeping a backup of the terraform state in a separate AWS account.

State locking happens automatically on all operations that could write state. You do not see any message that it happens. If state locking fails, Terraform does not continue. You can disable state locking for most commands with the -lock=false flag, but we do not recommend it.

Terraform has a force-unlock command to manually unlock the state if unlocking failed.

Be very careful with this command. If you unlock the state when someone else is holding the lock it could cause multiple writers. Force unlock should only be used to unlock your own lock in the situation where automatic unlocking failed.

To protect you, the force-unlock command requires a unique lock ID. Terraform will output this lock ID if unlocking fails. This lock ID acts as a nonce, ensuring that locks and unlocks target the correct lock.
https://developer.hashicorp.com/terraform/language/state/locking
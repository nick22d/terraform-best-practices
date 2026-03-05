# Best practices on Terraform's backend

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

For highly regulated environments, also consider keeping a backup of the terraform state in a separate AWS account.
Comments
Write your code so it is easy to understand. Only when necessary, use comments to clarify complexity for other maintainers.

Use # for both single- and multi-line comments. The // and /* */ comment syntaxes are not considered idiomatic, but Terraform supports them to remain backward-compatible with earlier versions of HCL.

# Each tunnel is responsible for encrypting and decrypting traffic exiting
# and leaving its associated gateway.
resource "google_compute_vpn_tunnel" "tunnel1" {
  ## ...


https://developer.hashicorp.com/terraform/language/style

Comments
The Terraform language supports three different syntaxes for comments:

# begins a single-line comment, ending at the end of the line.
// also begins a single-line comment, as an alternative to #.
/* and */ are start and end delimiters for a comment that might span over multiple lines.
The # single-line comment style is the default comment style and should be used in most cases. Automatic configuration formatting tools may automatically transform // comments into # comments, since the double-slash style is not idiomatic.
https://developer.hashicorp.com/terraform/language/syntax/configuration
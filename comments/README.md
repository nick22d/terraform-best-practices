Comments
Write your code so it is easy to understand. Only when necessary, use comments to clarify complexity for other maintainers.

Use # for both single- and multi-line comments. The // and /* */ comment syntaxes are not considered idiomatic, but Terraform supports them to remain backward-compatible with earlier versions of HCL.

# Each tunnel is responsible for encrypting and decrypting traffic exiting
# and leaving its associated gateway.
resource "google_compute_vpn_tunnel" "tunnel1" {
  ## ...
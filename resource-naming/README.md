Resource naming
Every resource within a configuration must have a unique name. For consistency and readability, use a descriptive noun and separate words with underscores. Do not include the resource type in the resource identifier since the resource address already includes it. Wrap the resource type and name in double quotes.

❌ Bad:

resource aws_instance webAPI-aws-instance {...}
✅ Good:

resource "aws_instance" "web_api" {...}

https://developer.hashicorp.com/terraform/language/style
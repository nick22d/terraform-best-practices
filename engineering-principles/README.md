- DRY 
- strong isolation / reduce the blast radius / separation of concerns
- clear governance
- kiss
- things that go together live together
- reusable modules
- deterministic infrastructure
- good documentation (descriptions, tags, comments, comment blocks, README files) / readability / maintainability

Write your description from the perspective of a module consumer to help your users understand how to use this variable. If you need to leave commentary about a variable block as a module maintainer, use a comment instead.
https://developer.hashicorp.com/terraform/language/block/variable




- build the infrastructure with two layers (first layer is the one terraform needs like backend etc while the second layer is about the actual infrastructure being deployed)

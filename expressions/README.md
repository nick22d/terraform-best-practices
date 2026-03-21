When using template directives, we recommend always using the "heredoc" string literal form and then formatting the template over multiple lines for readability. Quoted string literals should usually include only interpolation sequences.
https://developer.hashicorp.com/terraform/language/expressions/strings

Filesystem and Workspace Info
The following values are available:

path.module is the filesystem path of the module where the expression is placed. We do not recommend using path.module in write operations because it can produce different behavior depending on whether you use remote or local module sources. Multiple invocations of local modules use the same source directory, overwriting the data in path.module during each call. This can lead to race conditions and unexpected results.

path.cwd is the filesystem path of the original working directory from where you ran Terraform before applying any -chdir argument. This path is an absolute path that includes details about the filesystem structure. It is also useful in some advanced cases where Terraform is run from a directory other than the root module directory. We recommend using path.root or path.module over path.cwd where possible.

Aside from path.module, we recommend using the values in this section only in the root module of your configuration. If you are writing a shared module which needs a prefix to help create unique names, define an input variable for your module and allow the calling module to define the prefix. The calling module can then use terraform.workspace to define it if appropriate, or some other value if not:

module "example" {
  # ...

  name_prefix = "app-${terraform.workspace}"
}
https://developer.hashicorp.com/terraform/language/expressions/references

Equality Operators
The equality operators both take two values of any type and produce boolean values as results.

a == b returns true if a and b both have the same type and the same value, or false otherwise.
a != b is the opposite of a == b.
Because the equality operators require both arguments to be of exactly the same type in order to decide equality, we recommend using these operators only with values of primitive types or using explicit type conversion functions to indicate which type you are intending to use for comparison.
https://developer.hashicorp.com/terraform/language/expressions/operators

For example, the following expression is valid and will always return a string, because in Terraform all numbers can convert automatically to a string using decimal digits:

var.example ? 12 : "hello"

Relying on this automatic conversion behavior can be confusing for those who are not familiar with Terraform's conversion rules though, so we recommend being explicit using type conversion functions in any situation where there may be some uncertainty about the expected result type.
https://developer.hashicorp.com/terraform/language/expressions/conditionals

For sets of other types, Terraform uses an arbitrary ordering that may change in future versions. We recommend converting the expression result into a set to make it clear elsewhere in the configuration that the result is unordered. You can use the toset function to concisely convert a for expression result to be of a set type.

toset([for e in var.set : e.example])
https://developer.hashicorp.com/terraform/language/expressions/for

This special behavior of splat expressions is not obvious to an unfamiliar reader, so we recommend using it only in for_each arguments and similar situations where the context implies working with a collection. Otherwise, the meaning of the expression may be unclear to future readers.

Earlier versions of the Terraform language had a slightly different version of splat expressions, which Terraform continues to support for backward compatibility. This older variant is less useful than the modern form described above, and so we recommend against using it in new configurations.

Notice that with the attribute-only splat expression the index operation [0] is applied to the result of the iteration, rather than as part of the iteration itself. Only the attribute lookups apply to each element of the input. This limitation was confusing some people using older versions of Terraform and so we recommend always using the new-style splat expressions, with [*], to get the more consistent behavior.
https://developer.hashicorp.com/terraform/language/expressions/splat

Best Practices for dynamic Blocks
Overuse of dynamic blocks can make configuration hard to read and maintain, so we recommend using them only when you need to hide details in order to build a clean user interface for a re-usable module. Always write nested blocks out literally where possible.

If you find yourself defining most or all of a resource block's arguments and nested blocks using directly-corresponding attributes from an input variable then that might suggest that your module is not creating a useful abstraction. It may be better for the calling module to define the resource itself then pass information about it into your module. For more information on this design tradeoff, see When to Write a Module and Module Composition.
https://developer.hashicorp.com/terraform/language/expressions/dynamic-blocks

The only situation where it's appropriate to use any is if you will pass the given value directly to some other system without directly accessing its contents. For example, it's okay to use a variable of type any if you use it only with jsonencode to pass the full value directly to a resource, as shown in the following example:
https://developer.hashicorp.com/terraform/language/expressions/type-constraints
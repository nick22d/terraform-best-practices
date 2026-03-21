# Terraform Best Practices

## Overview

Terraform remains one of the most popular IaC tools in the industry whose functionality only keeps growing. Although it is impossible to follow a "one-size-fits-all" approach when it comes to building infrastructure, it should be feasible to define some general best practices that guide us when designing, implementing and managing infrastructure. This project is a collection of some of the most important best practices to keep in mind when using Terraform.

---

## Structure

![Diagram](./assets/images/structure.png)

This project has been structured in the following way:

1) At the top, we have defined a set of engineering principles. These principles function as the drivers behind all of our infrastructure engineering decisions.

2) Each engineering principle is divided into several subcategories of best practices (i.e. module management, version pinning etc). 

3) Finally, each subcategory is a collection of individual best practices with each best practice referring to a specific technique (i.e. encrypting the terraform state at rest).

**Note:** Many best practices are related to more than one engineering principle.

## Engineering Principles

**[Evolutionary Design](./docs/principles/evolutionary_design.md)** | **[YAGNI](./docs/principles/yagni.md)** | **[KISS](./docs/principles/kiss.md)** | **[Separation of Concerns](./docs/principles/separation_of_concerns.md)** | **[Everything as Code](./docs/principles/everything_as_code.md)**
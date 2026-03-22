<h1 align="center">Terraform Best Practices</h1>

<p align="center">
    <img src="assets/images/root-image.png">
</p>

---

<h2 align="center">Overview</h2>

Terraform remains one of the most popular IaC tools in the industry whose functionality only keeps growing. Although it is impossible to follow a "one-size-fits-all" approach when it comes to building infrastructure, it should be feasible to define some general best practices that guide us when designing, implementing and managing infrastructure. This project is a collection of some of the most important best practices to keep in mind when using Terraform.

---

<h2 align="center">Structure</h2>

<p align="center">
    <img src="assets/images/structure.png">
</p>

<br>
<br>

<div align="center">
This project has been structured in the following way:
</div>

<br>

**1)** At the top, we have defined a set of engineering principles. These principles function as the drivers behind all of our infrastructure engineering decisions.

**2)** Each engineering principle is divided into several subcategories of best practices (i.e. module management, version pinning etc). 

**3)** Finally, each subcategory is a collection of individual best practices with each best practice referring to a specific technique (i.e. encrypting the terraform state at rest).

**Note:** Many best practices are related to more than one engineering principle.

---

<h2 align="center">Engineering Principles</h2>

<br>
<br>

<div align="center">

**[Evolutionary Design](./docs/principles/evolutionary_design.md)** | 
**[YAGNI](./docs/principles/yagni.md)** | 
**[KISS](./docs/principles/kiss.md)** | 
**[Separation of Concerns](./docs/principles/separation_of_concerns.md)** | 
**[DRY](./docs/principles/everything_as_code.md)** | 
**[Determinism](./docs/principles/everything_as_code.md)** | 
**[Consistency](./docs/principles/everything_as_code.md)** |
**[Maintainability](./docs/principles/everything_as_code.md)**

</div>
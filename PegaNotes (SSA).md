# Pega SSA Notes (Senior System Architect)

## Contents

* [Enterprise Class Structure](#enterprise)
  * [Enterprise Class Structure Layers](#layers)
* [Managing Application Development](#dev)

<a name="enterprise"></a>
## Enterprise Class Structure

Pega lets you organize your application in the same way as your business. You can reuse common policies and procedures while allowing for differences between products, regions, channels, and customer segments.

This is acheived by using a class hierarchy structure called **Enterprise Class Structure (ECS)**.

From top to bottom this is:

* Application-specific policies and procedures
* Division-specific policies and procedures
* Enterprise-specific policies and procedures

Any rule in an ECS layer can be shared across multiple applications, and can be edjusted as business operations change. ECS also enforces best practice around reuse and standardization as a system expands.

<a name="layers"></a>
### Enterprise Class Structure Layers

#### Pega Platform Layer

The Pega Platform layer contains the built-in assets necessary for processing cases and other work in Pega applications. This layer also contains the assets Pega Platform uses.

#### Organization Layer

Contains assets used on an enterprise-wide basis. This includes rules for things like standard properties, decision tables, and service level rules. 

Also contains data such as assets and rules for data stored in the system, and data stored for accessing external system data. e.g. access to an external customer database is an integration point that may be added to the Organization layer.

#### Division Layer

Contains assets used on a division-wide basis. This is the middle level of the **(Org-Div-Unit)** heirarchy and is available for use in every application.

Assets in this layer may apply to a line of business, region, or brand. A rule here is available to all applications within the division through pattern inheritance. 

#### Framework and Implementation Layers

The **Framework** layer contains assets that can be extended in specific implementations. This could be a standard Pega application for a certain industry, or an extendable custom application.

The **Implementation** layer defines an application customized for a specific division or line of business. This may extend one or more framework layer applications.

For example, a dealer may have two brands, one focussed on luxury cars, and one on value for money cars. Each brand needs to manage sales. The dealer can use a generalized application that manages sales in the framework layer. Each brand can then extend this on the implementation layer to customize their own sales process. Each custom application contains brand-specific assets, such as styling and policies.

<a name="dev"></a>
## Managing Application Devlopment (Unit 7)

A **branch** in Pega is a container for rulesets with records that are undergoing rapid changes. Rulesets associated with a branch are called **branch rulesets**.

Branches are usually created for each team and allow development to occur without impacting other teams' work.


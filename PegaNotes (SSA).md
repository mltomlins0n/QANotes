# Pega SSA Notes (Senior System Architect)

## Contents

* [Enterprise Class Structure](#enterprise)
  * [Enterprise Class Structure Layers](#layers)
* [Managing Application Development](#dev)
* [Circumstancing](#circumstance)

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

<a name="circumstance"></a>
## Circumstancing (Unit 11)

**Circumstancing** is creating a variant of a rule to manage certain exceptions in otherwise normal processes. e.g. customers with "basic" membership receive responses after 24hours, but those with "elite" status receive a response after 6 hours. Instead of creating a separate rule for elite customers, you take advantage of **circumstancing**.

This allows you to customize the behaviour of an application to address multiple exceptions using a collection of targeted rules rather that a single complex rule.

Circumstancing establishes a baseline for expected case behavior and adds variants to address exceptions to the behavior. The goal of circumstancing is to create a variant for each anticipated situation. Pega selects the appropriate variant, or circumstance, to use based on the details of the case.

You can circumstance a rule according to the value of one or more conditions. You define a condition based on one variable, multiple variables, or the processing date, then apply the condition to a variant of the rule. When using the rule, the application evaluates the conditions defined on all the circumstanced variants. If one of the circumstancing conditions is satisfied, the application uses the corresponding rule variant. Otherwise, the application uses the base rule.

There are 4 circumstancing conditions:

* **Single value** - the rule variant is effective whenever the value of a single property satisfies the circumstancing condition. You specify the property to evaluate a comparison value when circumstancing a rule. If the value of the property matches the specified value for a case, the application applies the circumstanced variant of the rule, rather than the base rule.

* **Multiple value** - the rule variant is effective whenever a combination of property values satisfies the circumstancing condition. Multiple value circumstances are based on a circumstance **template** and circumstance **definition**. The circumstance **template** defines the properties on which to circumstance a rule. The circumstance **definition** defines the combination of conditions in which a property uses a variant of a rule. You apply the circumstance template and circumstance definition to the rule variant. If the case matches a combination in the circumstance definition, the application uses the circumstanced variant of the rule, rather than the base rule. This is also called **multivariate circumstancing**.

* **Date property** - the rule variant is effective whenever the value of a date property satisfies the circumstancing condition. This condition can be either a **single date** or a **range of dates**. If the value of the property is **later** than the specified date or **falls within the range of dates**, the application uses the circumstanced variant of the rule, rather than the base rule.

* **As-of date** - the rule variant is effective **after** a certain date, or during a **range of dates**. After the speified date or during the specified range, the application applies the circumstanced variant of the rule, rather than the base rule.

To circumstance a rule based on one variable, select cirumstance by **Property and Date** when creating the rule variant.

To circumstance based on multiple variables, select circcumstance by **Template**. 

For each cirumstanced rule you must also provide a unique **circumstance definition**. This defines the values for the circumstance template.

### Overriding a Circumstanced Rule

Circumstanced rules can be overidden in 2 ways:

* **Flagging a base rule** - the base rule incdicates that this version of the rule is now considered this rule's base and any previous circumstances no longer apply.

* **Withdrawing a rule** - A withdrawn rule hides previous circumstanced versions of a rule and reverts to the base rule.

When selecting which rule to use, Pega looks to the newest ruleset version **regardless of circumstancing**. e.g. If your have two rulesets, **01-01-42 (no circumstance)**, and **01-01-20 (circumstance you want to use)**, Pega will run the most recent rulset. In this case, **01-01-42**.

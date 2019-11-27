# Pega SSA Notes (Senior System Architect)

## Contents

* [Enterprise Class Structure](#enterprise)
  * [Enterprise Class Structure Layers](#layers)
* [Application Versioning](#version)
* [Managing Application Development](#dev)
* [Circumstancing](#circumstance)
* [Duplicate and Temporary Cases](#cases)
* [Parallel Processing](#parallel)
  * [Case Locking](#lock)
* [Flow Action Processing](#flow)
* [Decision Tables and Trees](#decision)
* [Case Approval](#approve)
* [Organizational Records](#organize)
* [Configuring Field Values](#fields)
* [Application Accessibility](#access)
___

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

___

<a name="version"></a>
## Application Versioning

**When creating a new application version it is best practive ensure that the rules for the new version are checked in, and to lock all but the highest ruleset versions before proceeding**.

Pega provides two methods for creating new versions of an application. Each method preserves prior application versions. Application versioning is a way to differentiate current and past application configurations. Rule resolution can look through all the minor and patch versions for the current major ruleset.

Application components include the application **ruleset stack** — this contains the rules and data types used by the application. To version an application, you must version the application's rulesets.

The versioning methods are **lock and roll** and **skimming**. The act of using a version method begins a release cycle. Every major version, minor version, and patch version represents a release cycle. Both methods list the highest version, and offers to roll the ruleset to a still-higher version by default.

**Loack and Roll** is best for incrementing **patch** versions, and **Skimming** is better for **major** and **minor** versions.

### Lock and Roll

When using this method you create a new empty ruleset version and copy the necessary rules into that new version.

When updating a version, Pega checks the previous minor version to find existing rules. Pega cannot find rules if they exist in a different **major** version.

There are 3 options when updating the application rule:

* **Do not update my application** is used when applying a patch update.

* **Update my Application to include the new Rulset Versions** is used when you are rolling out a new minor version, or when the application rule lists the patch number.

* **Create a new version of my application** is used when:
  * You want to create a new application rule. This could be creating a new version or description of the application.

  * You want to allow access the current and new versions of the application, during a test peroid or phased roll out for example.

### Skimming

Skimming saves the highest version of a ruleset into a new, higher ruleset version; and applies mainly to resolved rules. The two types of skimming are **major** and **minor** skimming, and these correspond to the update types.

If a rule is available, blocked, final, or withdrawn, it is carried forward when skimming. The only exception is that withdrawn rules **are not carried over** in a major skimming update.

After performing a skimming update, you must update:

* Application rules
* The required rulsesets and versions array in RuleSet version rules
* Access groups, to reference the new major version

___

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

___

<a name="cases"></a>
## Duplicate and Temporary Cases (Unit 13)

### Duplicate Cases

Duplicate cases can be found using the **search duplicate cases** step in a case life cycle. When a case enters the step, the system uses **basic** and **weighted** conditions to compare specific property values with cases already present in the system. 

All basic conditions must be met before considering potential duplicate cases. Once all basic conditions are met, the system continues to evaluate the weighted conditions to receive a weight value. Each condition has a weight (between 1 and 100) to determine the relative importance of a condition when making the comparisons. 

The system adds up the weights of all the conditions that evaluate to true. If the sum exceeds a specified threshold value, the system flags the current case as a potential duplicate. 

The search duplicate cases process then displays to the user the current case and the matching case in the system. The user may identify a potential duplicate from the list and resolve the current case as a duplicate. The system does not process the case further. The user may also decide that the current case is not a duplicate and may choose to continue processing the case.

### Temporary Cases

**Temporary cases are only stored in memory, on the clipboard and do not have case ID's. This is useful for saving storage and improving system performance**.

Once a case meets the condition specified by the organization, such as a customer change of address where the address entered is different from the current address, the case can be recorded in the database (persisted) for future reference.

Temporary cases are processed by a single operator and cannot be routed until they are persisted.

___

<a name="parallel"></a>
## Parallel processing (Unit 15)

You use the **Split Join** shape to call multiple processes that operate in parallel then rejoin the main flow. **Join conditions** can be specified to determine when the main process can continue.

A **Split For Each** is used to run a subprocess multiple times by iterating through a set of records in a page list or page group. When the items have been processed, the main flow continues.

**Spinoff** subprocesses allow you to run a subprocess in parallel to the main flow. The main process does not wait for the spinoff to complete befoire proceeding. 

The Join conditions for parallel processing are:

* **Any** - Main flow resumes when any of the conditions are met. Other subprocesses are stopped if they are not complete and open assignments for these subprocesses are cancelled.

* **All** - Main flow only resumes when all subprocesses are complete.

* **Some** - Uses a when rule or a count to determine when the main process can resume.

* **Iterate** - On the **Split For Each** shape, this starts flows for items in the page list or group and tests an optional when condition to determine whether to start a flow for a given iteration.

<a name="lock"></a>
### Case Locking

The default type of case locking in Pega is **Allow one user**. This locks a case when an operator opens it. If a second operator tries to open the same case, they can only do so in **review mode**. Pega locks the case for **30 minutes** or until the **user submits or closes the case**, whichever comes first.

**Allow multiple users** enables multiple operators to update the same case at the same time. A lock only occurs when the user submits the case. Changes are made by whichever operator submits first. When a second operator submits, they must refresh the case to get the new changes made by the first operator. The second operator can then submit their changes. 

Cases are locked on **parent** cases, and cascade down to any child cases created, the default being **allow one user**. This can be overwritten at the **child** case level, when multiple users need to access the child cases.

___

<a name="flow"></a>
## Flow Action Processing (Unit 17)

**In Pega, you can add pre- and post-processing actions to a flow action to manipulate data. These actions enable you to add related tasks to the flow action. For example, you can concatenate a person's first name and last name to create their full name**.

Pre-processing begins when a user selects a flow action as well as when the user is presented with the assignment. So if a user completes an assignment, then returns to is later, pre-processing will happen again. Logic can be added to test whether this should happen.

Pre-processing actions could be running a data transform, initializing a value, or initializing a list.

Consider the possibility of reuse when decising whether to use a pre-processing action. If a pre- or post-processing action applies to only one case type, then specialize the flow action for the case type instead of adding the pre-or post-processing action.

___

<a name="decision"></a>
## Decision Tables and Trees (Unit 19)

Decision tables are used when you need to test the values of multiple properties to make a decision.

Decision tables are laid out like spreadsheets, with rows and columns. 
A decision table is a table of conditions and results. You define a set of conditions – for example, property values that must fall within a certain range – and the results to return when the conditions are true. Each set of conditions contains a corresponding result. You can add columns for each condition you want to test against – either a property reference or an expression – and rows for each combination of conditions you want to test.

When the system evaluates a decision table, it starts with the top row and evaluates each condition in the row. If **all of the conditions are true**, the system returns the result for that row. If not, it advances to the next row, and evaluates the conditions in that row. And if none of the combinations returns a result, the system returns the **otherwise result** at the bottom of the table. This ensures that the r decision always returns a result.

A decision table uses an **equals** condition by default to evaluate a condition. **Greater than** and **less than** can be used when evaluating numerical conditions.

Enabling the **Evaluate all rows** option on a decision table makes that table evaluate all of its rows and return an array of the results.

### Decision Trees

Decisoin trees can also be used to handle logic that calculates a value from a set of test conditions.

While decision tables evaluate against the **same set** of properties or conditions, decision trees evaluate against **different** properties or conditions. While a decision table stops execution when it gets a **true** result, a decision tree can make additional evaluations after getting a **true** result.

A decision starts at the top of a decision tree and proceeds downwards, each **true** result advancing the decision process.

Decision trees contain condition branches — a comparison value, a comparison operator, and an action. The action can be to return a result, to continue the evaluation, or stop the evaluation. The branches are organized in a hierarchical tree structure. Typically, you specify common conditions and results at the trunk of the tree. You then extend the tree outward to more-specific conditions and their actions. When the decision tree is invoked, the system evaluates the top row and continues until it reaches a result that evaluates to true. The result is returned to the system. If the system processes through all the branches but does not reach a returned result, the system returns the final otherwise value.

Decision tree branches can be nested. For example, if a pruchase request is for **> $100**, the request must be routed for approval. Two possible outcomes of approval exist and must now be evaluated. If a purchase request is for **< $100** (the original condition is false), the two approval outcomes **do not** need to be evaluated.

The structure of such a decision tree might look like this:

* Purchase Request > $100 then continue
  * If department == Consulting then return Compliance
  * Otherwise return Work Manager
* Otherwise return No Approval Needed

Clicking **Show conflicts** when creating a decision tree shows any unreachable conditions.

Clicking **Show completeness** adds rows to indicate values that will not be evaluated. These are suggested conditions to add.

___

<a name="approve"></a>
## Case Approval (Unit 21)

Cases can either be **single approval** or **cascading approval**.

The two types of cascading approval are **Reporting structure** and **authority matrix**.

You use the reporting structure model when approvals **always move up** the reporting structure of the submitter or another defined list.

You use the authority matrix model when a set of rules directs the approval chain to individuals both inside and outside the organization of the submitter.

An authority matrix uses a decision table. The decision table identifies each party in the approval process as well as the conditions that determine when each party must provide approval.

The application evaluates the decision table. Each satisfied row or condition adds the results to a (page) list of approvers. Approval assignments route to approvers in the order of list appearance.

___

<a name="organize"></a>
Organizational Records (Unit 23)

**A Pega application uses an organizational structure to direct assignments to the right operator or work queues, determine the access rights of operators, and report on activity in various company departments**.

The Pega organizational structure is a three-level hierarchy. The top level is known as the **organization**, the middle level contains **divisions**, and the lowest level contains organization **units**.

* A heirarchy has only one **Organization**, representing the entire enterprise.

* A **division** represents a category of a business unit such as a regions, brand, or department.

* A **unit** contains operators wh operform work specific to their organization. This could be caseworkers, agents, and customer service representatives. 

A unit can have **child units**. For exmaple, two units (Hardware and Software) may report to a parent unit (Internal) in the IT division.

An operator is associated with a unit, division, and organization. The operator ID record (also an organization rule) stores the organizational structure of the operator. You can update the organization structure, if necessary.

___

<a name="fields"></a>
## Configuring Field Values (Unit 25)

When building an application, you often need to use a list of allowed values for a specific property. If the list of allowed values is large, expected to change frequently, or may be specific for each case type, you can use a **field value**.

**Field values** enable you to manage the list of allowed values separately from the property. Managing the allowed values separately from the property enables you to reuse a single property, and customize the allowed values based on the context of the property.

You can create field values to restrict the values of a property to a list of allowed values.

___

<a name="access"></a>
## Application Accessibility (Unit 29)

**Web Accessibility Initiative - Accessibile Rich Internet Applications (WAI-ARIA). Is a technical standard that defines ways to make web content and applications more accessible to people with disabilities**.

This is done with **accessibility roles**. These are specific attributes applied to UI elements that enable communication between assistive devices and Pega applications. There are roles that direct a screen reader to differentiate between a checkbox and a button for example.

Accessibility roles are provided to individual **access groups** and exist in the **PegaWAI** ruleset.

There are a few functions that are in a standard ruleset in Pega, so don't require the PegaWAI ruleset. These include:

* Setting tooltips to be read by a screen reader.
* High contrast colour scheme.
* Keyboard controls for tabbing through a UI.
* Links with icons that can be described by a screen reader.
* Provide a list of drop down options that can be read by a screen reader.

The **Accessibility Inspector** tool in Pega can be used to identify and correct accessibility issues. There are two main features that aid accessibility design:

* **Disability Preview** allows you to view your application through each variation of colourblindness.

* You can audit your application UI to identify configurations that may negatively impact accessibility. The four categories that can be assessed are:

* **Content** - an icon or tooltip is missing for example.

* **Structural** - The heading heirarchy is out of order for example, confusing screen readers.

* **Interactivity** - Options are not configured to allow a user to press a key to navigate content for example.

* **Compatibility** - Deprecated layouts (tab layout) is used instead of a layout group for example.

___


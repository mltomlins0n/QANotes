# Pega CSA Notes (Certified System Architect)

## Contents

* [Case Life Cycle](#case)
* [Service Level Agreements](#sla)
* [Parallel Processing](#parallel)
* [Routing Work](#routing)
* [Rules](#rules)
  * [Rulesets](#rulesets)
  * [Ruleset Stack](#rulesetStack)
* [Classes](#classes)
* [Inheritance](#inheritance)
  * [Pattern Inheritance](#pattern)
  * [Directed Inheritance](#directed)
* [Data Elements](#data)
  * [Data Types](#dataTypes)
  * [Data Pages](#dataPages)
* [Pega Property Rules](#propertyRules)
* [Validation Rules](#validation)
* [Declare Expressions](#declare)


<a name="case"></a>
## Case Life Cycle

- A **Case Type** is an abstract model of a business transaction.
- A **Case** is a specific instance of the transaction

Cases are organised into **stages**, which contain **processes**, which contain individual **steps**.

Processes and steps are named using **verb + noun** naming convention.

It is best practice to break steps into other processes if you have more than 7 steps to a process.

Alternate stages can be used when a case deviates from the sunny day path. E.g a request is cancelled.

<a name="sla"></a>
## Service level Agreements

**A service level agreement (SLA) establishes a deadline for work completion. This can range from an informal promise to negotiated contracts**.

There are 4 intervals in an SLA:

- Start
- Goal (occurs once)
- Deadline (occurs once)
- Passed Deadline (can repeat indefinitely)

SLA's have an **urgency**. This is a value is between 0 - 100 and denotes the urgency of a task.

The urgency generally increases after a case passes each interval.

Cases sometimes have an intial urgency. This is set before the case has even started, and any urgency at the "Start" interval is added to the initial interval. E.g. A case with an initial urgency of 10 and a start urgency of 15 has a total urgency of 25 when the case starts.

The "Goal" interval defines the amount of time in which the case or step should be completed.

<a name="parallel"></a>
## Parallel Processing

If processes can be performed in any order, they can be configured as parallel.

2 or more processes can be parallel.

This allows cases to advance through multiple paths at the same time within a stage.

<a name="routing"></a>
## Routing Work

When modelling a process, you define who should do the work on each task, or assignment.

There are 2 routing types:

- Work queue
- Work List

A **work queue** is a list of all open assignments for a **group of users**. Assignments stay in the work queue until a user associated with the work queue selects an assignment, or a manager sends an assignment to a specific user.

A **work list** is a list of all open assignments for a **specific user**.

Routing options include:

- Current user
- Specific user
- Work queue

You route an assignment to the **current user** if they should perform the task, e.g. the employee creating the request enters the expense details.

You route an assignment to the work list of a **specific user** if only that user needs to complete an assignment, e.g. if the manager is the only one to apporve expense reports.

You route to a **work queue** for a specific group when anyone in the group can complete the assignment. E.g. anyone in payroll could send payments to employees.

For more complex routing you can use **business logic** to route assignments. This is based on a **when** condition to route work based on certain conditions. A when rule can have multiple conditions.

<a name="rules"></a>
## Rules

Rules describe the behaviour of individual cases.

Each rule is an instance of a **rule type**. A rule type is an abstract model of a specific case behavior. Pega provides many rule types. For example, Pega provides one type of rule to describe a process flow, and another type of rule to describe an automated email notification.

Using individual rules makes your application modular, with rules acting much like classes in an OOP application.

This approach provides three significant benefits:

- Versioning, rules can be updated whenever case behaviour needs to change. Pega maintains a history of these changes much like GitHub.

- Delegation, System architects delegate rules to business users to allow business users to update case behavior as business conditions change. The business user updates the delegated rule, while other parts of the application remain unchanged.

- Reuse, rules should be reused whenever an application needs to incorporate existing case behaviour.  For example, you create a UI form to collect policyholder information for auto insurance claims. You can then reuse this UI form for property insurance claims and marine insurance claims.

<a name="rulesets"></a>
### Rulesets

To package rules for distribution as part of an application, you collect rules into a group called a ruleset. Rulesets can also be reused in different applications.

An instance of a ruleset is a **ruleset version**. Updating the contents of a ruleset creates a new version of that ruleset. 

You identify each ruleset by its name and version number. For example, an application to process expense reports includes a ruleset named Expense. You refer to the ruleset as Expense:01-02-03, where Expense is the name of the ruleset and 01-02-03 is the version number.

Ruleset numbers start at 01-01-01 and can increease to 99-99-99.

The version number is divided into three segments: a **major version**, a **minor version**, and a **patch version**.

- The major version represents a substantial release of an application. A major version change encompasses extensive changes to application functionality. For example, the Accounting department uses an application to manage expense reports. If Accounting wants to extend the application to track employee time off for payroll accounting, you create a new major version of the ruleset.

- The minor version represents an interim release or enhancements to a major release. For example, you need to update an expense reporting application to make automatic audit travel reimbursements. You create a new minor version of the ruleset.

- The patch version consists of fixes to address bugs in an application. For example, you notice that a field in the current version of an application has an incorrect label. You create a new minor version to correct the field label.

<a name="rulesetStack"></a>
### Ruleset Stack

Each application consists of a sequence of rulesets, called a **ruleset stack**. This determines the order in which Pega looks through rulesets to find the rule begin used. 

Each entry in the ruleset stack represents all the versions of the specified ruleset, starting with the listed version and working down to the lowest minor and patch version for the specified major version.

Each version of an application contains a unique ruleset stack. This allows an updated application to reference new ruleset versions that contain updates and new features.

<a name="classes"></a>
## Classes

In a Pega application, rules are grouped by their capacity for reuse. Each group is a **class**. 

Each application consists of 3 types of class.

- **Work class**, contains the rules that describe how to process a case or cases, such as processes, data elements, and user interfaces.

- **Integration class**, contains the rules that describe how the application interacts with other systems, such as a customer database or a third-party web server.

- **Data class**, contains the rules that describe the data objects used in the application, such as a customer or collection of order items.

Classes which contain other classes are **parent** classes. The contained class is the **child** class. A child class can inherit rules from its parent class, to reuse them.

Pega's "Work" class contains child classes for different case types, such as an auto insurance claim. Each child class contains all of the rules unique to that case type.

The class heirarchy determines how system architects can reuse rules in the application, and consists of several groups of classes:

- Classes that describe a specific case type.

- Classes that collect common rules and data elements, such as an pproval process shared across the whole IT department.

- Classes from other applications.

- Base classes provided by Pega. These classes contains rules that provide basic functionality for processing cases.

Any rule available to an application through the class hierarchy is considered in scope. Rules that an application cannot access through the class hierarchy are considered out of scope.

<a name="inheritance"></a>
## Inheritance

Inheritance allows your application to reuse existing rules for other cases or applications.

There are 2 methods of inheriting rules in Pega, **Pattern Inheritance** and **Directed Inheritance**.

<a name="pattern"></a>
### Pattern Inheritance

Pattern inheritance is automatic. It uses the class name structure to determine rules available to reuse. Pega Platform uses a multi-level class hierarchy of Organization (Org), Division (Div), Unit and Class group to organize application assets.

The **organization** layer contains all the classes for applications across an entire business or other organization. The organization layer often contains data and integration classes that can be applied across the entire organization.

The **division** layer contains the work, data, and integration classes for the division.

The optional **unit** layer contains the work, data, and integration classes for the unit.

The **class** group contains all the case types in an application.

<a name="directed"></a>
### Directed Inheritance

In directed inheritance, the parent class is explicitly specified. You apply directed inheritance to reuse standard Pega Platform rules and rules from other applications outside the business class hierarchy.

**Directed inheritance is the only option that allows an application class to inherit rules defined for standard Pega classes, such as the Work- or Data- class**.

<a name="reusing"></a>
### Resusing Rules Through Inheritance

When attempting to reuse rules through inheritance, Pega first searches through the parent classes indicated by pattern inheritance. If unsuccessful, Pega then searches the parent class indicated by directed inheritance as the basis for another pattern inheritance search. This process repeats until Pega reaches the last class in the class hierarchy, called the ultimate base class or @baseclass. If the rule cannot be found after searching @baseclass, Pega returns an error.

<a name="data"></a>
## Data Elements

Data elements are called **properties** or **fields**. These are the same thing. What can differ is the **mode** of the property. The 2 types are **value** mode and **page** mode.

- **Value** mode describe a single piece of information, such as a total.

- **Page** mode describes a data object such as a customer.

Value mode should be used for properties with no correlation to other properties. The 3 types of value mode properties are:

- **Single value** - stores a single piece of data.

- **Value list** - acts as an ordered list of single values.

- **Value gruop** - acts as an unordered list of single values.

**These 3 types are the same for page mode properties**.

<a name="dataTypes"></a>
### Data Types

Data can be sourced from different places:

- **Locally** - A Pega system of record can locally source data types.

- **Externally** - External systems can source data types.

- **None** - Data types can obtain data entered or transformed during runtime and not asscociate these with any system of record.

Data types can reference other data types. For example, a *Customer* data type could have an *address* property that is a field group defined in an *Address* data type.

The general rules for data type usage are:

- Use standard Pega data types when possible.

- Extend an existing data type if it only partly meets your needs.

- If no suitable data type exists, create a new one.

<a name="dataPages"></a>
### Data Pages

Data pages store data for an application regardless of source. Data pages cache data on demand to a clipboard page and define who can access the page.

The **Object Type** of a data page specifies the information the data page will capture. It acts like the **class** of the data page.

There are 3 scopes for data pages:

- **Thread** - Thread level scope is used when the data page is related to a particular case.

- **Requestor** - Requestor level scope lets you share data pages for a given user session and is often used when the data page contains data associated with the logged in operator.

- **Node** - Node level scope is used to make a data page instance accessible by all users of the application, and other applications running on a given node.

<a name="propertyRules"></a>
## Pega Property Rules

Pega comes with a set of standard property rules. The standard properties have names that start with px, py, or pz. You cannot create new properties starting with px, py, or pz.

- **px** - identifies special properties, your application can read but not write to these properties.

- **py** - you can use these in your application

- **pz** - Supports internal system processing, these values may change with new product releases. You can read but not write to these properties.

<a name="validation"></a>
## Validation Rules

You use validation rules when you cannot predict or control the value a user enters in a form. There are two types of validation rules: **validate** and **edit validate**.

You use validate rules to compare a property against a **condition** when the user submits a form. If the user enters a value that fails to meet the condition, the form displays an error when the form is submitted. 

For example, assume your view contains a field for date of birth. The property type and control cannot prevent users from entering a date that is in the future. However, you can design a validate rule to display an error if the user submits a date that is in the future.

You use edit validate rules with single value, value list, and value group properties to test for **patterns**. 

For example, you can configure a zip code property to reference an edit validate rule that tests whether the entered value has five digits. 

In another example, an email address can reference an edit rule to test whether the entered value contains an "@" symbol. If the submitted value is invalid, the field displays an error. 

Edit validate rules run when the user exits a field if the harness rule is configured to support client-side validation. Otherwise, edit validate rules are run when the user submits a form.

<a name="declare"></a>
## Declare Expressions

Declare expressions compute a value based on an expression and are automatically triggered based on backward or forward chaining. They consist of a **target** property, **source** properties and an **expression**.

- Declarative network - internal data structure that defines the relationship between properties whose value is automatically calculated based on changes to other property values. A declare expression in a network can use the target property of another declare expression as its source property.

- Declarative processing rules - allow configuration of app so that it **automatically updates property values** such as total order amount. e.g. when laptops are ordered by a user, the system multiplies the price of one laptop by the quantity to calculate total order amount. The updates only occur when triggered in the application defined using the trigger event. It monitors changes to the value and runs a computation to apply changes when it does. Used when rules need to be executed all the time, when the input data is changed or the result of the rule is used.

- Procedural processing rules - depends upon rules, such as data transforms, activities to instruct the application when to look for a trigger event. **Only changes data when the user submits a form**. To make the changes visible to the users as they enter values, you must configure sections to use the data transform to refresh the fields. Use at well defined intervals or have a control to fire off the expression
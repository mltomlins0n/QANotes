# Pega Notes

## Contents

- [Case Life Cycle](#CaseLifeCycle)
- [Service Level Agreements](#SLA)
- [Parallel Processing](#ParallelProcessing)
- [Routing Work](#RoutingWork)
- [Rules](#Rules)
  * [Rulesets](#Rulesets)
  * [Ruleset Stack](#RulesetStack)



## Case Life Cycle

- A **Case Type** is an abstract model of a business transaction.
- A **Case** is a specific instance of the transaction

Cases are organised into **stages**, which contain **processes**, which contain individual **steps**.

Processes and steps are named using **verb + noun** naming convention.

It is best practice to break steps into other processes if you have more than 7 steps to a process.

Alternate stages can be used when a case deviates from the sunny day path. E.g a request is cancelled.

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

## Parallel Processing

If processes can be performed in any order, they can be configured as parallel.

2 or more processes can be parallel.

This allows cases to advance through multiple paths at the same time within a stage.

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

## Rules

Rules describe the behaviour of individual cases.

Each rule is an instance of a **rule type**. A rule type is an abstract model of a specific case behavior. Pega provides many rule types. For example, Pega provides one type of rule to describe a process flow, and another type of rule to describe an automated email notification.

Using individual rules makes your application modular, with rules acting much like classes in an OOP application.

This approach provides three significant benefits:

- Versioning, rules can be updated whenever case behaviour needs to change. Pega maintains a history of these changes much like GitHub.

- Delegation, System architects delegate rules to business users to allow business users to update case behavior as business conditions change. The business user updates the delegated rule, while other parts of the application remain unchanged.

- Reuse, rules should be reused whenever an application needs to incorporate existing case behaviour.  For example, you create a UI form to collect policyholder information for auto insurance claims. You can then reuse this UI form for property insurance claims and marine insurance claims.

### Rulesets

To package rules for distribution as part of an application, you collect rules into a group called a ruleset. Rulesets can also be reused in different applications.

An instance of a ruleset is a **ruleset version**. Updating the contents of a ruleset creates a new version of that ruleset. 

You identify each ruleset by its name and version number. For example, an application to process expense reports includes a ruleset named Expense. You refer to the ruleset as Expense:01-02-03, where Expense is the name of the ruleset and 01-02-03 is the version number.

Ruleset numbers start at 01-01-01 and can increease to 99-99-99.

The version number is divided into three segments: a **major version**, a **minor version**, and a **patch version**.

- The major version represents a substantial release of an application. A major version change encompasses extensive changes to application functionality. For example, the Accounting department uses an application to manage expense reports. If Accounting wants to extend the application to track employee time off for payroll accounting, you create a new major version of the ruleset.

- The minor version represents an interim release or enhancements to a major release. For example, you need to update an expense reporting application to make automatic audit travel reimbursements. You create a new minor version of the ruleset.

- The patch version consists of fixes to address bugs in an application. For example, you notice that a field in the current version of an application has an incorrect label. You create a new minor version to correct the field label.

### Ruleset Stack

Each application consists of a sequence of rulesets, called a **ruleset stack**. This determines the order in which Pega looks through rulesets to find the rule begin used. 

Each entry in the ruleset stack represents all the versions of the specified ruleset, starting with the listed version and working down to the lowest minor and patch version for the specified major version.

Each version of an application contains a unique ruleset stack. This allows an updated application to reference new ruleset versions that contain updates and new features.


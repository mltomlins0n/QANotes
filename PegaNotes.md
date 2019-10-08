# Pega Notes

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

## Parallel Processes


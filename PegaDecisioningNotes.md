# Pega Decisioning Notes

## Contents

* [Objectives](#obj)
* [Next-Best-Action (NBA)](#NBA)

<a name="obj"></a>
## Objectives

* Understand the basic components and capabilities of the Pega Customer Decision Hub.

* Understand how Next-Best-Action improves the customer experience.

* Configure Next-Best-Action designer to automate Next-Best-Action strategy design process.

* Explain how predictive and adaptive modeling works and use them in decision strategies.

* Prioritize propositions based on predicted customer behavior.

* Run alternative strategies and compare the results using Visual Business Director.

<a name="NBA"></a>
## Next-Best-Action (NBA)

**The Customer Decision Hub combines analytics, business rules, customer data, and data collected during each customer interaction to create a set of actionable insights that it uses to make intelligent decisions. These decisions are known as the Next-Best-Action.**

Every Next-Best-Action weighs customer needs against business objectives to optimize decisions based on priorities set by the business manager.

A Next-Best-Action is designed as a series of decisions that execute in a hierarchical fashion. 

At the highest level, the Decision Hub selects the best actions related to a business goal, such as Retention, Service or Sales. Decisions defined at the business goal level determine the best action to be taken to achieve that goal.

All of this information is used by the Next-Best-Action decision strategy to evaluate every potential action that could be taken with a particular customer in a given situation. The decision strategy then recommends the best way to interact with the customer to achieve the optimal result.

Using the Next-Best-Action approach, the Customer Decision Hub is able to identify the best moments for making a sale, providing a service, making a retention offer, or doing nothing at all (e.g. if nothing is relevant enough to warrant the customer’s attention). 

Next-Best-Action is even able to select which offers are most likely to be accepted by the customer in a sales or retention situation. Next-Best-Action decisions are distributed, in real-time, to each of your real-time owned channels, such as web, mobile, and contact center.

A set of conditions decides the relevance of an action at every level.

The Customer Decision Hub uses two types of information to deteremine Next_Best-Action (NBA) recommendations:

* Call context.
* Customer Profile.

Two conditions must be met for the Customer Decision Hub to consider an offer for a customer:

* The **Active** attribute is set on the offer.
* The offer belongs to the **first relevant action** in the NBA hierarchy.




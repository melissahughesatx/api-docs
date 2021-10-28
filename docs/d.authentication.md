
# Security and authentication with Monite

Monite supplies the following access modes.

- Partner - a person who implements Monite in their SASS tool is our API partner. Each partner uses Monite APIs in order to provide additional functionality to its customers and the customers employees. The customer of a partner is referred to as an entity, and their employees as end users. Software engineers for the partner call Monite API top generate entity access tokens. These tokens are service tokens, they allow you to customize and configure API settings.

- Entity - an entity is the customer of a partner. Each entity is able to issue entity-level tokens and these tokens are bound to each entity and end user. One entity cannot access to data that belongs to another entity.

- End user - an end user is the person who uses the software products that access Monite. 

The differences between access modes is:

- Partner - reserved for engineers of API partners. It's not possible to trigger any business logic calls from this level.

- Entity - 'root access' for entities. On this level it's possible to access all types of records that are stored in Monite that belong to the customers of this entity. Monite does not check access permissions on this level!

- End user - an optional level for entity employees.  This security level is designed for those API partners who don't want to build access control tools on their side and want to use our solutions. Monite creates customizable roles and permissions on this level and checks access policies when making API calls.

This section shows you how to implement the different security modes into your app.

## How it works

The following diagram shows how the different security layers interact with Monite.

TBD

## What you need

To successfully understand and complete this task you must have the following:

- A valid Monite account.
- A monite key/value pair.
- Good knowledge of [Multi-factor authentication](https://en.wikipedia.org/wiki/Multi-factor_authentication).


## Implement Monite security and authentication 

<!--

The title for the steps does not have to be implemen t.

If there are multiple logical sections in the procedure such as back-end and frond-end, use subsections. The goal is to modularize the various elements and build the project in a logical way that makes it easy for the user to follow along.

Write an intro sentence to explain the subsections:

For example:

-->

Step-up authentication implementation involves changes to components in your app and security infrastructure. This section shows you how to:

- [Implement partner security](link to module1)
- [Implement Entity security](link to module2)
- [Implement end user security](link to module2)

<!--

If there are no locical subsections, write a single procedure here.

-->

### Implement partner security

<!--

A module explains how to implement a set of functions that operate together to produce a specific feature of the project.
Be sure to explain all inputs and outputs (not necessarity a complete API reference), discuss their significance within the code and how they work together.


```javascript 
 code blocks 
 Always remember to put the coding language
```

For example:

-->

TBD

To add partner security to your app:

1. Do this.
1. Do that:
    1. Sometimes you need substeps.
    1. Always at least 2 substeps.
1. Do something else.

### Implement Entity security

<!--

Brief explanation about what the technology does in this section, then a procedure.

For example:

-->

TBD

To add entity authentication to your security infrastructure:

1. Do this.
1. Do that:
    1. Sometimes you need substeps.
    1. Always at least 2 substeps.
1. Do something else.

## Implement Monite security and authentication

Step-up authentication implementation involves changes to components in your app and security infrastructure. This section shows you how to:

- [Implement Client security](link to module1)
- [Implement Entity security](link to module2)
- [Implement Entity-user security](link to module2)


### Implement Client security

TBD

To add Client security to your app:

1. Do this.
1. Do that:
   1. Sometimes you need substeps.
   1. Always at least 2 substeps.
1. Do something else.

### Implement Entity security

TBD

To add entity authentication to your security infrastructure:

1. Do this.
1. Do that:
   1. Sometimes you need substeps.
   1. Always at least 2 substeps.
1. Do something else.

### Implement Entity-user security


TBD

To add Entity-user to your security infrastructure:

1. Do this.
1. Do that:
   1. Sometimes you need substeps.
   1. Always at least 2 substeps.
1. Do something else.


## Test security in your app

To ensure that you have implemented &lt;A rewording of the page title&gt; correctly:

1. Do this.
1. Do that:
   1. Sometimes you need substeps.
   1. Always at least 2 substeps.
1. Do something else.


## Reference

TBD

You use the following to configure or develop Monite security in your app:




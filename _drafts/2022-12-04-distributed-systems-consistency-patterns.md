---
layout: post
title: 'Distributed Systems: Consistency Patterns'
date: '2022-12-04 23:30:44 +0530'
categories: [CONSISTENCY, DISTRIBUETD SYSTEMS]
tags: [distributed-systems, patterns, consistency]
---
## How to achieve Consistency in Distributed Systems ?

Ensuring consistency in distributed transactions can be challenging, because it involves coordinating the changes made by multiple transactions across multiple systems. Here are some strategies that can help ensure consistency in distributed transactions:

Two-phase commit protocol: This protocol involves two phases: a prepare phase and a commit phase. In the prepare phase, each participant in the transaction prepares to commit its changes. If all participants are ready to commit, the transaction can proceed to the commit phase. If any participant is not ready to commit, the transaction is rolled back.

Three-phase commit protocol: This protocol is similar to the two-phase commit, but adds an additional phase called the "pre-commit" phase. In this phase, each participant prepares to commit its changes and sends a message to the coordinator indicating whether it is ready to commit. If all participants are ready to commit, the coordinator sends a commit message to all participants and the transaction is committed. If any participant is not ready to commit, the coordinator sends a rollback message and the transaction is rolled back.

Optimistic concurrency control: This approach relies on the assumption that conflicts between transactions are rare, and allows transactions to proceed without locks. When a transaction is committed, it checks to see if any conflicts have occurred. If a conflict is detected, the transaction is rolled back and the user is prompted to retry the transaction.

Pessimistic concurrency control: This approach assumes that conflicts between transactions are more likely, and uses locks to prevent conflicts. When a transaction wants to update a record, it acquires a lock on the record. This prevents other transactions from updating the same record until the lock is released.

No single approach is best for all situations, and the appropriate strategy for ensuring consistency in distributed transactions will depend on the specific requirements of the application.

## Further Reading

https://kousiknath.medium.com/consistency-guarantees-in-distributed-systems-explained-simply-720caa034116#:~:text=Consistency%20in%20Distributed%20Systems%3A%20Consistency,client%20has%20updated%20the%20data.

https://medium.com/@sanksons/when-dealing-with-distributed-systems-we-generally-classifies-the-system-as-a-strong-consistency-aff9fa1601b0
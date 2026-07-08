**[< Home](README.md)**
# Persistence

## JPA
Java Persistence API
Bundle of specifications that defines how to persist data in a database.
Five standards:
- Persistence Unit
- EntityManagerFactory
- EntityManager
- Contexte de persistance
- Objects Entity

The different life cycle of an object:
- New (transient)
- Managed
- Detached
- Removed

## JDBC
Java Database Connectivity

Permit connecting to a database, all done by hand. Writing sql, managing connections, object mapping etc...

## ACID
- Atomicity
  - Request is either all or none
- Consistency
  - the data is always in the sane state
- IsolationJDBC
  - the transaction is isolated from other transactions
- Durability
  - when the transaction is committed, the data is persisted (permanent even if the system crashes)

## Hibernate
Hibernate is a Java persistence framework.

## Entity manager
It's a component of JPA specifications that is responsible for managing the persistence context.
It gives an interface to effectuate CRUD operations.

Hibernate session is an implementation of the entity manager.

## Cache

- L1
  - Short-term memory 
  - specific to a process
- L2
  - Long-term memory
  - shared among processes

## Projections 
= dto for database

tell the database what field you want to get specifically

When you want only a spefic field from the database without getting all the data

## DDL 
Data Definition Language

## APM
Application Performance Management
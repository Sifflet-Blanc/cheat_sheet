[< Home](README.md)
# Architecture

2 major types :
- inside a service
  - n-tier
  
    3 main layers
    - Web tier
    - Business tier
    - Data tier
  - Hexagonal
    
    Business layer independent of everything (framework, db, etc...)
- infrastructure
  - Monolithic
  
    Adapt to a project from scratch because we don't perfectly know what the client needs. 

    We can always explode it to microservices later. For different reasons :
    - Scalability
    - Reusability
    - Team size (too big for monolithic)
  - Distributed monolithic
  
    The services are coupled with each other
  - Microservices

    Asynchronous communication between services



# Observability

Tools :
- Signoz
- Sentry
- Datadog

on microservices' architecture :
- Log trace (create an uuid to trace the request in all the services)

# ASYNC
Used on microservices' architecture. (not only async also sync)

Known :
- Kafka
  - Big volume of a message
- RabbitMQ
  - Message is suppressed after consumption 
  - Work like a queue

## Kafka

Message in kafka :
- Key
- value
- timestamp

### Topic
Topic can be cut into partitions (one or more).

2 type of topic :
- classic
- compact, I have two times the same key; the last one erases the previous one

### Consumer
- offset (liked to group id)
- group id
- partition (at least one)
- topic

### Producer
- topic
- acknowledge `acks` (0, 1, all) (the confirmation of the message is well received by the leader or all)


**Out of the box**, store the message in a bdd and after a scheduler with send it to kafka. There are two principal advantages : 
- if there is a translation, we can wait for the end of the translation before sending the message
- if kafka is down, the scheduler will retry to send the message periodically
# What are Microservices ?

Microservices has become an increasing popular architecture choice
in the half decade.

This chapter help to look at the core ideas behind microservices,
the prior architecture that brought us here and reasons why this is so widely adopted.

## Microservices at a Glance

1. Microservices are independently releasable services that are modeled
   around a business domain.
2. A service encapsulates functionality and makes it accessible to other services
   via network - you can construct more complex system on this building block
3. Example one microservice represent inventory, another order processing and
   another shipping, but together they form a complete E-commerce system.
4. Microservices are an architecture choice that is focused on giving you many
   options for solving problems you might face.
5. On outside, microservices look like a black box and implements API's which
   can be used by other services or consumers.
6. Internal implementation of microservices is hidden from outside world. The
   language it uses and database it uses is hidden from outside world.
7. **Important** - This means microservice architecture avoid using shared database
   in most circumstances.
8. Microservices embrace the concept of **Information Hiding**. It means that it
   hides as much as information within the service and only exposes little
   information to outside world via external interfaces.
9. This allows you to change the internal implementation of microservice easily without
   affecting the outside world as the as the external interface remains the same.
10. Changes in microservice should affect upstream microservices, enabling
    independent releasability of functionality or s service.
11. Having clear, stable service boundaries that don't change when the internal implementation
    changes results in a system that have looser coupling and strong cohesion/

## Key Concepts of Microservices

Core ideas that must be understood when you explore microservices are

### Independent Deployability

1. Independent Deployability is the idea that when you make a change to a
   microservice, you can deploy that change without having to deploy any other
   microservices.
2. On surface, this seems like a simple idea, but it has a lot of implications.
3. Getting into a habit of deploying microservices independently will inherit
   bring a lot of discipline to your development process.
4. To ensure independent deployability, you need to ensure that the microservices
   are loosely coupled and have well defined interfaces.
5. Some implementation choice make this hard to achieve, such as using a
   shared database or tightly coupling the microservices.
6. To achieve independent deployability, there are so many things to get right
   that in turn brings own benefit
7. The desire for loosely coupled services with stable interfaces guides our
   thinking about how we find our microservice boundaries in the first place.

### Modeled Around Business Domain

1. 
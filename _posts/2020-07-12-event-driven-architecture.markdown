---
layout: single 
title:  "Event-Driven Architecture, a brief introduction"
date:   2020-07-12 16:30:44 -0500
categories: jekyll update
---
6 min read

<img alt="Image for post" class="s t u fo ai" src="https://miro.medium.com/max/680/1*Gj9SfHMqprQ3vKK9thHWrw.png" width="618" height="227" srcSet="https://miro.medium.com/max/680/1*Gj9SfHMqprQ3vKK9thHWrw.png 552w,https://miro.medium.com/max/680/1*Gj9SfHMqprQ3vKK9thHWrw.png 618w" sizes="618px"/>

This post gives a brief introduction to the event-oriented software architecture and its purpose presenting three key concepts to consider: Contracts, Message Brokers and state machines; the examples are given talking about microservices and thinking about the implementation of the architecture for Backend applications.

Let’s start with a formal definition:

> **“An event-oriented architecture is a design pattern that allows a set of reactive systems to communicate with each other through the publication and consumption of events, which can be interpreted as state changes of objects.”**

Now let’s see how contracts, message brokers and state machines work with this definition.

* * *

**Service Contracts**
=====================

> …that allows a set of reactive systems to communicate with each other …

**A contract** is a definition or document that allows another entity, be it another microservice, scheduled work or client (app) to know the information to send to communicate with it and what it will receive in return, as its name indicates the contract is the agreement between the service and its subscribers.

<img alt="Image for post" class="pa ul s t u iz ai ji" width="711" height="441" src="https://miro.medium.com/max/782/0*sOOGw5IvSzsvSL4a.png" srcset="https://miro.medium.com/max/304/0*sOOGw5IvSzsvSL4a.png 276w, https://miro.medium.com/max/607/0*sOOGw5IvSzsvSL4a.png 552w, https://miro.medium.com/max/704/0*sOOGw5IvSzsvSL4a.png 640w, https://miro.medium.com/max/770/0*sOOGw5IvSzsvSL4a.png 700w" sizes="700px">

The systems can be of different sizes and categories according to their functions, their domains, etc., it is important to define a category with which to divide them and define a clear scope for each one, a system can go from a class with a specific task, sharing a base code with other small systems, up to a large system which inside can be composed of several services that works together to perform their function or business functionality.

<img alt="Image for post" class="pa ul s t u iz ai ji" width="751" height="381" src="https://miro.medium.com/max/826/0*5XhHgykgeMCjyXBe.png" srcset="https://miro.medium.com/max/304/0*5XhHgykgeMCjyXBe.png 276w, https://miro.medium.com/max/607/0*5XhHgykgeMCjyXBe.png 552w, https://miro.medium.com/max/704/0*5XhHgykgeMCjyXBe.png 640w, https://miro.medium.com/max/770/0*5XhHgykgeMCjyXBe.png 700w" sizes="700px">

For our use case, we will go into detail talking about microservices, therefore our definition and subsequent examples will talk about a group of microservices which communicate with each other, each microservice fulfills a particular task and must have an entry and a clear exit, this information is known as the **contract**.

* * *

Message Broker and the Observer Pattern
=======================================

> … through the publication and consumption of events …

With microservices and contracts clear, we have the need to communicate our microservices between them. It’s common that microservices not always have to be exposed to a final user or client application, it is very common for systems to communicate together to keep data integrity or to execute processes in a specific order that overflow the functionality of the current service. There are different strategies to communicate systems between them such as:

*   Rest
*   RestFull
*   Web sockets
*   Long polling
*   **Event-Driven**
*   Soap
*   ETC …

We seek to choose the right strategy to solve the business problem, as this post is about event-driven architectures, so we will focus on the types of problems that this architecture can solve, such as updating markers and positions. In real-time of the SOME GAME standings, this problem is ideal for an event-oriented architecture since the score is modified before a remote event launched by a system that manages the game as such, in addition to the location of a player in the table of positions may change in the event of a score modification of X (minimum one) players.

A system designed to react by events will be executed independent of the execution time of the one who triggered the event, in the same way, the system that triggered the event is entitled to continue its execution without waiting for the result of the execution of the system that runs in another plane (not to be confused with background execution of operating systems).

The ability to be reactive is given by the **observer design pattern** which is a pattern of behavior, taking care of the communication of messages from one to many before a state change.

<img alt="Image for post" class="pa ul s t u iz ai ji" width="811" height="371" src="https://miro.medium.com/max/892/0*ebolaBV_rew0nF_J.png" srcset="https://miro.medium.com/max/304/0*ebolaBV_rew0nF_J.png 276w, https://miro.medium.com/max/607/0*ebolaBV_rew0nF_J.png 552w, https://miro.medium.com/max/704/0*ebolaBV_rew0nF_J.png 640w, https://miro.medium.com/max/770/0*ebolaBV_rew0nF_J.png 700w" sizes="700px">

For practical terms, the observer pattern is implemented in the Messages Brokers, which are intermediate systems that are responsible for the translation and broadcasting of messages to their subscribers, these systems can be seen as the means of communication between the subsystems giving the ability to publish events to the entire system network before any change of state without affecting its natural flow and also the ability to subscribe in the events that the subscriber really need which would work as triggers.

<img alt="Image for post" class="pa ul s t u iz ai ji" width="811" height="521" src="https://miro.medium.com/max/892/0*eBxIarvKABxnaa6m.png" srcset="https://miro.medium.com/max/304/0*eBxIarvKABxnaa6m.png 276w, https://miro.medium.com/max/607/0*eBxIarvKABxnaa6m.png 552w, https://miro.medium.com/max/704/0*eBxIarvKABxnaa6m.png 640w, https://miro.medium.com/max/770/0*eBxIarvKABxnaa6m.png 700w" sizes="700px">

* * *

Finite state machines
=====================

> … Which can be interpreted as state changes of objects. …

In the previous sections, it is said that an event is triggered by the state change of an object, objects have attributes and abilities, among their attributes depending on the object you could have a list of states among which you will always have a current state (Current) and the ability to change to another listed state, the state can have one or more following states (Next) depending on the business logic and parameters received and a previous state (Prev), events are published in the Message Broker at the moment a state change occurs from Current to any other possible state of the object, this attribute is known as a finite state machine.

<img alt="Image for post" class="pa ul s t u iz ai ji" width="489" height="433" src="https://miro.medium.com/max/538/0*lUoauS6vW24QXpJD.png" srcset="https://miro.medium.com/max/304/0*lUoauS6vW24QXpJD.png 276w, https://miro.medium.com/max/538/0*lUoauS6vW24QXpJD.png 489w" sizes="489px">

A finite state machine has a predetermined number of possible states, always has an initial state and the following state may vary depending on the parameters received by the machine.

Next Steps
==========

*   If you are here because you are thinking about a specific problem you must first decide if an event-oriented architecture is the best solution to the business problem you have in mind.
*   On the other hand, if you are out of technical curiosity, find a small problem with which you can put this architecture in practice.
*   Choose an infrastructure on which to develop the project, it can be in the cloud, taking advantage of the AWS, Azure, Google Cloud, Digital Ocean, Etc trials; or instead configure local environments to emulate several microservices and configure a message broker for communication, the important thing is to document yourself in the best possible way and to start working with known infrastructures.
*   And as final step Iterate, suffer, learn, correct and repeat :)

* * *

Conclusions
===========

Event-driven systems have many use cases and can enter as a complement to an existing system, a system does not have to be designed from the beginning as an event-driven system, a parallel solution can be designed to remove additional processing or develop new functionalities without affecting the core of the existing system.

This architecture is very useful in the process of decoupling monolithic systems and in the creation of systems based on microservices.  
It is very important to work neatly in the design of the architecture to be lifted before starting the implementation since it allows the concrete definition of the participants of the system, their communications, their possible states, and subscriptions as well as preventing falling into the anti-pattern **Large ball of mud** since as this architecture can be observed, the Observer design pattern has as its central axis and during its development, there are other design patterns and software design definitions which ensures stability and structure when developed correctly.  
Thanks to the infrastructure in the cloud, some providers have structured solutions to manage infrastructures of different sizes as Amazon does it with its SNS and Kinesis products, in addition to the fact that there are already several easy-access solutions on the market from which we can get profit like RabbitMQ and Apache Kafka among others.

_I hope this brief introduction touches your curiosity to go deep into this architecture design._

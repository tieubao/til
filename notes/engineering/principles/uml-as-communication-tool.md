---
title: "UML as a communication tool"
date: 2019-04-05
captured: "2019-04-05T07:28:11Z"
tags: [uml, software-engineering, modeling, communication]
source: "GitHub issue tieubao/til#420"
aliases: []
status: refined
---

## Context

An overview of the Unified Modeling Language (UML) as a communication tool for software development. UML uses diagrams and descriptions to help developers visualize, construct, and document software components. It is a modeling language, not a process.

**Source:** [The Unified Modeling Language](http://science-technology.vn/?p=2146)

## Key UML diagrams

| Diagram | Purpose |
|---------|---------|
| Use Case Diagram | Capture functional requirements and define project scope. The real value is in the text portion, not the diagram itself. |
| Class Diagram | Probably the most widely used UML diagram. Easy to explain to users. |
| Activity Diagram | Model workflow and time sequences. Acts as glue binding many different views of a system into one diagram. |
| Object Diagram | Similar to class diagrams but includes sample values for quality attributes, helpful for concrete examples. |
| Sequence Diagram | Illustrate message passing back and forth between objects. |
| Deployment Diagram | Explain hardware devices in a system and software components installed on each. |

## UML can communicate "what" and "how"

- "What" is required of a system (requirements perspective, for customers)
- "How" a system may be implemented (system perspective, for developers)

UML is often used during the requirements phase to facilitate better understanding of user needs. Both developers and users can find it easy to learn, review, and detect errors early.

## Practical advice

Start by using a few UML diagrams in a small project, then expand to more types in other projects. The best way to learn UML is incrementally.

**Recommended book:** UML Distilled by Martin Fowler (Addison Wesley). An excellent, short, easy-to-read introduction.

## Related

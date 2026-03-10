# System Design structure

## What are we doing?

## Functional requirements

## Non functional requirements
This section should define requirments like availability, latency, consistancy and scalability

## System architecture diagram
This section should define high level system architecture diagram, it should show how user/client interacts with the system.

For exampel:

user -> [System] -> [DataBase]

System:

Request -> Load balancer(s) -> Frontend -> backend/API

## Core data models

What will the user input?

What is required for each API? 

## Design APIs

REST vs RPC, 

Take in consideraation for scaling and expansion

## Critical components

What are the critical components that the system require to work?

Do we need cache?

## Scaling strategies

If there is increase in traffc, how can the system handle it?
If there is increase in scope, how can the system grow?

## Reliability and error/failure handling

In case of bot attack?

In case of system failure? Retries?

## Tradeoffs

What are we giving away for the benifits we have?

What are other possible solutions? what are the pros and cons?

---
layout: single
classes: wide
permalink: /process-aware-information-system
title: "Process Aware Information Systems"
excerpt: "Introduction"
last_modified_at: 2021-01-20
toc: false
sidebar:
  nav: "docs"
---

This post aims at introducing what a Process Aware Information System (PAIS) is and giving an overview of our solution to boost the building of such applications.

So you can expect a little bit of concepts and technologies.

Reference architecture.

## Process Aware Information System (PAIS)

The term PAIS was firstly coined by Dumas and Hofstede in the book *Process-Aware Information Systems* (2005) and can be summarized as a software system that manages and executes operational processes involving people, applications, and/or information sources on the basis of process models.
In other words, a PAIS typically focuses on processes instead of focusing on data. 

## Traditional x Process-Aware Information System


Let's make things clear here.
We are not saying that traditional applications are not oriented by processes. 
As discussed by Dumas and Hofstede almost two decades ago, the development of applications are less data centred and more processes centred. 
In fact, nowadays it is quite common to start the development of an application by understanding and designing the business processes behind the solution.
In all these years developing software, I witnessed many applications that were designed from scratch and their code was written by developers who took, among other things, process models as inputs to define the behaviour of the applications.
So, if process models guide the coding phase, what's the problem? We already have process-aware applications, right? 
I can't say so. 
The problem here is that process models vanish at the coding phase and there are some limitations with this approach:



### Process models are weak (optional, incosistent, and outdated)

The root of the problem here comes from the fact that in traditional applications process models are used only as requirement artifacts. 
They are not artifacts required for the execution of the application. 
In such a scenario, process models tend to be weak (optional, inconsistent, outdated). 
Optional: it is common to skip the mapping and representation of some process models, after all they will only be used as reference in the coding phase. I don't have to say that skipping the mapping of a process is not good right?
Inconsistent: since process models are not used by a computational engine and they are consumed only by humans (analysts to understand the requirements, developers to write the code, and testers to validate and verify the solution), process engineers don't need to follow all the rules imposed by the chosen notation and process models are usually incosistent. 
Outdated: during software development and maintenance, changes are inevitable (and welcome according to agilists like me) and a team has to be very disciplined to keep their process models updated. 
The trend is that these models become outdated after a while.
In an agile development environment, where *"running code is more important than documentation"*, the problem is worse.

### It is difficult to explore the benefits of process improvement techniques.
Since we don't have our processes executions explicitly tracked, we are losing a huge opportunity to use techniques for process improvement such as
SLA failure prevention, bottleneck detection, process optimization, and integration with a recommendation system.




There are many solutions for developing PAIS. One solution

So, we refer as traditional applications those that are not explicitly integrated with a process engine. Where Process-Aware are applications whosein the context of this text are applications that are not explici


No Code

Low Code 

Hybrid Solution


## JHipster AgileKip

Our solution is composed of two components: a reference architecture and a jhipster blueprint generator.

### Reference Architecture (RA)

The RA that we are proposing integrates open-sources technologies to provide a computational platform for building and running PAIS. 
The main technologies that composes the RA is shown in Figure below.


The RA is divided in three layers: front-end, back-end, and database.

The front-end layer is single-page application written in Vue.

The back-end layer is a little bit funnier and integrates two amazing platforms for developing enterprises application: 
**Spring Framework** and **Camunda**.

The database layer is composed of a traditional Relational Database (Oracle, Postgres, MySQL, etc).
Since we are using JPA as a solution for Object Relational Mapping (ORM), we are not dependent on any specific vendor.













Although process is everywhere. 

In a Process Aware Information System, the process is a first class citizen element in our architecture. We are making the process explicit in the solution.


For example, in a typical 


Cheers and follow us on social networks!

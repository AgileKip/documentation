---
layout: single
classes: wide
permalink: /part01
title: "Travel Plan Process - Part 1 (A very simple process)"
excerpt: "Part01"
last_modified_at: 2021-10-25
toc: false
sidebar:
  nav: "docs"
---

In the remainder of this tutorial, we will focus on the development of a process named **Travel Plan Process**. 
In a nutshell, this process encompasses the steps users execute when they are planning a travel. 
So, our goal is to develop a process aware application that aids a user planning a travel.

In the part 1 of this tutorial, we start with a very simple version of this process composed of only three user tasks as shown in the model below:


![Screen Shot 2021-10-26 at 03 44 08](https://user-images.githubusercontent.com/4369840/138831528-92b68f4a-a578-40f9-8234-6be1d4fd9db6.png)

## Steps to develop a process

To develop this process, we should execute the following steps:

1. Defining the business process model (BPMN)
1. Defining the domain model (UML)
1. Generating the domain entity
1. Generating the process bind entity
1. Generating the start process entity
1. Generating the user task entities
1. Deploying the process
1. Executing the process

### 1. Defining the business process model (BPMN)

You should use the [Camunda Modeler](https://camunda.com/download/modeler/) to create the BPMN model.

Important notes about creating a BPMN model:

* The model should be a BPMN (CMMN is not supported yet).
* After add a task into the model, you should define its type (usually *User Task*, *Service Task* or *Message Task*). The modeler does not put a default type) so you should mark all human tasks as *User Task*.
* You should specify a process id (see Figure below). The process id is a very important element. It will be associated with the process definition key and cannot be changed after you generate the start code for the process. We suggest using a meaninful name here.
* The process should be marked as **executable**
* The process should be marked as **startable**. The only exception here is for subprocesses (we will talk about subprocesses in another part of this tutorial)
* Optionally, you can specify a documentation for the process using markdown notation. This documentation will be shown in the application.


![part1-bpmn-camunda-modeler-1](https://user-images.githubusercontent.com/4369840/138925859-68a83823-be05-41e8-bfc0-40f902316648.png)

![part1-bpmn-camunda-modeler-2](https://user-images.githubusercontent.com/4369840/138925863-224e2203-0e04-4888-b6ed-e4b3c4990938.png)


Save the file and keep it for later.


### 2. Defining the domain model

The goal of this step is to define the domain model related to the process. You can use any UML modeler or drawing application. We are using here [JDL Studio](https://www.jhipster.tech/jdl-studio/).

To keep things simple in this part of the tutorial, the domain model is represented by a single entity named **TravelPlan** that has a few properties as shown in the model below.

![part1-domain-model](https://user-images.githubusercontent.com/4369840/139011411-5550fc46-9d6c-4155-8754-cb2ec2f56ddf.png)

### Entities

The next four steps are *entities oriented*. You should generated: *domain entities*, *process-binding entity*, *start-form entity*, and *user task entities*. Each entity type plays a different role in the application:

* **domain entity**: this entity represents the data that are handled by the process. 
* **process-binding entity**: this entity represents the binding between a process instance and a domain entity. In other words, the aim of this entity is to indicate the domain entity associated to the process instance.
* **start-form entity**: this entity is used to generate the start form for start the process.
* **user-task entity**: this entity is used to generate the form for a specific user task present in the process.



### 3. Generating the domain entity

The domain entity 





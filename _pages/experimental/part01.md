---
layout: single
classes: wide
permalink: /experimental/part01
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
1. Deploying the process
1. Starting an instance of the process
1. Generating the user task entities
1. Executing the user tasks from a process instance


**Travel Plan project on GitHub**: The source-code of the **Travel Plan** system is on the following GitHub repository:
[https://github.com/AgileKip/travel-plan-tutorial](https://github.com/AgileKip/travel-plan-tutorial). You can check it out to compare with the code you are generating.
{: .notice--warning}


### 1. Defining the business process model (BPMN)

You should use the [Camunda Modeler](https://camunda.com/download/modeler/) to create the BPMN model.

Important notes about creating a BPMN model:

* The model should be a BPMN (CMMN is not supported yet).
* After add a task into the model, you should define its type (usually *User Task*, *Service Task* or *Message Task*). The modeler does not put a default type) so you should mark all human tasks as *User Task*.
* You should specify a process id (see Figure below). The process id is a very important element. It will be associated with the process definition key and cannot be changed after you generate any code for the process. We recommend using a suggestive name here.
* The process should be marked as **executable**
* The process should be marked as **startable**. The only exception here is for subprocesses (we will talk about subprocesses in another part of this tutorial)
* Optionally, you can specify a documentation for the process using markdown notation. This documentation will be shown in the application.


![part1-bpmn-camunda-modeler-1](https://user-images.githubusercontent.com/4369840/138925859-68a83823-be05-41e8-bfc0-40f902316648.png)

![part1-bpmn-camunda-modeler-2](https://user-images.githubusercontent.com/4369840/138925863-224e2203-0e04-4888-b6ed-e4b3c4990938.png)


Save the file and keep it for later.


### 2. Defining the domain model

The goal of this step is to define the domain model related to the process. You can use any UML modeler or drawing application. We are using here [JDL Studio](https://www.jhipster.tech/jdl-studio/).

To keep things simple in this part of the tutorial, the domain model is represented by a single entity named **TravelPlan** that has a few properties as shown in the model below.

![part1-domain-model](https://user-images.githubusercontent.com/4369840/139113292-40b92be8-2a76-42c3-85ca-da062b7c7c41.png)

### Entities

The next four steps are *entities oriented*. You should generated: *domain entities*, *process-binding entity*, *start-form entity*, and *user task entities*. Each entity type plays a different role in the application:

* **domain entity**: this entity represents the data that are handled by the process. 
* **process-binding entity**: this entity represents the binding between a process instance and a domain entity. In other words, the aim of this entity is to indicate the domain entity associated to the process instance.
* **start-form entity**: this entity is used to generate the start form for start the process.
* **user-task entity**: this entity is used to generate the form for a specific user task present in the process.


### 3. Generating the domain entity (TravelPlan)

To generate the **TravelPlan** entity, create the metadata file `.jhipster/TravelPlan.json`:

```
{
    "fields": [
      {
        "fieldName": "name",
        "fieldType": "String"
      },
      {
        "fieldName": "startDate",
        "fieldType": "LocalDate"
      },
      {
        "fieldName": "endDate",
        "fieldType": "LocalDate"
      },
      {
        "fieldName": "airlineCompanyName",
        "fieldType": "String"
      },
      {
        "fieldName": "airlineTicketNumber",
        "fieldType": "String"
      },
      {
        "fieldName": "hotelName",
        "fieldType": "String"
      },
      {
        "fieldName": "hotelBookingNumber",
        "fieldType": "String"
      },
      
      {
        "fieldName": "carCompanyName",
        "fieldType": "String"
      },
      {
        "fieldName": "carBookingNumber",
        "fieldType": "String"
      }
    ],
    "relationships": [],
    "entityType": "domain",
    "service": "serviceClass",
    "dto": "mapstruct",
    "jpaMetamodelFiltering": false,
    "readOnly": true,
    "pagination": "no",
    "name": "TravelPlan",
    "changelogDate": "20210401000001",
    "skipFakeData": true
  }
```

Important notes about metadata for domain entities:

* `fields` describes the primitive types of the entity
* `relationships` describes the relationships with other domain entities (not with process entities)
* `entityType` should be `domain`
* `service` should be `serviceClass`
* `dto` should be `mapstruct`
* `jpaMetamodelFiltering` should be `false`
* `readOnly` should be `true`
* `pagination` should be `no`
* `skipFakeData` should be `true`

After creating the metadata, execute the following command on the docker container:

    jhipster entity TravelPlan --regenerate

The generator will create new files and modify existing files. For each existing file, the generator will ask for confirmation. You should say `y` (yes) to all confirmation questions.
You will see something like that:

```
jhipster@88a5923070f2:~/app/travelPlan$ jhipster entity TravelPlan --regenerate
INFO! Using JHipster version installed globally
...

Found the .jhipster/TravelPlan.json configuration file, entity can be automatically generated!

[entity] calling original askForFields function for the domain entity TravelPlan
Calling original configureEntityTable function...
[entity] Calling original addToYoRc function for the domain entity TravelPlan
     info Creating changelog for entities TravelPlan
     info Using blueprint generator-jhipster-agilekip for entity-server subgenerator
     info Using blueprint generator-jhipster-agilekip for entity-client subgenerator
     info Using blueprint generator-jhipster-agilekip for entity-i18n subgenerator
[entity-server] Calling original writeServerFiles function for the domain entity TravelPlan
[entity-client] Calling original writeClientFiles function for the domain entity TravelPlan
[entity-server] Calling original customizeFiles function for the domain entity TravelPlan
[entity-client] Calling original customizeFiles function for a domain entity TravelPlan...
   create src/main/resources/config/liquibase/changelog/20210401000001_added_entity_TravelPlan.xml
 conflict src/main/resources/config/liquibase/master.xml
? Overwrite src/main/resources/config/liquibase/master.xml? overwrite
    force src/main/resources/config/liquibase/master.xml
    force .yo-rc.json
    force .jhipster/TravelPlan.json
   create src/main/java/com/mycompany/myapp/domain/TravelPlan.java
   create src/main/java/com/mycompany/myapp/web/rest/TravelPlanResource.java
   create src/main/java/com/mycompany/myapp/repository/TravelPlanRepository.java
   create src/main/java/com/mycompany/myapp/service/TravelPlanService.java
   create src/main/java/com/mycompany/myapp/service/dto/TravelPlanDTO.java
   create src/main/java/com/mycompany/myapp/service/mapper/EntityMapper.java
   create src/main/java/com/mycompany/myapp/service/mapper/TravelPlanMapper.java
   create src/test/java/com/mycompany/myapp/web/rest/TravelPlanResourceIT.java
   create src/test/java/com/mycompany/myapp/domain/TravelPlanTest.java
   create src/test/java/com/mycompany/myapp/service/dto/TravelPlanDTOTest.java
   create src/test/java/com/mycompany/myapp/service/mapper/TravelPlanMapperTest.java
   create src/main/webapp/app/shared/model/travel-plan.model.ts
   create src/main/webapp/app/entities/travel-plan/travel-plan-details.vue
   create src/main/webapp/app/entities/travel-plan/travel-plan-details.component.ts
   create src/main/webapp/app/entities/travel-plan/travel-plan.vue
   create src/main/webapp/app/entities/travel-plan/travel-plan.component.ts
   create src/main/webapp/app/entities/travel-plan/travel-plan.service.ts
   create src/test/javascript/spec/app/entities/travel-plan/travel-plan.component.spec.ts
   create src/test/javascript/spec/app/entities/travel-plan/travel-plan-details.component.spec.ts
   create src/test/javascript/spec/app/entities/travel-plan/travel-plan.service.spec.ts
 conflict src/main/webapp/i18n/en/global.json
? Overwrite src/main/webapp/i18n/en/global.json? overwrite
    force src/main/webapp/i18n/en/global.json
   create src/main/webapp/i18n/en/travelPlan.json
 conflict src/main/java/com/mycompany/myapp/config/CacheConfiguration.java
? Overwrite src/main/java/com/mycompany/myapp/config/CacheConfiguration.java? overwrite
    force src/main/java/com/mycompany/myapp/config/CacheConfiguration.java
 conflict src/main/webapp/app/router/entities.ts
? Overwrite src/main/webapp/app/router/entities.ts? overwrite
    force src/main/webapp/app/router/entities.ts
 conflict src/main/webapp/app/main.ts
? Overwrite src/main/webapp/app/main.ts? overwrite
    force src/main/webapp/app/main.ts
 conflict src/main/webapp/app/core/jhi-navbar/jhi-navbar.vue
? Overwrite src/main/webapp/app/core/jhi-navbar/jhi-navbar.vue? overwrite
    force src/main/webapp/app/core/jhi-navbar/jhi-navbar.vue

No change to package.json was detected. No package manager install will be executed.
Entity TravelPlan generated successfully.
...
webpack 5.28.0 compiled successfully in 36764 ms
Congratulations, JHipster execution is complete!
Sponsored with ❤️  by @oktadev.
```

All the files for the TravelPlan entity were generated. To verify the changes, restart the application and navigate to the menu **Entities -> TravelPlan**.


![part1-app-after-domain-entity-generation](https://user-images.githubusercontent.com/4369840/139123217-fb3cf58c-5804-484c-a0ea-dfc48c2c7a9d.png)

**Note**: You can checkout the tag **part1-domain-entity** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the code generated on this step. `git checkout part1-domain-entity`.
{: .notice--warning}

### 4. Generating the process-bind entity (TravelPlanProcess)

To generate the **TravelPlanProcess** entity, create the metadata file `.jhipster/TravelPlanProcess.json`: 

```
{
  "fields": [
    {
      "fieldName": "name",
      "fieldType": "String"
    },
    {
      "fieldName": "startDate",
      "fieldType": "LocalDate"
    },
    {
      "fieldName": "endDate",
      "fieldType": "LocalDate"
    },
    {
      "fieldName": "airlineCompanyName",
      "fieldType": "String"
    },
    {
      "fieldName": "airlineTicketNumber",
      "fieldType": "String"
    },
    {
      "fieldName": "hotelName",
      "fieldType": "String"
    },
    {
      "fieldName": "hotelBookingNumber",
      "fieldType": "String"
    },
    {
      "fieldName": "carCompanyName",
      "fieldType": "String"
    },
    {
      "fieldName": "carBookingNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [],
  "entityType": "process-binding",
  "processBpmnId": "TravelPlanProcess",
  "domainEntityName": "TravelPlan",
  "service": "serviceClass",
  "dto": "mapstruct",
  "jpaMetamodelFiltering": false,
  "readOnly": false,
  "pagination": "no",
  "name": "TravelPlanProcess",
  "changelogDate": "20210401000002",
  "skipFakeData": true
}
```

Important notes about metadata for process-bind entities:

* `fields` describes the fields from the domain entity that should be rendered in the process details page
* `relationships` describes the relationships from domain entity that should be rendered in the process details page
* `entityType` should be `process-binding`
* `processBpmnId` should match the process id defined in the BPMN file
* `domainEntityName` should match the domain entity previously created
* `service` should be `serviceClass`
* `dto` should be `mapstruct`
* `jpaMetamodelFiltering` should be `false`
* `readOnly` should be `false`
* `pagination` should be `no`
* `skipFakeData` should be `true`

After creating the metadata, execute the following command on the docker container:

    jhipster entity TravelPlanProcess --regenerate

As before, the generator will show a message informing an existing file will be touched and ask for confirmation. You should say `y` (yes) to all confirmation questions.

**Note**: You can checkout the tag **part1-process-binding-entity** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the code generated on this step. `git checkout part1-process-binding-entity`.
{: .notice--warning}


### 5. Generating the start-form entity (TravelPlanStartForm)

To generate the **TravelPlanStartForm** entity, create the metadata file `.jhipster/TravelPlanStartForm.json`: 

```
{
    "fields": [
      {
        "fieldName": "name",
        "fieldType": "String"
      },
      {
        "fieldName": "startDate",
        "fieldType": "LocalDate"
      },
      {
        "fieldName": "endDate",
        "fieldType": "LocalDate"
      }
    ],
    "relationships": [],
    "entityType": "start-form",
    "processBpmnId": "TravelPlanProcess",
    "processEntityName": "TravelPlanProcess",
    "domainEntityName": "TravelPlan",
    "service": "serviceClass",
    "dto": "mapstruct",
    "jpaMetamodelFiltering": false,
    "readOnly": false,
    "pagination": "no",
    "name": "TravelPlanStartForm",
    "changelogDate": "20210401000002",
    "skipFakeData": true
  }
  ```

  Important notes about metadata for start-form entities:

* `fields` describes the fields from the domain entity that should be in the start form
* `relationships` describes the relationships from the domain entity that should be in the start form
* `entityType` should be `start-form`
* `processBpmnId` should match the process id defined in the BPMN file
* `processEntityName` should match the process bind entity previously created
* `domainEntityName` should match the domain entity previously created
* `service` should be `serviceClass`
* `dto` should be `mapstruct`
* `jpaMetamodelFiltering` should be `false`
* `readOnly` should be `false`
* `pagination` should be `no`
* `skipFakeData` should be `true`


After creating the metadata, execute the following command on the docker container:

    jhipster entity TravelPlanStartForm --regenerate

As before, the generator will show a message informing an existing file will be touched and ask for confirmation. You should say `y` (yes) to all confirmation questions.

**Note**: You can checkout the tag **part1-start-form-entity** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the code generated on this step. `git checkout part1-start-form-entity`.
{: .notice--warning}

### 6. Deploying the process

Now you have generated the entities *domain*, *process-bind* and *start-form*, you can finally start the process (do not celebrate too early, you cannot execute the whole process yet). The next step is to deploy the BPMN into the application.

* Restart the application
* Login with *admin* account
* Navigate to the menu **Administration -> Process Definitions**. 

![part1-process-deployment-1](https://user-images.githubusercontent.com/4369840/139150157-633b0abe-267e-4b0e-991a-48782b32ba75.png)

* Click in the **Deploy a process** button.
* Choose the BPMN file you created in the step *1. Defining the business process model (BPMN)*
* Click in the **Deploy** button.

![part1-process-deployment-2](https://user-images.githubusercontent.com/4369840/139150159-162367fe-c646-49cc-9b1c-10f6f042f7b6.png)

Now you can see the **TravelPlanProcess** in the process definition list. To see the deployments of this process, click on the **Deployments** button.

![part1-process-deployment-3](https://user-images.githubusercontent.com/4369840/139150163-0de796a8-d51e-4d8c-8135-3e3d811a450c.png)

On the deployments page, you can see all deployments from a given process. To view details of a deployment, click on **View** button.

![part1-process-deployment-4](https://user-images.githubusercontent.com/4369840/139153007-ba6288e2-c69d-4297-b8e4-55ae65fefbd7.png)

On the deployment view page, you can see details from the deployment including its bpmn model.

![part1-process-deployment-5](https://user-images.githubusercontent.com/4369840/139153012-9bf2c40a-a764-4ec8-95e2-e4661dc1a5ff.png)

**Note**: You can checkout the tag **part1-bpmn-deployed** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the BPMN file `src/main/resources/TravelPlanProcess.bpmn` used in this step. `git checkout part1-bpmn-deployed`.
{: .notice--warning}

### 7. Starting an instance of the process

Once the process is deployed, we can start it by clicking on the **Play** button.

The start form contains the fields you defined in the **TravelPlanStartForm** entity. The process description (above the start form) came from the element `documentation` of the BPMN model.
To start the process click on the **Start** button.

![part1-process-start-1](https://user-images.githubusercontent.com/4369840/139153015-f05898dc-4a1c-48f1-972f-df2561e8d001.png)

After started, the application redirects you to the *Process Definitions* main page and show a notification that a process instance was created. To see the instances of a process, click on the **Instances** button.

![part1-process-start-2](https://user-images.githubusercontent.com/4369840/139153017-f567126a-fb53-41fb-9ffa-f1abed613512.png)

The process instance view shows all the instances from a given process. To see details of a process instance, click on the **View** button.

![part1-process-start-3](https://user-images.githubusercontent.com/4369840/139153019-f07715cb-2700-4209-82f1-e08a196025c2.png)

The process instance details page shows the fields and relationships from the domain entity you define in the **TravelPlanProcess** entity. The details page also shows the tasks and the process model associated with the process instance.

![part1-process-start-4](https://user-images.githubusercontent.com/4369840/139153021-059beec4-7c1d-4169-8037-02af2041d028.png)
![part1-process-start-5](https://user-images.githubusercontent.com/4369840/139153022-8d856e03-0e0d-498d-be14-3c42d81ac2c8.png)

Congratulations, you have just started the *Travel Plan* process. Now let's move on and generate the entities that will allow us to execute the whole process.

### 8. Generating the user task entities

The last entities on our list is the *user-task* entities.

To generate the first user task of the process, **TaskFlight** entity, create the metadata file `.jhipster/TaskFlight.json`: 

```
{
    "fields": [
        {
            "fieldName": "name",
            "fieldType": "String",
            "fieldReadOnly": true
          },
          {
            "fieldName": "startDate",
            "fieldType": "LocalDate",
            "fieldReadOnly": true
          },
          {
            "fieldName": "endDate",
            "fieldType": "LocalDate",
            "fieldReadOnly": true
          },
          {
            "fieldName": "airlineCompanyName",
            "fieldType": "String"
          },
          {
            "fieldName": "airlineTicketNumber",
            "fieldType": "String"
          }
    ],
    "relationships": [],
    "entityType": "user-task-form",
    "processBpmnId": "TravelPlanProcess",
    "processEntityName": "TravelPlanProcess",
    "taskBpmnId": "TaskFlight",
    "domainEntityName": "TravelPlan",
    "service": "serviceClass",
    "dto": "mapstruct",
    "jpaMetamodelFiltering": false,
    "readOnly": false,
    "pagination": "no",
    "skipFakeData": true,
    "name": "TaskFlight",
    "changelogDate": "20210401000004"
  }
  ```


Important notes about metadata for user-task-form entities:

* `fields` describes the fields from the domain entity that should be in the task form
* `relationships` describes the relationships from the domain entity that should be in the task form
* `entityType` should be `user-task-form`
* `processBpmnId` should match the process id defined in the BPMN file
* `taskBpmnId` should match the task id defined in the BPMN file
* `processEntityName` should match the process bind entity previously created
* `domainEntityName` should match the domain entity previously created
* `service` should be `serviceClass`
* `dto` should be `mapstruct`
* `jpaMetamodelFiltering` should be `false`
* `readOnly` should be `false`
* `pagination` should be `no`
* `skipFakeData` should be `true`


After creating the metadata, execute the following command on the docker container:

    jhipster entity TaskFlight --regenerate

**Note**: You can checkout the tag **part1-user-task-flight** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the code generated on this step. `git checkout part1-user-task-flight`.
{: .notice--warning}    

Now it's time to check the first user task you've just created. 

* Restart the application
* Login with *admin* account
* Navigate to the menu **Administration -> Process Instances**. 
* Click on the **View** button on the process instance you created previously
* Click on the **Play** button from the task *Choose flight* shown in the tasks section


The user task form contains the fields you defined in the **TaskFlight** entity. The task description (above the task form) came from the element `documentation` of the task in the BPMN model.
To complete the task click on the **Complete** button.

![part1-process-user-task-flight](https://user-images.githubusercontent.com/4369840/139306249-7fb596b4-0eda-4a69-ab6b-e306216ca3c0.png)


You can also execute this task from the **My Task** menu.

After completing the *Choose Flight* task, you can check on the process instance details page that the corresponding fiedls were updated (*Airline Company Name* and *Airline Ticket Number*) and that the next task in the process (*Book a hotel*) is ready to be executed.

![part1-process-user-task-flight-completed-1](https://user-images.githubusercontent.com/4369840/139307980-476c046d-421f-4470-9b5b-9b098162543b.png)
![part1-process-user-task-flight-completed-2](https://user-images.githubusercontent.com/4369840/139307989-375b5d05-8a1c-4493-a6b2-bd30cbb065dc.png)

To generate the next user task of the process, **TaskHotel** entity, create the metadata files `.jhipster/TaskHotel.json`: 

```
{
  "fields": [
    {
      "fieldName": "name",
      "fieldType": "String",
      "fieldReadOnly": true
    },
    {
      "fieldName": "startDate",
      "fieldType": "LocalDate",
      "fieldReadOnly": true
    },
    {
      "fieldName": "endDate",
      "fieldType": "LocalDate",
      "fieldReadOnly": true
    },
    {
      "fieldName": "hotelName",
      "fieldType": "String"
    },
    {
      "fieldName": "hotelBookingNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [],
  "entityType": "user-task-form",
  "processBpmnId": "TravelPlanProcess",
  "processEntityName": "TravelPlanProcess",
  "taskBpmnId": "TaskHotel",
  "domainEntityName": "TravelPlan",
  "service": "serviceClass",
  "dto": "mapstruct",
  "jpaMetamodelFiltering": false,
  "readOnly": false,
  "pagination": "no",
  "skipFakeData": true,
  "name": "TaskHotel",
  "changelogDate": "20210401000003"
}
```

After creating the metadata, execute the following command on the docker container:

    jhipster entity TaskHotel --regenerate

Now it's time to check the second user task you've just created. 

* Restart the application
* Login with *admin* account
* Navigate to the menu **Administration -> Process Instances**. 
* Click on the **View** button on the process instance you created previously
* Click on the **Play** button from the task *Choose Hotel* shown in the tasks section

![part1-process-user-task-hotel](https://user-images.githubusercontent.com/4369840/139311449-fd2d66a8-2c77-4045-876f-de3df0de49e9.png)

You can also execute this task from the **My Task** menu.

After completing the *Choose Hotel* task, you can check on the process instance details page that the corresponding fiedls were updated (*Hotel Name* and *Hotel Booking Number*) and that the next task in the process (*Rent a car*) is ready to be executed.

![part1-process-user-task-hotel-completed-1](https://user-images.githubusercontent.com/4369840/139311452-12f262dc-1bd2-48d8-a7ca-ff60ab510d39.png)
![part1-process-user-task-hotel-completed-2](https://user-images.githubusercontent.com/4369840/139311454-db801c8a-ae95-43bd-a855-9a330cf49a07.png)


To generate the last user task of the process, **TaskCar** entity, create the metadata files `.jhipster/TaskCar.json`: 

```
{
    "fields": [
        {
            "fieldName": "name",
            "fieldType": "String",
            "fieldReadOnly": true
          },
          {
            "fieldName": "startDate",
            "fieldType": "LocalDate",
            "fieldReadOnly": true
          },
          {
            "fieldName": "endDate",
            "fieldType": "LocalDate",
            "fieldReadOnly": true
          },
          {
            "fieldName": "carCompanyName",
            "fieldType": "String"
          },
          {
            "fieldName": "carBookingNumber",
            "fieldType": "String"
          }
    ],
    "relationships": [],
    "entityType": "user-task-form",
    "processBpmnId": "TravelPlanProcess",
    "processEntityName": "TravelPlanProcess",
    "taskBpmnId": "TaskCar",
    "domainEntityName": "TravelPlan",
    "service": "serviceClass",
    "dto": "mapstruct",
    "jpaMetamodelFiltering": false,
    "readOnly": false,
    "pagination": "no",
    "skipFakeData": true,
    "name": "TaskCar",
    "changelogDate": "20210401000005"
  }
```

After creating the metadata, execute the following command on the docker container:

    jhipster entity TaskCar --regenerate

Now it's time to check the second user task you've just created. 

* Restart the application
* Login with *admin* account
* Navigate to the menu **Administration -> Process Instances**. 
* Click on the **View** button on the process instance you created previously
* Click on the **Play** button from the task *Rent a car* shown in the tasks section

![part1-process-user-task-car](https://user-images.githubusercontent.com/4369840/139313606-8fcef9d0-87e7-4ecd-aa3c-e7ba1e5cd31f.png)

You can also execute this task from the **My Task** menu.

After completing the *Rent a car* task, you can check on the process instance details page that the corresponding fiedls were updated (*Car Company Name* and *Car Booking Number*) and the process is marked as completed.

![part1-process-user-task-car-completed-1](https://user-images.githubusercontent.com/4369840/139313609-ed89080e-b9d9-467c-9f9e-1283487800e5.png)
![part1-process-user-task-car-completed-2](https://user-images.githubusercontent.com/4369840/139313610-a5c52f5a-834a-483c-a8dc-562db5c94226.png)

You've just finished the development of your first process. Congratulations!!!

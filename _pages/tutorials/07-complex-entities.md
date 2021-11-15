---
layout: single
classes: wide
permalink: /tutorials/complex-entities
title: "Complex Entities"
excerpt: "Complex Entities"
last_modified_at: 2021-11-03
toc: false
sidebar:
  nav: "docs"
---

So far we have been handling a very simple domain entity: a single entity with a bunch of fields. In this tutorial we will explore how to handle a more complex domain model. 

Let's consider in this tutorial a more complex version of the domain model for the **Travel Plan Process**:

![part5-reviewed-domain](https://user-images.githubusercontent.com/4369840/141357572-6b066d1e-17f6-4f8a-9614-be3a13585af1.png)

In this version, the entity **Travel Plan** was decomposed and there are three new entities: **AirlineCompany**, **Hotel**, and **RentalCarCompany**.

The remainder of this tutorial is 

### 1. Generating the new domain entities

To generate these entities, you should create their metadata files in the directory `.jhipster` as follow:

**~/.jhipster/AirlineCompany.json**:
```json
{
  "fields": [
    {
      "fieldName": "name",
      "fieldType": "String"
    },
    {
      "fieldName": "website",
      "fieldType": "String"
    },
    {
      "fieldName": "email",
      "fieldType": "String"
    }
  ],
  "relationships": [],
  "entityType": "domain",
  "service": "serviceClass",
  "dto": "mapstruct",
  "jpaMetamodelFiltering": false,
  "readOnly": false,
  "pagination": "no",
  "name": "AirlineCompany",
  "changelogDate": "20211109000001",
  "skipFakeData": true
}
```


**~/.jhipster/Hotel.json**:
```json
{
  "fields": [
    {
      "fieldName": "name",
      "fieldType": "String"
    },
    {
      "fieldName": "website",
      "fieldType": "String"
    },
    {
      "fieldName": "email",
      "fieldType": "String"
    }
  ],
  "relationships": [],
  "entityType": "domain",
  "service": "serviceClass",
  "dto": "mapstruct",
  "jpaMetamodelFiltering": false,
  "readOnly": false,
  "pagination": "no",
  "name": "Hotel",
  "changelogDate": "20211109000002",
  "skipFakeData": true
}
```

**~/.jhipster/RentalCarCompany.json**:
```json
{
  "fields": [
    {
      "fieldName": "name",
      "fieldType": "String"
    },
    {
      "fieldName": "website",
      "fieldType": "String"
    },
    {
      "fieldName": "email",
      "fieldType": "String"
    }
  ],
  "relationships": [],
  "entityType": "domain",
  "service": "serviceClass",
  "dto": "mapstruct",
  "jpaMetamodelFiltering": false,
  "readOnly": false,
  "pagination": "no",
  "name": "RentalCarCompany",
  "changelogDate": "20211109000003",
  "skipFakeData": true
}
```

Important notes about metadata for these domain entities:

* `fields` describes the primitive types of the entity
* `relationships` describes the relationships with other domain entities (not with process entities)
* `entityType` should be `domain`
* `service` should be `serviceClass`
* `dto` should be `mapstruct`
* `jpaMetamodelFiltering` should be `false`
* `readOnly` should be `false` (we want to generate the CRUD support)
* `pagination` should be `no`
* `skipFakeData` should be `true`

After creating the metadata for the new entities, execute the following command on the docker container:

    jhipster entity AirlineCompany --regenerate

The generator will create new files and modify existing files. For each existing file, the generator will ask for confirmation. You should say `y` (yes) to all confirmation questions.

**Note: In spite of informing only the entity `AirlineCompany` in the command line, the generator will identify all new entities in the `.jhipster` directory and generate the code for all of them. So, it's not necessary to execute the command for the entities `Hotel` and `RentalCarCompany`.**
{: .notice--info}

After generating the files, you can (re)start the application and see new entries in the `entities` menu: 

![part5-menu](https://user-images.githubusercontent.com/4369840/141362041-1508d3e0-12e0-40ce-bc9a-12b2c3255298.png)

As we set the `readOnly` property to `false`, the code for creating, updating, and deleting the entities are also provided. Before move to next step, we have to populate our database with airline companies, hotels, and rental car companies. 
You can use the application to create such data:

![part5-crud-airline-company](https://user-images.githubusercontent.com/4369840/141362037-0b47d37e-8fe6-45cf-b438-e9a19ffd3a41.png)

![part5-crud-hotel](https://user-images.githubusercontent.com/4369840/141363996-de327162-4baf-41f5-aab2-008cb1a09b72.png)

![part5-crud-rental-car-company](https://user-images.githubusercontent.com/4369840/141364002-4c1d9718-a887-487d-9f6c-5d745f6751ec.png)

### 2. (Re)Generating the domain entity (TravelPlanProcess)


Now, let's change the **TravelPlan** entity metadata:

**~/.jhipster/TravelPlan.json**:
```json
{
  "fields": [
    {
      "fieldName": "travelName",
      "fieldType": "String"
    },
    {
      "fieldName": "userName",
      "fieldType": "String"
    },
    {
      "fieldName": "userEmail",
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
      "fieldName": "airlineTicketNumber",
      "fieldType": "String"
    },
    {
      "fieldName": "hotelBookingNumber",
      "fieldType": "String"
    },
    {
      "fieldName": "carBookingNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [
    {
      "relationshipName": "airlineCompany",
      "otherEntityName": "airlineCompany",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    },
    {
      "relationshipName": "hotel",
      "otherEntityName": "hotel",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    },
    {
      "relationshipName": "rentalCarCompany",
      "otherEntityName": "rentalCarCompany",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    }
  ],
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

Note that the `relationships` list contains references to the new entities.

After updating the metadata for the **TravelPlan** entity, execute the following command on the docker container:

    jhipster entity TravelPlan --regenerate

The generator will create new files and modify existing files. For each existing file, the generator will ask for confirmation. You should say `y` (yes) to all confirmation questions.

### 3. (Re)Generating the process-bind entity (TravelPlanProcess)

Since we changed the domain entity, update the metadata file `.jhipster/TravelPlanProcess.json`: 

**~/.jhipster/TravelPlanProcess.json**
```json
{
  "fields": [
    {
      "fieldName": "travelName",
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
      "fieldName": "airlineTicketNumber",
      "fieldType": "String"
    },
    {
      "fieldName": "hotelBookingNumber",
      "fieldType": "String"
    },
    {
      "fieldName": "carBookingNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [
    {
      "relationshipName": "airlineCompany",
      "otherEntityName": "airlineCompany",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    },
    {
      "relationshipName": "hotel",
      "otherEntityName": "hotel",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    },
    {
      "relationshipName": "rentalCarCompany",
      "otherEntityName": "rentalCarCompany",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    }
  ],
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

Note that the `relationships` property includes the relationships from domain entity (**TravelPlan**) that should be rendered in the process instance details page.

After updating the metadata, execute the following command on the docker container:

    jhipster entity TravelPlanProcess --regenerate

As before, the generator will show a message informing an existing file will be touched and ask for confirmation. You should say `y` (yes) to all confirmation questions.

### 4. (Re)Generating the start-form entity (TravelPlanStartForm)

Since the start form for this process does not include the new entities, there is no need to change this entity. 

**.jhipster/TravelPlanStartForm.json**
```json
{
  "fields": [
    {
      "fieldName": "travelName",
      "fieldType": "String"
    },
    {
      "fieldName": "userName",
      "fieldType": "String"
    },
    {
      "fieldName": "userEmail",
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

If you need to set a relationship in the start form, you should include it in the property `relationships`.


### 5. (Re)Generating the user-task **TaskFlight**

In this version of the process, the user-task **TaskFlight** refers the entity **AirlineCompany**. 

**~.jhipster/TaskFlight.json**
```json
{
  "fields": [
    {
      "fieldName": "travelName",
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
      "fieldName": "airlineTicketNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [
    {
      "relationshipName": "airlineCompany",
      "otherEntityName": "airlineCompany",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    }
  ],
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

The form for this task shows the following fields: `travelName`, `startDate`, `endDate` (read only), `airlineTicketNumber`, and `airlineCompany`.

After updating the metadata, execute the following command on the docker container:

    jhipster entity TaskFlight --regenerate

As before, the generator will show a message informing an existing file will be touched and ask for confirmation. You should say `y` (yes) to all confirmation questions.

Now it's time to check the form for this task.

* Restart the application
* Login with *admin* account
* Navigate to the menu **Administration -> Process Instances**. 
* Click on the **View** button on the process instance you created previously
* Click on the **Play** button from the task *Choose flight* shown in the tasks section

![part5-user-task-flight](https://user-images.githubusercontent.com/4369840/141422165-438fdac8-486d-45e9-880d-0aac1232df08.png)

Note the **Airline Company** field in the task form is a combobox with all the airlines companies previously created.

 
### 6. (Re)Generating the user-task **TaskHotel** 

In this version of the process, the user-task **TaskHotel** refers the entity **Hotel**. 

**~.jhipster/TaskHotel.json**
```json
{
  "fields": [
    {
      "fieldName": "travelName",
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
      "fieldName": "hotelBookingNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [
    {
      "relationshipName": "hotel",
      "otherEntityName": "hotel",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    }
  ],
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

The form for this task shows the following fields: `travelName`, `startDate`, `endDate` (read only), `hotelBookingNumber`, and `hotel`.

After updating the metadata, execute the following command on the docker container:

    jhipster entity TaskHotel --regenerate

As before, the generator will show a message informing an existing file will be touched and ask for confirmation. You should say `y` (yes) to all confirmation questions.

![part5-user-task-hotel](https://user-images.githubusercontent.com/4369840/141426161-64132e8e-2116-419d-9305-ec5a5ff67768.png)

### 7. (Re)Generating the user-task **TaskCar** 

In this version of the process, the user-task **TaskCar** refers the entity **RentalCarCompany**. 

**~.jhipster/TaskCar.json**
```json
{
  "fields": [
    {
      "fieldName": "travelName",
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
      "fieldName": "carBookingNumber",
      "fieldType": "String"
    }
  ],
  "relationships": [
    {
      "relationshipName": "rentalCarCompany",
      "otherEntityName": "rentalCarCompany",
      "relationshipType": "many-to-one",
      "otherEntityField": "name"
    }
  ],
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

The form for this task shows the following fields: `travelName`, `startDate`, `endDate` (read only), `carBookingNumber`, and `rentalCarCompany`.

After updating the metadata, execute the following command on the docker container:

    jhipster entity TaskCar --regenerate

As before, the generator will show a message informing an existing file will be touched and ask for confirmation. You should say `y` (yes) to all confirmation questions.

![part5-user-task-car](https://user-images.githubusercontent.com/4369840/141426155-007be660-6dab-4d67-8509-ac85ae13e503.png)

**Note**: You can checkout the tag **part5-complex-entities** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the code generated on this step. `git checkout part5-complex-entities`.
{: .notice--warning} 


### Summary

On this tutorial we showed how to handle a slightly more complex domain model. We adapted the entity **TravelPlan** and introduced three new entities: **AirlineCompany**, **Hotel**, and **RentCarCompany**.

We created the metadata for these new entities and generated the CRUD code for them. After that, we adapted the metadata of the entities **TravelPlan**, **TravelPlanProcess**, **TaskFlight**, **TaskHotel**, and **TaskCar** to include the new entities into the `relationships` property.
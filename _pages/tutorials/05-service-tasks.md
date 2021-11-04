---
layout: single
classes: wide
permalink: /tutorials/service-tasks
title: "Service Tasks"
excerpt: "Service Tasks"
last_modified_at: 2021-11-03
toc: false
sidebar:
  nav: "docs"
---

In this tutorial, we will focus on how to implement service tasks. 
A service task is a category of task that is automatically executed by the process engine and that are usually used in situations such as:

- communication with third party systems;
- evaluation of complex condition expressions (see tutorial [Gateways](handling-gateways));
- updating domain entities;
- etc

Service tasks are useful for many other situations and you will find when you should use them when you start to design your processes.

Let's change the **Travel Plan Process** to include a service task *Integrate Airline Company, Hotel, Rent Car Company* that aims at integrating with third party systems at the end of the process:

![part3-process-service-task](https://user-images.githubusercontent.com/4369840/140199677-f6017a14-18cf-448e-a063-f799527022d9.png)

To configure this task, click on the wrench icon and then select the type **Service Task**

![part3-process-service-task-type-configuration](https://user-images.githubusercontent.com/4369840/140200578-6b77c316-c7c3-40a9-8a35-e6bcd9db1231.png)

Then configure the delegate on the *property* tab as follow:

- Implementation: **Delegate Expression**
- Delegate Expression: `${integrateThirdPartyDelegate}`

![part3-process-service-task-delegate-configuration](https://user-images.githubusercontent.com/4369840/140200574-2020a5f0-4e52-451b-bf67-3fd248b799e1.png)

Now create a java delegate class `IntegrateThirdPartyDelegate` that implements the interface `org.camunda.bpm.engine.delegate.JavaDelegate`.

```java
package com.mycompany.myapp.delegate;

import com.mycompany.myapp.service.AirlineService;
import com.mycompany.myapp.service.HotelService;
import com.mycompany.myapp.service.RentCarService;
import com.mycompany.myapp.service.dto.TravelPlanProcessDTO;
import org.camunda.bpm.engine.delegate.DelegateExecution;
import org.camunda.bpm.engine.delegate.JavaDelegate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class IntegrateThirdPartyDelegate implements JavaDelegate {

    @Autowired
    AirlineService airlineService;

    @Autowired
    HotelService hotelService;

    @Autowired
    RentCarService rentCarService;

    @Override
    public void execute(DelegateExecution delegateExecution) throws Exception {
        TravelPlanProcessDTO travelPlanProcess = (TravelPlanProcessDTO) delegateExecution.getVariable("processInstance");

        //Confirming the flight
        airlineService.confirmFlight(travelPlanProcess.getTravelPlan().getAirlineTicketNumber());

        //Confirming the hotel booking
        hotelService.confirmReservation(travelPlanProcess.getTravelPlan().getHotelBookingNumber());

        //Confirming the car booking
        rentCarService.confirmReservation(travelPlanProcess.getTravelPlan().getCarBookingNumber());
    }
}
```

The class **AirlineService** is a Spring component that implements the communication with the airline company system:

```java
package com.mycompany.myapp.service;

import org.springframework.stereotype.Component;

@Component
public class AirlineService {

    public void confirmFlight(String airlineTicketNumber) {
        System.out.println("AirlineService: ###########################################");
        System.out.println("AirlineService: ###########################################");
        System.out.println("AirlineService: ###########################################");
        System.out.println("AirlineService:        CONFIRMING FLIGHT " + airlineTicketNumber);
        System.out.println("AirlineService:        FLIGHT " + airlineTicketNumber + " CONFIRMED");
        System.out.println("AirlineService: ###########################################");
        System.out.println("AirlineService: ###########################################");
        System.out.println("AirlineService: ###########################################\n\n\n");
    }
}
```

The class **HotelService** is a Spring component that implements the communication with the hotel reservation system:

```java
package com.mycompany.myapp.service;

import org.springframework.stereotype.Component;

@Component
public class HotelService {
    public void confirmReservation(String hotelBookingNumber) {
        System.out.println("HotelService: ###########################################");
        System.out.println("HotelService: ###########################################");
        System.out.println("HotelService: ###########################################");
        System.out.println("HotelService:        CONFIRMING BOOKING HOTEL " + hotelBookingNumber);
        System.out.println("HotelService:        BOOKING HOTEL " + hotelBookingNumber + " CONFIRMED");
        System.out.println("HotelService: ###########################################");
        System.out.println("HotelService: ###########################################");
        System.out.println("HotelService: ###########################################\n\n\n");
    }
}
```

The class **RentCarService** is a Spring component that implements the communication with the rental car company reservation system:

```java
package com.mycompany.myapp.service;

import org.springframework.stereotype.Component;

@Component
public class RentCarService {
    public void confirmReservation(String carBookingNumber) {
        System.out.println("RentCarService: ###########################################");
        System.out.println("RentCarService: ###########################################");
        System.out.println("RentCarService: ###########################################");
        System.out.println("RentCarService:        CONFIRMING BOOKING CAR " + carBookingNumber);
        System.out.println("RentCarService:        BOOKING CAR " + carBookingNumber + " CONFIRMED");
        System.out.println("RentCarService: ###########################################");
        System.out.println("RentCarService: ###########################################");
        System.out.println("RentCarService: ###########################################\n\n\n");
    }
}
```

The **Travel Plan Process** now has a *Service Task* called *Integrate Airline Company, Hotel, Rent Car Company* that is automatically executed at the end of the process.

**Note**: You can checkout the tag **part3-handling-service-task** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the BPMN file `src/main/resources/TravelPlanProcess.bpmn`, the delegate `IntegrateThirdPartyDelegate`, and the classes `AirlineService`, `HotelService`, and `RentCarService` used in this step. `git checkout part3-handling-service-task`.
{: .notice--warning}


That's it for today.  
I hope this tutorial was helpful to you and you can implement processes with service tasks.





Comming soon!
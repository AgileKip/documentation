---
layout: single
classes: wide
permalink: /tutorials/handling-gateways
title: "Handling Gateways"
excerpt: "Part02"
last_modified_at: 2021-11-01
toc: false
sidebar:
  nav: "docs"
---

In this tutorial, we will focus on two strategies for working with gateways:

- **Using implicit variables**
- **Using a delegate**

## Using implicit variable

To explore the use of gateways, let's extend the **Travel Plan Process** to make it a little bit cleaver.

![part2-process-with-gateways](https://user-images.githubusercontent.com/4369840/139966505-c9a5e7a4-7c3e-4022-90d9-d3f09b54d38c.png)

On this version of the process, the hotel booking task is skipped for same-day trips. The *fork* exclusive gateway (the first one) has two outputs. The "*more than 1-day trip*" flow is executed if the travel has more than 1 day. Otherwise the "*same day trip*" flow is executed. 

The easiest way to configure this gateway is by using the implicit variable `processInstance`. 

To do so, we can set the "*more than 1-day trip*" flow as follows:

- Condition type: **Expression**
- Expression: `${processInstance.travelPlan.endDate > processInstance.travelPlan.startDate}`

The variable `processInstance` is automatically set by the *AgileKip Reference Architecture* and represents an instance of the *process-binding* entity (in our example **TravelPlanProcess**). 
As we showed in the previous tutorial [Getting Started](/pap-documentation/tutorials/getting-started), the *process-binding* entity has a reference to the *domain* entity (**TravelPlan**). 
Finally, the **TravelPlan** entity has two date fields (*startDate* and *endDate*). The condition expression evaluates to true whether the end date is after the start date. 

![part2-gateway-flow-more-than-one-day-trip](https://user-images.githubusercontent.com/4369840/139966499-6205f1fe-95a2-4685-ab9e-033cc096ca7d.png)

A similar configuration can be done in the "*same day trip*" flow:

- Condition type: **Expression**
- Expression: `${! (processInstance.travelPlan.endDate > processInstance.travelPlan.startDate) }`

![part2-gateway-flow-same-day-trip](https://user-images.githubusercontent.com/4369840/139966503-26067ad6-d195-4dab-90ed-ee0c625a5e88.png)


**Note**: You can checkout the tag **part2-bpmn-with-simple-gateways** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the BPMN file `src/main/resources/TravelPlanProcess.bpmn` used in this step. `git checkout part2-bpmn-with-simple-gateways`.
{: .notice--warning}


## Using a delegate

As we said before, using conditional expressions with the implicit variable `processInstance` is the easiest way to configure gateways. However, there are many situations for which these expressions are not suitable. For example for complex expressions or for conditinal expressions that requires a more sofisticated algorithm. In this scenarios, we can use delegates to configure gateways. 

Let's update the **Travel Plan Process** to use this strategy:


![part2-process-with-delegate-gateways](https://user-images.githubusercontent.com/4369840/140020970-f9578667-1037-4743-b514-d08dc9a1fd5c.png)

In this version of the process, we included a delegate just before the *fork* gateway. The aim of this delegate is to calculate whether the trip has more than one day and set the result of this calculation as a variable in the process engine so it can be used later in the process.

The delegate is configured as follow:

- Implementation: **Delegate Expression**
- Delegate Expression: `${calculateSameDayTripDelegate}`


![part2-process-with-delegate-gateways-delegate-configuration](https://user-images.githubusercontent.com/4369840/140080167-4a751726-f545-45be-a124-e6d53c3c9382.png)

Now it is necessary to create a java class named `CalculateSameDayTripDelegate` that implements the interface `JavaDelegate` and mark it with the `@Component` Spring annotation. An implementation of this delagate is shown below: 

```java
package com.mycompany.myapp.delegate;

import com.mycompany.myapp.service.dto.TravelPlanProcessDTO;
import org.camunda.bpm.engine.delegate.DelegateExecution;
import org.camunda.bpm.engine.delegate.JavaDelegate;
import org.springframework.stereotype.Component;

@Component
public class CalculateSameDayTripDelegate implements JavaDelegate {

    @Override
    public void execute(DelegateExecution delegateExecution) throws Exception {
        TravelPlanProcessDTO travelPlanProcess = (TravelPlanProcessDTO) delegateExecution.getVariable("processInstance");
        Boolean moreThan1DayTrip = travelPlanProcess.getTravelPlan().getEndDate().isAfter(travelPlanProcess.getTravelPlan().getStartDate());
        delegateExecution.setVariable("moreThan1DayTrip", moreThan1DayTrip);
    }
}
```

Now that the delegate makes things simpler, we can just use the `moreThan1DayTrip` variable on the gateway flows configuration:

For the *more than 1 day trip* flow:
- Condition type: **Expression**
- Expression: `${moreThan1DayTrip}`


![part2-process-with-delegate-gateways-flow1-configuration](https://user-images.githubusercontent.com/4369840/140080173-a49ef98a-4bb1-4313-a08b-f58413c4f1c6.png)


For the *same day trip* flow:
- Condition type: **Expression**
- Expression: `${!moreThan1DayTrip}`

![part2-process-with-delegate-gateways-flow2-configuration](https://user-images.githubusercontent.com/4369840/140080178-c8d46ef4-c36d-482f-a8b2-5ed4244d2a28.png)

**Note**: You can checkout the tag **part2-bpmn-with-delegate-and-gateway** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the BPMN file `src/main/resources/TravelPlanProcess.bpmn` and the delegate `CalculateSameDayTripDelegate` used in this step. `git checkout part2-bpmn-with-delegate-and-gateway`.
{: .notice--warning}


## Summary

In this tutorial we showed two strategies to handle gateways: (a) **Using the `processInstance` implicit variable**, and (b) **Using a delegate**. Although using the implicit variable is simpler, there are situation for which the use of such strategy is not suitable. For these more complicated situations, you can use a delegate just before the gateway to set a variable to be used in the flows configuration.

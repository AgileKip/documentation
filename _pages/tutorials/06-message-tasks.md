---
layout: single
classes: wide
permalink: /tutorials/message-tasks
title: "Message Tasks"
excerpt: "Message Tasks"
last_modified_at: 2021-11-03
toc: false
sidebar:
  nav: "docs"
---


Sending emails is something very common in process automation so in this tutorial we will focus on how to send emails using message tasks. 

*Message tasks* are quite similar to *service tasks* in the following way: 
(a) they are automatically executed by the process engine; and
(b) they are configured the same way (see our other tutorial [Service Tasks](/pap-documentation/tutorials/service-tasks))

So let's change the **Travel Plan Process** to include a message task that aims at sending an email to the user with a summary of the travel plan at the end of the process:

![message-task-new-message-task](https://user-images.githubusercontent.com/4369840/140585323-cda8e2ca-7dfb-4d64-b0ef-f47a424dc979.png)


To configure this new task, click on the wrench icon and then select the type **Message Task**.

![message-task-new-message-task-configure-task-type](https://user-images.githubusercontent.com/4369840/140586869-5e8f1951-ec77-41a2-94d4-fd27cb6cc50d.png)



Then configure the delegate on the *property* tab as follow:

- Implementation: **Delegate Expression**
- Delegate Expression: `${emailTravelPlanSummaryDelegate}`

![message-task-new-message-task-configure-message-task](https://user-images.githubusercontent.com/4369840/140586871-0be601ed-f064-4386-bc87-e35ff37eb96f.png)

Now create a java delegate class `EmailTravelPlanSummaryDelegate` that implements the interface `org.camunda.bpm.engine.delegate.JavaDelegate`.

```java
package com.mycompany.myapp.delegate;

import com.mycompany.myapp.service.dto.TravelPlanDTO;
import com.mycompany.myapp.service.dto.TravelPlanProcessDTO;
import org.camunda.bpm.engine.delegate.DelegateExecution;
import org.camunda.bpm.engine.delegate.JavaDelegate;
import org.springframework.stereotype.Component;

@Component
public class EmailTravelPlanSummaryDelegate implements JavaDelegate {

    @Override
    public void execute(DelegateExecution delegateExecution) throws Exception {
        TravelPlanProcessDTO travelPlanProcess = (TravelPlanProcessDTO) delegateExecution.getVariable("processInstance");
        TravelPlanDTO travelPlan = travelPlanProcess.getTravelPlan();
        //TODO: there are some coding to be done here using the processBinding entity (TravelPlanProcess)...
    }
}
```

So far we have configured the *message task* in the BPMN file and created an empty java delegate *EmailTravelPlanSummaryDelegate*. 
We still have some code to do to enable the delegate to actually send the email. 
An easy way to send emails in our reference architecture is by using the *MailService* component.

### **MailService** Component 

The *MailService* component has a few methods specialized on sending emails.
We will explore here the method `sendEmail` that is pretty straighforward and receives three parameters: `to`, `subject`, and `content`. 
So let's see how to build these parameters starting by the destination email address.

```java
String to = travelPlan.getUserMail();
```

The destination email address (represented here by the `to` variable) comes from the **TravelPlan** domain entity.

```java
String subject = "[AgileKip] Summary of your travel " + travelPlan.getTravelName();
```
The email subject (represented here by the `subject` variable) is also calculated using the **TravelPlan** domain entity.

To build the email content, we will use the framework [*Thymeleaf*](https://www.thymeleaf.org/):

```java
Context context = new Context(Locale.getDefault());
context.setVariable("travelPlan", travelPlan);
String content = templateEngine.process("travelPlanProcess/travelPlanSummaryEmail", context);
```

As you see, the building of the email content is a little bit trickier.
Basically you have to:

- Create a `context` object to encapsule all the data *Thymeleaf* will need. In our example, the `travelPlan` object has all the data required to build the email content.
- Create a *Thymeleaf* template for the email content.

A *Thymeleaf* template is an html file annotated with some *Thymeleaf* extensions.
The framework searchs for the templates in the folder `main/resources/template`. So, we have to create the file `main/resources/template/travelPlanProcess/travelPlanSummaryEmail.html`.  
The project structure and the content of this file is shown below:

![message-task-project-structure-for-tempates](https://user-images.githubusercontent.com/4369840/140586865-0ab37813-dbac-4fdd-a2b5-5d89bc949e19.png)


```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:lang="${#locale.language}" lang="en">
  <head>
    <title>Travel Plan Summary</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
    <p>Dear <span th:text="${travelPlan.userName}">userName</span></p>
    <p>Here is a summary of your travel <b th:text="${travelPlan.travelName}">travelName</b></p>
    <ul>
        <li><b>Start</b>: <span th:text="${travelPlan.startDate }"></span></li>
        <li><b>End</b>: <span th:text="${travelPlan.endDate }"></span></li>
        <li><b>Flight</b>: <span th:text="${travelPlan.airlineCompanyName}"></span> / <span th:text="${travelPlan.airlineTicketNumber}"></span></li>
        <li><b>Hotel</b>: <span th:text="${travelPlan.hotelName}"></span> / <span th:text="${travelPlan.hotelBookingNumber}"></span></li>
        <li><b>Rental Car</b>: <span th:text="${travelPlan.carCompanyName}"></span> / <span th:text="${travelPlan.carBookingNumber}"></span></li>
    </ul>
    <p>
      Regards,
      <br />
      <em>AgileKip Team</em>
    </p>
  </body>
</html>
```

You can find more information about the *Thymeleaf* framework on its official website: [https://www.thymeleaf.org/](https://www.thymeleaf.org/)

Let's put everything together and see the final code of the `EmailTravelPlanSummaryDelegate` delegate:  


```java
package com.mycompany.myapp.delegate;

import com.mycompany.myapp.service.MailService;
import com.mycompany.myapp.service.dto.TravelPlanDTO;
import com.mycompany.myapp.service.dto.TravelPlanProcessDTO;
import org.camunda.bpm.engine.delegate.DelegateExecution;
import org.camunda.bpm.engine.delegate.JavaDelegate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.thymeleaf.context.Context;
import org.thymeleaf.spring5.SpringTemplateEngine;

import java.util.Locale;

@Component
public class EmailTravelPlanSummaryDelegate implements JavaDelegate {

    @Autowired
    MailService mailService;

    @Autowired
    SpringTemplateEngine templateEngine;

    @Override
    public void execute(DelegateExecution delegateExecution) throws Exception {
        TravelPlanProcessDTO travelPlanProcess = (TravelPlanProcessDTO) delegateExecution.getVariable("processInstance");
        TravelPlanDTO travelPlan = travelPlanProcess.getTravelPlan();
        String to = travelPlan.getUserEmail();
        String subject = "[AgileKip] Summary of your travel " + travelPlan.getTravelName();
        Context context = new Context(Locale.getDefault());
        context.setVariable("travelPlan", travelPlan);
        String content = templateEngine.process("travelPlanProcess/travelPlanSummaryEmail", context);
        mailService.sendEmail(to, subject, content, false, true);
    }
}
```

**Note**: You can checkout the tag **part4-message-task** from the [TravelPlan repository on Github](https://github.com/AgileKip/travel-plan-tutorial) to get the BPMN file `src/main/resources/TravelPlanProcess.bpmn`, the delegate `EmailTravelPlanSummaryDelegate`, and the template `travelPlanSummaryEmail.html` used in this tutorial. `git checkout part4-message-task`.
{: .notice--warning}

### Checking the resulting email (optional... but recommended)

We have already done the coding necessary to send the desired email.
However, if you execute the **TravelPlan** process on development mode, you are probably not receiving any email at all and are getting instead the following error:

```
2021-11-05 18:58:50.216  WARN 22861 --- [vel-plan-task-2] com.mycompany.myapp.service.MailService  : Email could not be sent to user 'ulisses.telemaco@gmail.com'

org.springframework.mail.MailSendException: Mail server connection failed; nested exception is com.sun.mail.util.MailConnectException: Couldn't connect to host, port: localhost, 25; timeout -1;
  nested exception is:
	java.net.ConnectException: Connection refused (Connection refused). Failed messages: com.sun.mail.util.MailConnectException: Couldn't connect to host, port: localhost, 25; timeout -1;
  nested exception is:
	java.net.ConnectException: Connection refused (Connection refused)
	at org.springframework.mail.javamail.JavaMailSenderImpl.doSend(JavaMailSenderImpl.java:448)
	at org.springframework.mail.javamail.JavaMailSenderImpl.send(JavaMailSenderImpl.java:361)
	at org.springframework.mail.javamail.JavaMailSenderImpl.send(JavaMailSenderImpl.java:356)
	at com.mycompany.myapp.service.MailService.sendEmail(MailService.java:73)
	at com.mycompany.myapp.service.MailService$$FastClassBySpringCGLIB$$2ee4e862.invoke(<generated>)
	at org.springframework.cglib.proxy.MethodProxy.invoke(MethodProxy.java:218)
	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.invokeJoinpoint(CglibAopProxy.java:779)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:163)
	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:750)
	at org.springframework.aop.aspectj.AspectJAfterThrowingAdvice.invoke(AspectJAfterThrowingAdvice.java:64)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:750)
	at org.springframework.aop.aspectj.MethodInvocationProceedingJoinPoint.proceed(MethodInvocationProceedingJoinPoint.java:89)
	at com.mycompany.myapp.aop.logging.LoggingAspect.logAround(LoggingAspect.java:105)
	at jdk.internal.reflect.GeneratedMethodAccessor195.invoke(Unknown Source)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:566)
	at org.springframework.aop.aspectj.AbstractAspectJAdvice.invokeAdviceMethodWithGivenArgs(AbstractAspectJAdvice.java:634)
	at org.springframework.aop.aspectj.AbstractAspectJAdvice.invokeAdviceMethod(AbstractAspectJAdvice.java:624)
	at org.springframework.aop.aspectj.AspectJAroundAdvice.invoke(AspectJAroundAdvice.java:72)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:750)
	at org.springframework.aop.interceptor.ExposeInvocationInterceptor.invoke(ExposeInvocationInterceptor.java:97)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:750)
	at org.springframework.aop.interceptor.AsyncExecutionInterceptor.lambda$invoke$0(AsyncExecutionInterceptor.java:115)
	at java.base/java.util.concurrent.FutureTask.run$$$capture(FutureTask.java:264)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java)
	at tech.jhipster.async.ExceptionHandlingAsyncTaskExecutor.lambda$createWrappedRunnable$1(ExceptionHandlingAsyncTaskExecutor.java:78)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: com.sun.mail.util.MailConnectException: Couldn't connect to host, port: localhost, 25; timeout -1
	at com.sun.mail.smtp.SMTPTransport.openServer(SMTPTransport.java:2210)
	at com.sun.mail.smtp.SMTPTransport.protocolConnect(SMTPTransport.java:722)
	at javax.mail.Service.connect(Service.java:342)
	at org.springframework.mail.javamail.JavaMailSenderImpl.connectTransport(JavaMailSenderImpl.java:518)
	at org.springframework.mail.javamail.JavaMailSenderImpl.doSend(JavaMailSenderImpl.java:437)
	... 31 common frames omitted
Caused by: java.net.ConnectException: Connection refused (Connection refused)
	at java.base/java.net.PlainSocketImpl.socketConnect(Native Method)
	at java.base/java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:399)
	at java.base/java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:242)
	at java.base/java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:224)
	at java.base/java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.base/java.net.Socket.connect(Socket.java:608)
	at java.base/java.net.Socket.connect(Socket.java:557)
	at com.sun.mail.util.SocketFetcher.createSocket(SocketFetcher.java:335)
	at com.sun.mail.util.SocketFetcher.getSocket(SocketFetcher.java:214)
	at com.sun.mail.smtp.SMTPTransport.openServer(SMTPTransport.java:2160)
	... 35 common frames omitted
```

This is happening because we don't have an SMTP server configured in our development environment.
To fix this problem, we recommend the use of the tool [**FakeSMTP**](http://nilhcem.com/FakeSMTP/).
This is a very handy tool that can be used on development mode to mock an SMTP server.

After downloading the tool and executing it, set the port to `25` and start the server:

![message-task-fakesmtp-configuration](https://user-images.githubusercontent.com/4369840/140586873-ecb8b2a3-1cd1-492d-90e7-d607be5abb52.png)

![message-task-fakesmtp-waiting-emails](https://user-images.githubusercontent.com/4369840/140585325-cca1dbf2-3543-4eed-a1c8-039763cc9280.png)

The *fakeSMTP* is running and waiting for emails.
Now start the **TravelPlan** process, execute all the user tasks (*Flight*, *Hotel*, and *Car*) and check on the fakeSMTP interface for the new email:

![message-task-fakesmtp-email-received](https://user-images.githubusercontent.com/4369840/140585328-4f8d41cd-ee95-4cb7-99aa-2cf0cfeaac93.png)

![message-task-email-example](https://user-images.githubusercontent.com/4369840/140585327-3d16530f-9747-417d-a9ae-516d35a15963.png)

The email with the *Travel Plan* summary was generated and sent to the user who started the process.

Congratulations, you've just learned how to send emails on our platform.


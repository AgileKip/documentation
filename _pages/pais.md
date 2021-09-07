---
layout: single
classes: wide
permalink: /pais
title: "Process Automation"
excerpt: "Our goals"
last_modified_at: 2021-01-20
toc: false
sidebar:
  nav: "docs"
---

Process Automation is intended to support the execution of recurring tasks and thus streamline how businesses and organizations work. Automation is typically achieved when tasks, roles and information are organized in workflows (connected tasks) and used as a standard way to execute procedures throughout the organization. Moreover, such workflow can be used to implement special software systems that control and track how and when tasks are executed. For example, the workflow for handling a new employee may have tasks such as _Prepare Desk and Equipment_, _Grant Access To Systems_ and _Send New Hire the Welcome Pack_. Each task may be handled by a different person from a different department within the organization. Having a systematic and automated way to executed those tasks is important to avoid mishaps when onboarding new hires, such as forgetting to create an email account (which is part of _Granting Access To Systems_) or sending the Welcome Pack twice for the same address. Automating the _Employee Onboarding_ process is even more crucial for big organizations where they have to keep track of several new hires daily and mishaps cause bigger problems (e.g. compliance issues).

Process Automation is not only about supporting and organizing people's work but also about streamlining how systems are used to support the current workflow. For example, the _Employee Onboarding_ process may have a _User Task_ to inform the new hire's name and a _Systems Task_ to send that name to the back-end access control systems. These two tasks must be synchronized and executed properly (as in a transaction) to guarantee the process achieve the desired goals.

Automation can go even further and systematize sending messages, making decisions and handling errors, providing an end-to-end solution for the whole workflow. In addition, organizing how import (if not all) workflows are executed within organizations brings the added benefit of measuring how each and every task is executed, in which order, for how long and by who. This added benefit is quite important when organizations are subject to process improvement cycles such as the PDCA, as tasks that are taking too long may be re-engineered to achieve better results.

There are several ways to implement process automation within organizations and using bespoke software systems or by combining a Workflow Management System and some Information Systems such as CRMs, ERPs, etc., are among the most common ones. The former approach requires a huge implementation effort as a synchronization subsystem is mandatory to ensure proper task sequencing. As a result, using a Workflow Management System(WFMS) is widely adopted and has several benefits such as builtin synchronization, user management, messaging, transaction control, error-handling, etc. Another benefit comes from the fact that modern WFMS are capable of interpreting and executing workflow models specified using the Business Process Modeling Notation (BPMN), an industry standard when it comes to process modeling.

BPMN allows software engineers to describe several types of tasks and how they should be connected (sequenced) to achieve the proper outcome. BPMN also allows defining events, lanes and pools (kind of roles), making decisions, calling other processes, etc.

{% include figure image_path="/assets/images/dumb_onboarding.png" alt="Sample Process" caption="Fig 1. Sample Onboarding Processes." %}

Fig1 exposes two BPMN models for our illustrative _Onboarding Process_ and they demonstrate BPMN expressiveness. Both models bring the _Prepare Desk and Equipment_, _Grant Access To Systems_ and _Send New Hire the Welcome Pack_ tasks as UserTasks and a _Do Something Else_ task as a ServiceTask (the top left icons are different). Although the process models may look similar they allow completely different behaviors. The model starting with A indicates all tasks MUST be executed in the prescribed sequence and as a result the only possible workflow trace for onboarding the new hire John is: 1) _Prepare Desk and Equipment_ --> 2) _Grant Access To Systems_ --> 3) _Send New Hire the Welcome Pack_ --> 4) _Do Something Else_. Onboarding another employee MUST follow the same sequence, otherwise the process won't finish successfully. This process is even more restrictive as the subsequent task is only available after the preceding task is finished.

A careful analysis of the process model starting with B (at the bottom of Fig1) reveals that some tasks may be executed in parallel as they are surrounded by two _Parallel Gateways_. The tasks are quite the same but _Send New Hire the Welcome Pack_ may be executed before _Prepare Desk and Equipment_, or after, or concurrently, depending on how resources become available.

This is just the tip of the iceberg on what Process Automation can do and our approach is designed to leverage on BPMN and a WFMS to generate the process-aware application that will systematize the workflow.

Cheers and follow us on social networks!

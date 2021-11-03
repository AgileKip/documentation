---
layout: single
classes: wide
permalink: /tutorials/generating-an-app
title: "Generating, starting, and testing an app"
excerpt: "GeneratingAnApp"
last_modified_at: 2021-10-25
toc: false
sidebar:
  nav: "docs"
---

In this tutorial we will show how to generate, start, and test an app using the **AgileKip Generator**.

We are assuming you have already installed the generator according to the tutorial [Installing AgileKIP Generator](installation).

The steps to generate, start and test an app are:

1. Connecting in the docker container
1. Creating a working directory for the project
1. Executing the AgileKip Generator
1. Disconnecting from the docker container
1. Starting the application
1. Navigating in the application

### 1. Connecting in the docker container

This command starts a shell session in the docker container previously created (see [Installing AgileKIP Generator](installation)). 
If you are using an IDE (such as *Visual Code Studio* or *IntelliJ*) that has the feature to attach a shell to a started docker container, this step is not necessary.

    HOST-MACHINE$ docker container exec -it agilekip.v0.0.11 bash


### 2. Creating a working directory for the project

By default, the docker container shell is started on the directory `/home/jhipster/app/`. The commands below create a subdirectory for the new project.
    
    DOCKER-MACHINE:~/app$ mkdir myapp
    DOCKER-MACHINE:~/app$ cd myapp

### 3. Executing the generator

To generate the app, you should execute the `jhipster` command indicating you are using the AgileKIP Generator (which is JHipster blueprint):

    DOCKER-MACHINE:~/app/myapp$ jhipster --blueprints agilekip --skip-jhipster-dependencies

This command starts a wizard composed of 21 questions. You can answer them as follow (basically accepting the default values):

- Which type of application would you like to create? **Monolithic application**
- What is the base name of your application? **myapp**
- Do you want to make it reactive with Spring WebFlux? **No**
- What is your default Java package name? **com.mycompany.myapp**
- Which type of authentication would you like to use? **JWT authentication**
- Which type of database would you like to use? **SQL**
- Which production database would you like to use? **PostgreSQL**
- Which development database would you like to use? **H2 with disk-based persistence**
- Which cache do you want to use? **Ehcache**
- Do you want to use Hibernate 2nd level cache? **Yes**
- Would you like to use Maven or Gradle for building the backend? **Maven**
- Do you want to use the JHipster Registry to configure, monitor and scale your application? **No**
- Which other technologies would you like to use? 
- Which Framework would you like to use for the client? **Vue**
- Do you want to generate the admin UI? **Yes**
- Would you like to use a Bootswatch theme? **Default JHipster**
- Would you like to enable internationalization support? **Yes**
- Please choose the native language of the application: **English**
- Please choose additional languages to install 
- Besides JUnit and Jest, which testing frameworks would you like to use? 
- Would you like to install other generators from the JHipster Marketplace? **No**

Next AgileKIP Generator generates the application and prepares your application for running. 
You will see something like this in your console:

```
KeyStore 'src/main/resources/config/tls/keystore.p12' generated successfully.

   create .prettierrc
   create .prettierignore
   create .gitattributes
   create .editorconfig
   create .gitignore
   create sonar-project.properties
   create .huskyrc
   create mvnw
   create mvnw.cmd
   create .mvn/wrapper/maven-wrapper.jar
   create .mvn/wrapper/maven-wrapper.properties
   create .mvn/wrapper/MavenWrapperDownloader.java
   create src/main/resources/banner.txt
   create src/main/resources/config/liquibase/changelog/00000000000000_initial_schema.xml
   create src/main/resources/config/liquibase/master.xml
   create src/main/docker/jib/entrypoint.sh
   create checkstyle.xml
   create pom.xml
   create src/main/resources/.h2.server.properties
   create src/main/resources/logback-spring.xml
   create src/main/resources/i18n/messages.properties
   create package.json
   ...
   create src/main/webapp/i18n/en/register.json
   create src/main/webapp/i18n/en/sessions.json
   create src/main/webapp/i18n/en/settings.json
   create src/main/webapp/i18n/en/user-management.json
   create src/main/webapp/i18n/en/activate.json
   create src/main/webapp/i18n/en/global.json
   create src/main/webapp/i18n/en/health.json
   create src/main/webapp/i18n/en/reset.json
Git repository initialized.

Changes to package.json were detected.

Running npm install for you to install the required dependencies.

added 2831 packages, and audited 2832 packages in 7m

70 vulnerabilities (23 moderate, 47 high)

To address issues that do not require attention, run:
  npm audit fix

To address all issues possible (including breaking changes), run:
  npm audit fix --force

Some issues need review, and may require choosing
a different dependency.

Run `npm audit` for details.
Application commit to Git failed from /home/jhipster/app/myapp. Try to commit manually.

If you find JHipster useful consider sponsoring the project https://www.jhipster.tech/sponsors/

Server application generated successfully.

Run your Spring Boot application:
./mvnw

Client application generated successfully.

Start your Webpack development server with:
 npm start


> myapp@0.0.0 clean-www
> rimraf target/classes/static/app/{src,target/}

Congratulations, JHipster execution is complete!
Sponsored with ❤️  by @oktadev.
jhipster@ab2d5e1138cf:~/app/myapp$ 
```

### 4. Disconnecting from the docker container
 
After generating the application, you do not need to work in the docker container. To return to your host machine just enter `exit`:

    DOCKER-MACHINE:~/app/myapp$ exit


### 5. Starting the application

Now your application is ready to be started on your host machine with the following command:

    HOST-MACHINE$ ./mvnw

This command takes a few minutes to complete. Your application is successfully started when you see the following message on the console:

```
----------------------------------------------------------
        Application 'travelPlan' is running! Access URLs:
        Local:          http://localhost:8080/
        External:       http://192.168.0.29:8080/
        Profile(s):     [dev, api-docs]
----------------------------------------------------------
```

### 6. Testing the application

To test your application, open a browser and navigate to the URL [http://localhost:8080/](http://localhost:8080/)


![Screen Shot 2021-10-26 at 01 56 12](https://user-images.githubusercontent.com/4369840/138817629-4be20620-1c6e-43ec-81d5-29db06de01a0.png)

If you want to sign in, you can try the default accounts:

- Administrator (login="admin" and password="admin")
- User (login="user" and password="user").


Among the features already intalled, you can try:

* User Management
* Process Definition
* Process Deployment
* Process Instance
* All Tasks
* My Tasks  

Congratulation!!! You generated your first application using the AgileKip Generator.

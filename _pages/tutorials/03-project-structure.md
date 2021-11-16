---
layout: single
classes: wide
permalink: /tutorials/project-structure-and-tech-stack
title: "Project Structure and Tech Stack"
excerpt: "Project Structure and Tech Stack"
last_modified_at: 2021-11-16
toc: false
sidebar:
  nav: "docs"
---

This tutorial explains the project structure and tech stack of our applications and is divided into three parts:
- Client Side Code
- Server Side Code, and 
- Resources Code.

## Client Side Code

### Project structure on the client side code

The client code for our applications can be found under `src/main/webapp`.

Here is the main project structure:

```plaintext
webapp
├── app                             - Your application
│   ├── account                     - Account related components
│   ├── admin                       - Administration related components
│   ├── core                        - Main components such as Home, navbar, ...
│   ├── entities                    - Generated entities (domain, process-binding, start-form, and user-task entities)
│   ├── locale                      - I18n / translation related components
│   ├── router                      - Routing configuration
│   ├── shared                      - Shared elements such as your config, models and util classes
│   ├── app.component.ts            - The application main class
│   ├── app.vue                     - The application main SFC component
│   ├── constants.ts                - Global application constants
│   ├── main.ts                     - Index script, application entrypoint
│   └── shims-vue.d.ts
├── content                         - Contains your static files such as images and fonts
├── i18n                            - Translation files
├── swagger-ui                      - Swagger UI front-end
├── 404.html                        - 404 page
├── favicon.ico                     - Fav icon
├── index.html                      - Index page
└── manifest.webapp                 - Application manifest
```

### Technology stack on the client side

Single Web page application:

*   [Vue](https://vuejs.org/)
*   [vue-class-component](https://github.com/vuejs/vue-class-component) style and guidelines
*   Responsive Web Design with [Twitter Bootstrap](http://getbootstrap.com/)
*   [HTML5 Boilerplate](http://html5boilerplate.com/)
*   Compatible with modern browsers (Chrome, FireFox, Microsoft Edge...)
*   Full internationalization support with [vue-i18n](https://kazupon.github.io/vue-i18n/)
*   Optional [Sass](https://www.npmjs.com/package/node-sass) support for CSS design
*   Installation of new JavaScript libraries with [NPM](https://www.npmjs.com/get-npm)
*   Build, optimization and live reload with [Webpack](https://webpack.js.org/)
*   Testing with [Jest](https://facebook.github.io/jest/) and [Protractor](http://www.protractortest.org)


## Server Side Code

### Project structure on the server side code

The server code is composed of java files that can be found under `src/main/java/${ROOT_JAVA_PACKAGE}` where `ROOT_JAVA_PACKAGE` represents the `packageName` property you defined when you generated the application.

Here is the main project structure for the server side code:

```plaintext
java/ROOT_JAVA_PACKAGE
├── aop.logging             - Classes used for aspect oriented features
├── config                  - Configuration Spring Components. Classes annoted with @Configuration
├── delegate                - Java delegates used in the Service Tasks. Classes annoted with @Component
├── domain                  - Domain entities. Classes annoted with @Entity
├── process                 - Contains subpackages for each process you generated
├── repositor               - Repository Spring Components. Classes annoted with @Repository
├── security                - Security related classe
├── service                 - Service Spring Components. Classes annotated with @Service
│   ├── dto                 - Plain java classes known as DTOs (a lighter version of the domain entities)
│   └── mapper              - Mapstruct Components known as Mappers
├── web.rest                - Controller Spring Components. Classes annoted with @RestController
├── ApplicationWebXml.java  - Java class used when the application is deployed as a WAR file
└── MyApp.java              - Main java class that is used when the application is executed as a JAR file
```

### Technology stack on the server side

A complete [Spring application](http://spring.io/):

*   [Spring Boot](http://projects.spring.io/spring-boot/) for application configuration
*   [Maven](http://maven.apache.org/) configuration for building, testing and running the application
*   [Spring Security](http://docs.spring.io/spring-security/site/index.html)
*   [Spring MVC REST](http://spring.io/guides/gs/rest-service/) + [Jackson](https://github.com/FasterXML/jackson)
*   [Spring Data JPA](http://projects.spring.io/spring-data-jpa/) + Bean Validation
*   [MapStruct](https://mapstruct.org/) for java bean mappings
*   Database updates with [Liquibase](http://www.liquibase.org/)
*   [Thymeleaf](http://www.thymeleaf.org/) template engine for generating emails and web pages on the server side


## Resources Code

Resources code is composed of supporting artifacts and can be found under `src/main/resources/`. 
Here is the structure for the resources code:

```plaintext
java/ROOT_JAVA_PACKAGE
├── bpmn                       - BPMN files to be deployed into the application
├── config                     - Spring and liquibase configuration files
│   ├── liquibase              - Liquibase configuration files 
│      ├── changelog           - Liquibase changeset files
│      ├── data                - Liquibase fake data files
│      └── master.xml          - Liquibase master file
│   └── tls/keystore.p12       - Digital certificate that uses PKCS#12 (Public Key Cryptography Standard #12) encryption
│   ├── application.yml        - Spring general configuration file (shared between all profiles)
│   ├── application-dev.yml    - Spring configuration file for developing mode (profile dev)
│   ├── application-prod.yml   - Spring configuration file for production mode (profile prod)
│   └── application-tls.yml    - Spring configuration file for TLS purpose (profile tls)
├── i18n                       - I18n files.
│   ├── messages.properties    - General i18n file
│   └── messages_en.properties - I18n file for English language
├── templates                  - Thymeleaf template files
│   ├── mail                   - Thymeleaf template files for emails
│   └── error.html             - Thymeleaf template file for the error page
├── banner.txt                 - Banner file (shown in the shell when the app is starting)
└── logback-spring.xml         - Logback Spring configuration file (for logging purposes)
```
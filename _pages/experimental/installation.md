---
layout: single
classes: wide
permalink: /experimental/installation
title: "Installing AgileKIP Generator"
excerpt: "InstallingAgileKIPGenerator"
last_modified_at: 2021-10-25
toc: false
sidebar:
  nav: "docs"
---

In this tutorial we will show how to set up the **AgileKip Generator** in your environment.

## Docker Installation 

The easiest way to install the AgileKip Generator is using the official docker image. 

```
HOST-MACHINE$ cd MY_WORK_DIRECTORY
HOST-MACHINE$ docker container run \
  --name agilekip.v0.0.11 \
  -v $PWD:/home/jhipster/app \
  -d -t agilekip/generator-jhipster-agilekip:v0.0.11
```

You should replace `MY_WORK_DIRECTORY` for your work directory (example: `/home/utelemaco/development/agilekip-generator`)

* Container name: **agilekip.v0.0.11**
* Container image: **agilekip/generator-jhipster-agilekip:v0.0.11**
* Mounted volume: `MY_WORK_DIRECTORY` in the host machine is mounted to `/home/jhipster/app` in the docker container


The command below starts a shell session in the docker container **agilekip.v0.0.11**. 
If you are using an IDE (such as *Visual Code Studio* or *IntelliJ*) that has the feature to attach a shell to a started docker container, this step is not necessary.

    HOST-MACHINE$ docker container exec -it agilekip.v0.0.11 bash

To check the AgileKip Generator installation, you can execute the following command: 

```
jhipster@ab2d5e1138cf:~$ npm info generator-jhipster-agilekip

generator-jhipster-agilekip@0.0.11 | Proprietary | deps: 2 | versions: 1
The community edition of the JHipster AgileKip, a generator to scaffold web app integrated with the Camunda platform
https://github.com/agilekip/generator-jhipster-agilekip

keywords: yeoman-generator, jhipster-blueprint, jhipster-7

dist
.tarball: https://registry.npmjs.org/generator-jhipster-agilekip/-/generator-jhipster-agilekip-0.0.11.tgz
.shasum: be64318d6c8b061f520bb2ccf25db09a8c97f40a
.integrity: sha512-I9tgZRM0agAa1uuqIUr3zuhdrtNL2VSojjXDJqrFLmehiVQcUPmhKyFnqs9VhMF2/lj8wZDcTVkbjcj678kPHQ==
.unpackedSize: 1.4 MB

dependencies:
chalk: 2.4.1              generator-jhipster: 7.0.1 

maintainers:
- utelemaco <ulisses.telemaco@gmail.com>

dist-tags:
latest: 0.0.11  

published a week ago by utelemaco <ulisses.telemaco@gmail.com>
```

Congratulation!!! You've just installed AgileKip Generator on your environment.



    

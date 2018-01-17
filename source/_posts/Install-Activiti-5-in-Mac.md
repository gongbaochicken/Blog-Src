---
title: Install Activiti 5 in Mac
date: 2015-06-13 22:49:05
tags:
  - Java
  - Workflow
---
This is simple and clear tutorial about installing Aciviti 5, a workflow framework
that can be integrated with Java Spring.
<!--more-->

### Step 1. Download Activiti from its official website
Download URL:http://activiti.org/download.html

![](http://7xjjbh.com1.z0.glb.clouddn.com/Activiti.jpg)

Unzip the download package, and you will find several folders, including:

* database: All database script(MySql, H2, etc)
* docs: The help doc for Activiti users
* libs: Almost all jar packages you need
* wars: activiti-explorer.war + activiti-rest.war

### Step 2. Download and Configure Tomcat

Activiti needs a servlet container for Activiti-explorer.war, therefore we choose Apache-Tomcat.

Download Link: http://tomcat.apache.org/

Open bash, and set new path. For exapmle:

>export CATALINA_HOME=/Users/"YOUR Path"/apache-tomcat-7.0.62

>export PATH=/Users/"YOUR Path"/Documents/apache-tomcat-7.0.62/bin:$PATH

Don't forget make your configuration active
>source .bash_profile

### Step 3. Check if Tomcat can run the activiti frame

Please copy activiti-explorer.war and activiti-rest.war to webapps folder in your Tomcat root folder.

Change directory to the tomcat/bin, run:
> sh startup.bat

The script helps you to run the demo, and you can check it on
** http://localhost:8080/activiti-explorer/ **

If you successfully see the login page, then you are on the right track.
![Login](http://7xjjbh.com1.z0.glb.clouddn.com/Activiti Login.jpg)

The Activiti offers the following username/password to login:

|Username | Password | Role |
|------------ | ------------- | ------------|
|kermit | kermit  | admin |
|gonzo | gonzo  | manager |
|fozzie | fozzie | user |


### Step 4. Install Plugin on Eclipse(J2EE)
Open your eclipse(I use Eclipse Java EE IDE for Web Developers), Click Help -> Install New Software -> Add, and enter the following information:
```
Name: Activiti BPMN 2.0 designer
Location: http://activiti.org/designer/update/
```

This process will take a few minutes. No kidding. I mean, it is pretty slow. Please be patient. When install the plugin, eclipse may pop the following error information:
![Error](http://7xjjbh.com1.z0.glb.clouddn.com/Error.jpg)

In most cases, this is because you use incorrect version of the eclipse. According to the official document, Activiti BPMN 2.0 designer only supports Eclipse Indigo and Eclipse Juno.



** Author: Jason Zhuo Jia. Please mark the derivation if you want to copy this article in other places.**

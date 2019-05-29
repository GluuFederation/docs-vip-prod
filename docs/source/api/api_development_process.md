# Introduction

This document describes the process for developers to add new features to the oxTrust API.

The oxTrust API, as a VIP feature, does not ship by default with Gluu CE. The API code is hosted on oxTrust project under a module named **api-server**.

## Set up the working environment
 
 Oxtrust project is a Java and Maven based project, so any IDE that supports Java can be used. We recommend IntelliJ Idea or Eclipse.
 
 Your working environment must have the following tools installed:

1. Java version 8
1. Maven 3.3+
1. Git 
1. IDE (Eclipse, IntelliJ Idea, etc)
1. The latest version of the [Gluu Server](https://gluu.org/docs/ce)

## Adding new feature

 1. Clone the oxtrust project from the [repo](https://github.com/GluuFederation/oxTrust).
 1. Import the project in your IDE
 1. Create new web resource as require for the feature you are adding.
 The code must follow the same structure as the existing one. The API documentation is generated using Swagger 3.

## Write the test to validate the feature
 
 There is a test package where test for each feature is implemented, these tests are use to ensure that all features exposes trough API are working as expected. So each feature must be tested in that package.
 
## Build and deploy the new war file
 
### Build

1. Log in to the Gluu container
1. Navigate to **oxTrust** directory
1. Run `rm -rf server-test/target/* && mvn package -Dmaven.test.skip=true`
 The last command will build the new oxtrust war that contains the api code. That war will be use in next step to replace the existing one.
 
### Deploy
 
1. Log in to the Gluu container
1. Navigate to **/opt/gluu/jetty/identity/**
1. Edit the **start.ini** file and comment the line starting with **--module-forwarded**
1. Navigate to **/opt/gluu/jetty/identity/webapps/** 
1. Save a copy of the existing **identity.war** file
1. Copy the war file named **oxtrust.war** from **oxTrust/server/target/** folder to the current directory and rename it **identity.war**.
1. Restart the identity service with **systemctl restart identity**
1. Log in to Gluu Admin UI
1. Activate the oxTrust API test mode under the **JSON configuration** section
1. Use a tool like Postman to make API calls.

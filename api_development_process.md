# Introduction

This document describe the process to add new feature to oxtrust api and must be use as a guideline for developers willing to add new features.

Oxtrust Api as a VIP feature don't ship by default with Gluu CE.The Api code is hosted on oxTrust project under a module named **api-server**.

# Setup the working environment
 
 Oxtrust project is a java and maven based project hence every IDE that support Java can be used. We recommend IntelliJ Idea or Eclipse.
 
 Your working envrionnment must have the following tools installed:

1. Java version 8
1. Maven 3.3+
1. Git 
1. IDE(Eclipse, IntelliJ Idea, etc)
1. Latest Gluu server

# Adding new feature

 1. Clone the oxtrust project from the [repo](https://github.com/GluuFederation/oxTrust).
 1. Import the project in your IDE
 1. Create new web resource as require for the feature you are adding.
 The code must follow the same structure as the existing one. The API documentation is generated using swagger3.

# Write the test to validate the feature
 
 There is a test package where test for each feature is implemented, these tests are use to ensure that all features exposes trough API are workig as expected. So each feature must be tested in that package.
 
# Build and deploy the new war
 
 ## Build
 
 1. Move into **oxTrust** directory
 1. Run **rm -rf server-test/target/* && mvn package -Dmaven.test.skip=true**
 The last command will build the new oxtrust war that contains the api code. That war will be use in next step to replace the existing one.
 
 ## Deploy
 
 1. Login into gluu coontainer
 1. Move to **/opt/gluu/jetty/identity/**
 1. Edit the file name **start.ini** and comment the line starting with **--module-forwarded**
 1. Move to **/opt/gluu/jetty/identity/webapps/** 
 1. Save a copy of the existing file named **identity.war**
 1. Copy the war file named **oxtrust.war** from **oxTrust/server/target/** folder to the current directory and rename it **identity.war**.
 1. Restart identity service **systemctl restart identity**
 1. Login into Gluu Admin Ui
 1. Activate oxTrust Api test mode under **Json configuration** section
 1. Use a tool like postman to make a API calls.


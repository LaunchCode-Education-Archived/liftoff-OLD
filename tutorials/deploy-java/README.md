# Tutorial: Deploying an App

In this tutorial, we’ll walk through deploying a Spring Boot application to Pivotal Web Services using Cloud Foundry. Along the way, we’ll address other topics to consider when deploying an application, such as database hosting and migration, password management, and network routing.

## Requirements

This tutorial assumes you have the following installed

[Java 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
[Gradle](https://gradle.org/install/)
[MAMP](https://www.mamp.info/en/)
Cloud Foundry CLI (Below)

## Pivotal Web Services Setup

Pivotal Web Services will be the cloud hosting provider for this tutorial. There are many cloud hosts available to consider (Amazon, Azure, etc), but we’ll be using Pivotal because it has a lot of support for deploying Spring Boot applications and is relatively inexpensive.

Create a [Pivotal Account](https://pivotal.io/platform). You will need to provide a credit card number, and a phone number to verify. Later in this chapter, we’ll cover how to make sure everything is un-deployed so you don’t incur any nasty hosting fees.

Next, create a ‘space’ named “production” for our application to be deployed to. Developers typically use spaces to create segments to their environments, and may have one for production, development, testing, and so on.

## Cloud Foundry Setup

We’ll be using a tool called Cloud Foundry, widely used in the industry to manage cloud applications and services. It provides tools to package and deploy our code to production with minimal configuration.

Install the [Cloud Foundry CLI](https://pivotal.io/platform/pcf-tutorials/getting-started-with-pivotal-cloud-foundry/install-the-cf-cli). This tool will connect our computer to our Pivotal account, and allow us to deploy and manage our apps.

Log into your Pivotal account

`cf login`

You should your email, organization name, and the space you just created on the confirmation message.

## Preparing your project

This tutorial will be based off of the [cheese-mvc](https://github.com/LaunchCodeEducation/cheese-mvc) project. Clone this project and checkout the ‘video-one2many-end’ branch. Check this out as a new branch called ‘deployment’. If you’d like to keep the changes we’ll be making, first fork the project to your own repository, and continue based off the new git repository url.

`git clone https://github.com/LaunchCodeEducation/cheese-mvc.git`
`git checkout --track origin/video-one2many-end`
`git checkout -b develop`

If you haven’t done so, open up your phpMyAdmin page and create a new database user called `cheese-mvc` (and create the corresponding table). You should now be able to start the application using `gradle bootRun`.

## Recap on Java Deployables

Typically when we’re working on our projects, we’re building and running them locally. We have tools like Gradle and our IDEs that help us. We won’t have those tools in production, so we’ll need to package up our code into an executable JAR file.

Recall that Java is a compiled language, where the `javac` will take our source files and build an executable JAR file. When the JAR is executed, it also needs to be able to find all the dependencies (for instance, Spring) at runtime in order to use them.

Spring Boot adds a plugin to Gradle that allows us to package up both our source code, alongside any dependencies, creating a “Fat JAR”. This JAR file can then be deployed to our server, and run anywhere that has the JVM installed. Try building a fat JAR yourself by running `gradle assemble`. This created a fat JAR which we can run from the command line with the command `java -jar build/libs/cheese-mvc-0.0.1-SNAPSHOT.jar`

## Creating Your Manifest

In order to deploy an application with Cloud Foundry, you first must define what your application is, and how it should be run. Cloud Foundry uses manifest files to manage this configuration. Create a file at the root of the project named `manifest.yml`. And add the following lines.

```
applications:
- name: cheese-mvc
  buildpack: java_buildpack
  path: build/libs/cheese-mvc-0.0.1-SNAPSHOT.jar
```

The `name` will specify the unique name for our application, in this case `cheese-mvc`. The `buildpack` tells Cloud Foundry what type of application this is and how to manage it. The `path` is where to find our executable project, the fat JAR.

Let’s try and deploy our app using `cf push`

This will fail, but let’s try our hand with a little debugging.

`cf logs cheese-mvc --recent`

Now we can view the recent logs from our app (denoted by the `--recent` flag), see if the logs look familiar. You’ll see that our app can’t connect to the database, which makes sense, since we haven’t set up a database for our app.

## Configure Our App for a Cloud Database

If you had a project without any service dependencies (like a database), this would be all you need to deploy your application. In our case, we want to also deploy a database as well. Luckily, Pivotal provides an easy (and free) way to setup a MySQL database in our cloud environment.

First, we need to make some changes to our properties to support our changes. Update your `application.properties` so it matches the following.


```
# Hibernate ddl auto (create, create-drop, update, none)
# In this case, we'll want the default behavior to do nothing
spring.jpa.hibernate.ddl-auto = none

# Limit the number of active database connections
# Cloud Foundry's Spark databases can only provide up to four connections
spring.datasource.tomcat.max-active = 4
```


The first property we’ll change to `none`. Previously, we’ve let Hibernate manage our database, but this will be problematic in production. Soon, we’ll use another tool to help manage how our database is configured.

The second property is used to limit our active connections to our database. Typically, Spring Boot sets reasonable defaults to the number of active connections, but Pivotal’s Spark databases only allow four connections at a time.

## Flyway Database Migration

Previously we’ve used Hibernate to create and update our database tables. This is great for testing and allows us to add and change database schema on the fly. But, in the real world, we have to be careful to maintain our data in production, and be very intentional in the changes that we make to our database. Flyway is a tool that uses SQL scripts to create, change, and migrate database schemas and data. These SQL scripts will live alongside our project and will provide a way for us to easily recreate the structure of our database, as well as make additional changes in the future. Flyway tracks scripts that have been executed, and detects new scripts and runs them at startup.

First, add the following dependency to our `build.gradle` `dependencies` section. Spring Boot will detect this dependency, and automatically start using Flyway. We will also need to provide our application with SQL scripts for _how_ Flyway should manage our database.

Create a directory called ‘src/main/resources/db/migration’ and create a file named `V1__initialize.sql`. Flyway will load these alphabetically and apply our changes. Since Flyway keeps track of which migrations it’s performed previously, it is important to not change this file once deployed. Instead create a new file (i.e. V2__my_new_change.sql) to make additional changes.

The easiest way to create our initialization script is to export our existing cheese-mvc schema. Open the PhpMyAdmin Screen, select your database, and choose the export tab. Hit OK and copy the generated SQL and add it to our `V1__initialize.sql` file. This will include any test data you have already created, so you may want to clean up any `INSERT` statements if you’d like to start with a fresh slate.

An example `V1__initalize.sql` can be found here:


```
-- phpMyAdmin SQL Dump
-- version 4.6.5.2
-- https://www.phpmyadmin.net/
--
-- Host: localhost:8889
-- Generation Time: Oct 16, 2017 at 10:54 PM
-- Server version: 5.6.35
-- PHP Version: 7.0.15

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";

--
-- Database: `cheese-mvc`
--

-- --------------------------------------------------------

--
-- Table structure for table `category`
--

CREATE TABLE `category` (
  `id` int(11) NOT NULL,
  `name` varchar(15) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- Table structure for table `cheese`
--

CREATE TABLE `cheese` (
  `id` int(11) NOT NULL,
  `description` varchar(255) NOT NULL,
  `name` varchar(15) NOT NULL,
  `category_id` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- Indexes for table `category`
--
ALTER TABLE `category`
  ADD PRIMARY KEY (`id`);

--
-- Indexes for table `cheese`
--
ALTER TABLE `cheese`
  ADD PRIMARY KEY (`id`),
  ADD KEY `FK8tkjj8elvx1xu75dpuci756h8` (`category_id`);

--
-- AUTO_INCREMENT for table `category`
--
ALTER TABLE `category`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;
--
-- AUTO_INCREMENT for table `cheese`
--
ALTER TABLE `cheese`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;

--
-- Constraints for table `cheese`
--
ALTER TABLE `cheese`
  ADD CONSTRAINT `FK8tkjj8elvx1xu75dpuci756h8` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`);
```

## Creating a Database Service

Next, let’s configure a service inside Cloud Foundry for our database. Cloud Foundry considers anything that isn’t an application to be a service, and can provide pre-configured services to help speed up deployment. In this case, we want to create a MySQL database for our application. Pivotal has a database called ‘cleardb’, a flavor of MySQL designed for cloud hosting.  The ‘spark’ instance size is free. Creating a service in Cloud Foundry will also manage passwords and inject them into the application. This means that we do not need to check in our database credentials into version control (because we shouldn’t!).

Create a new service:

`cf create-service cleardb spark cheese-mvc-db`

Check running services:

`cf services`

You should see your newly created service displayed, but no application tied to it.

## Service Binding

Now you need to attach our application to your service. Cloud Foundry calls this ‘service binding’, and provides a connection between the two.

`cf bind-service cheese-mvc cheese-mvc-db`

We can confirm this has been configured by running `cf services` again, and we should see our app name has been added.

Once this has been established, it does not need to be re-bound when we redeploy.

## Adding a Service to the Manifest

Above, we manually configured our service binding for our application. Alternatively, the `manifest.yml` can bind services as well. Update your `manifest.yml` to the following:

```
applications:
- name: cheese-mvc
  buildpack: java_buildpack
  path: build/libs/cheese-mvc-0.0.1-SNAPSHOT.jar
  services:
    - cheese-mvc-db
```

## Deploying with our database

Now that there is a database service, and matching service binding, deploy your app again.

`cf push`

We should see a message that our “App started”. Open up the Pivotal Control panel https://console.run.pivotal.io/, select your space, and find your application. Click the ‘Route’ Tab to see where your application is available at. In my case, it was `https://cheese-mvc.cfapps.io`, but yours may be something else. Be sure to append /cheese onto the end of the url (i.e. `https://cheese-mvc.cfapps.io/cheese`, and try out your newly deployed app!

## Shut It Down

All good things must come to an end. Or, at the very least, will be billed hourly. When you’re not showing off your new application, it’s economical to undeploy your application.

To stop an application

`cf stop cheese-mvc`

And when you’re ready to use it again

`cf start cheese-mvc`

Since the spark instance of our database is free, we can leave it running. This way we can always have some test data in our application to demo. But, if you want to make sure you’ve removed everything (and losing all of your data in the process, you can remove the database service as well.

First, remove the service binding, tying the database to the application

`cf unbind-service cheese-mvc cheese-mvc-db`

Then, remove the database service
`cf delete-service cheese-mvc-db`

And then, you can delete your application entirely

`cf delete cheese-mvc`

## Scripting

Once you start making changes to your own applications you are going to want to be able to easily deploy your application. It may be helpful to build some helper scripts, that way you don’t have to remember all the right commands. Here is an example of a deployment script you can use:

deploy.sh
```
#!/usr/bin/env bash

# Clean up any old artifacts, and rebuild the jar
gradle clean assemble

# Create the database (will give a warning if already exists)
cf create-service cleardb spark cheese-mvc-db

# Deploy to cf
cf push
```
Mac and Linux users should be able to deploy the application via `sh ./deploy.sh`. Windows users, if you have git Bash installed, open a new bash prompt and do the same.

You can also script the destruction of your application as well

destroy.sh
```
#!/usr/bin/env bash

# Stops the running app
cf stop cheese-mvc

# Removes the binding between services
cf unbind-service cheese-mvc cheese-mvc-db

# Removes the db service
cf delete-service cheese-mvc-db -f

# Deletes the app entirely
cf delete cheese-mvc -f
```
And run with `sh ./destroy.sh`


## Further Learning

How do I keep a set of properties for local development to connect to my database? Using spring profiles, and two sets of property files, you can create properties for both local and deployment. https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-change-configuration-depending-on-the-environment

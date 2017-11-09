# Tutorial: Logging

In this tutorial we'll discuss why logging is important, how to control your logging output, how to write your own logging statements, and how to view your logs in production.

## Requirements

This tutorial assumes you have the following installed

[Java 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

[Gradle](https://gradle.org/install/)

## Why Logging Matters

* Provides observability of your app in a live environment

* Allow you to debug errors that have happened

* Using a logging framework provides a consistent, informative, timestamped output, and handles formatting and displaying stack traces. If you've used `System.out.println` to output information, you've noticed that these messages lack that extra context that the other messages have.

## Considerations

* Don't log sensitive information, such as passwords, PII (personally identifiable information), or credit card numbers.

* Too much logging is hard to sort through, not enough and it's not useful. We'll talk about a few techniques to manage the signal to noise ratio.

## Setup

Spring Boot by default ships with all the necessary libraries to add additional logging to your application. You will be interacting with [SLF4J](https://www.slf4j.org/), a logging abstraction which simplifies logger use. Under the hood, the logging implementation is provided by [Logback](https://logback.qos.ch/).

This tutorial will be based off of the [cheese-mvc](https://github.com/LaunchCodeEducation/cheese-mvc/tree/video-validation-end) project. Clone this project and checkout the ‘video-validation-end’ branch. Check this out as a new branch called `logging`. If you’d like to keep the changes we’ll be making, first fork the project to your own repository.

Feel free to use your own project for this tutorial, just make sure you update class and package names where relevant.

## Reading Log messages

You likely already have some experience reading log messages, but let's examine a typical Spring logged event.

Here's the first log message that displays immediately under the Spring Boot banner on startup:

`2017-11-05 13:33:58.594  INFO 73394 --- [  restartedMain] org.launchcode.CheeseMvcApplication      : Starting CheeseMvcApplication on LaunchCodeComputer with PID 73394 (/Users/launchcoder/workspace/cheese-mvc/build/classes/main started by launchcoder in /Users/launchcoder/workspace/cheese-mvc)`

Here's what's displayed by default:

* Date and Timestamp
* Log Level — ERROR, WARN, INFO, DEBUG or TRACE (discussed next)
* Process ID
* Thread name — Enclosed in square brackets (may be truncated for console output).
* Logger name — This is usually the source class name (often abbreviated).
* The log message - Here Spring is telling us what process ID our application started on, where it was started from, and by whom.

## Controlling Logging Output

Loggers typically supports five degrees of severity of logging, ranging from most severe `ERROR` to least severe `TRACE`. This gives you context the reason for the message, as well as helps you control which messages are displayed during runtime.

Logging levels allow you to control what information our application logs. You may want to only log `INFO` and above when your application is working normally, but you may want to have left some `DEBUG` level logs in order to troubleshoot a problem without redeploying your code. Logging frameworks allow you to control this via configuration files and properties.

In Spring Boot projects, you can use the `application.properties` to control which packages are logged at what level.

Spring boot does this by finding all properties prepended with `logging.level`, and will then take all the proceeding key value as the package to be logged.

If you want to log every event at a specific level use:

```
logging.level.root=DEBUG
```

If you want to log everything that Spring is doing, you'll set:

```
logging.level.org.springframework=DEBUG
```

Notice that we're logging `org.springframework`. This is the package name, and any class that emits a `DEBUG` log at, or below this directory will be displayed.

Say you want to log everything in your app, but ignore informational messages from Spring:

```
logging.level.org.springframework=WARN
logging.level.org.launchcode=TRACE
```

Here are some more common packages that may be useful to log during debugging:

```
# For debugging security
logging.level.org.springframework.security=DEBUG

# General debugging of web applications without getting ALL of Spring's logs
logging.level.org.springframework.web=DEBUG

# Database/ORM logging. Best used in conjunction:
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type=TRACE

# Consuming a REST apis via RestTemplate:
logging.level.org.springframework.web.client.RestTemplate = debug
logging.level.org.apache.http = debug
```

_Note: Some loggers also provide a `FATAL` level, but logback treats this the same as an `ERROR`_

## Adding a logger

As you start to build more complex applications and host them in a live environment, you'll want to start adding your own logging messages.

To do so, you'll need to instantiate (create) a logger inside the class we want to do some logging in. This is done by adding the following to the top of your class,  inside of your class declaration.

`private static final Logger logger = LoggerFactory.getLogger(CheeseController.class);`

Here, we're adding a logger to our `CheeseController`, but if you're working on your own project, be sure to use the class name as necessary.

This gives us access to a logging instance, which we can start using to create log messages.

## Creating a log statement

Consider a situation where you might want to log why a user failed validation. This way, if the user contacts us confused as to why they can't create a new cheese, register, or otherwise use our application, we'll have a record of what was the root problem. Add the following line inside one of our validation blocks, like so:

```
if (errors.hasErrors()) {
    logger.info("Error during cheese registration: " + errors.getAllErrors().toString());
    model.addAttribute("title", "Add Cheese");
    return "cheese/add";
}
```

In the event that a user has provided incorrect information when adding a cheese, we will log that there was an error, as well as the errors themselves, which should give us insight into what was the root of the problem.

## Passing Objects

Here we are just logging a string which we concatenate with the `+` symbol. Each logger provides many overloaded methods allowing you to pass in additional parameters, such as an object or an exception. Any objects that are passed in will be logged via their toString method. If it's an object you've created make sure you've overridden the default toString and you aren't logging any sensitive information stored in your object.

## Reading logs in Cloud Foundry

If you've deployed your app via Cloud Foundry, you can view your logs by using the `cf logs` command. For example, if your app was named 'cheese-mvc' you'd type `cf logs cheese-mvc`. This will display your logs in realtime. If you want to view recent logs, add the `--recent` flag like so `cf logs cheese-mvc --recent` and the most recent logs will be displayed.

## Reading logs in Pivotal Web Services

If you've hosted your app using Pivotal Web Services, you can also view your logs by logging into your [PWS console](console.run.pivotal.io/). Navigate to your space, then your application, and there should be a tab for "Logs". You should be able to view old logs, as well as monitor them in realtime.

## Additional Resources

* [Spring Boot Docs - Logging Features](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-logging.html)
* [Spring Boot Docs - Logging How To](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html)
* [Cloud Foundry Logging](https://docs.cloudfoundry.org/devguide/deploy-apps/streaming-logs.html)

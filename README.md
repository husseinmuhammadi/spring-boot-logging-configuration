# SPRING BOOT LOGBACK CONFIGURATION
This tutorial will demontrate logback configuration in spring boot projects step by step

## 1. EASY SETUP
If the only change you need to make to logging is to set the levels of various loggers then you can do that in application.properties using the "logging.level" prefix, e.g.
````
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=ERROR
````
You can also set the location of a file to log to (in addition to the console) using "logging.file".

## 2. Spring Boot Default Configuration
By default Spring Boot picks up the native configuration from its default location for the system (e.g. classpath:logback.xml for Logback), but you can set the location of the config file using the "logging.config" property.

If you put a logback.xml in the root of your classpath it will be picked up from there (or logback-spring.xml to take advantage of the templating features provided by Boot). Spring Boot provides a default base configuration that you can include if you just want to set levels, e.g.
````
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
    <logger name="org.springframework.web" level="DEBUG"/>
</configuration>
````

## 3. CONSOLE APPENDER

If you put a logback.xml in the root of your classpath it will be picked up from there (or logback-spring.xml to take advantage of the templating features provided by Boot).


## 4. FILE APPENDER
To configure logback for file only output you need a custom logback-spring.xml that imports file-appender.xml but not console-appender.xml:
````
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />
    <property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}/}spring.log}"/>
    <include resource="org/springframework/boot/logging/logback/file-appender.xml" />
    <root level="INFO">
        <appender-ref ref="FILE" />
    </root>
</configuration>
````
You also need to add logging.file to your application.properties:

````
logging.file.name=log/myapplication.log
````
Just like most Spring Boot properties, you can set these properties using environment variables instead, if you like.

Just convert the property to upper-case, and change dots to underscores. So you can use environment variables like this:

````
# to change the log file path
LOGGING_FILE_PATH=/path/to/mylogs
# to change the log filename
LOGGING_FILE_NAME=myapp.log
````

---

### LOGBACK PROPERTIES

````
<property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/spring.log}"/>
````

Like Bash, Logback uses `:-` as its default value operator. The line in question is setting the `LOG_FILE` property:

* If `LOG_FILE` is already set, use that
Otherwise, if `LOG_PATH` is set, use that suffixed with `spring.log`
* Otherwise, if `LOG_TEMP` is set, use that suffixed with `/spring.log`
* Otherwise, if `java.io.tmpdir` is set, use that suffixed with `/spring.log`
* Otherwise use `/tmp/spring.log`



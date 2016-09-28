# srcclr_gradle_training_lab

One of the most common ways developers introduce security issues in their applications is by using components with known vulnerabilities.  The reason this happens is that it is virtually impossible for a developer to manually keep track of each library that the teams uses and which versions of the library have known vulnerabilities.  

srcclr ([https://www.sourceclear.com/]( https://www.sourceclear.com/)) provides an excellent tool for identifying known vulnerabalities in your project's dependencies. 

This 30 minute lab will teach you how to setup srcclr in a project to automatically scan for vulnerabilities.

##Lab structure:
1. Set up an existing web application that was intentionally created with a known vulnerability.  You will pull down the application from Github, run it, and then start it to show it works.   
2. Add srcclr to the project’s build script.
3. Run the project with srcclr and see the vulnerability report
4. Fix the vulnerability 
5. Rerun srcclr to show the vulnerability has been fixed and the application works.

##Lab Requirements
#####To use this lab you will need
* 1. Git and Gradle running on your laptop. 
* 2. A srcclr account (signup on https://www.sourceclear.com)
* 3. An Auth Token
    * 3.1 ([Login](https://srcclr.com)) to SourceClear and select the team you want to setup the Gradle plugin in, and then select Agents > New Agent > Gradle (Under "Scan Automatically").
    * 3.2 Once you have entered the Gradle plugin setup page, click the "Create Authentication Token" button.
    * 3.3 Copy and paste the Auth Token someplace where you can easily retrieve it for later in this tutorial.


#Step 1) Set up a web application that has a known vulnerability

Make a new directory on your laptop for the lab and then navigate to it.
Clone this labs git repo.
```bash
git clone https://github.com/jtj/srcclr_gradle_training_lab
```
Navigate into the newly created directory.
```bash
cd srcclr_gradle_training_lab/
```

Run a clean build to make sure that everything is OK
```bash
./gradlew clean build
```

You should get a "BUILD SUCCESFULL" message.

Start the app to make ensure that you can run it
```bash
./gradlew build && java -jar build/libs/insecure-web-app-0.1.0.jar
```

A bunch (that’s the technical term) of text will scroll by as the webserver is started. When it stops, open a browser and go to http://localhost:8080/
You should see a success message that says "Greetings from srcclr!"

> Tip: Control-c will stop the webserver on a mac.

**Congratulations! You have set up an insecure web application on your laptop! Now let’s find out how to track down the vulnerability in your application and fix it.**

#Step 2) Setup the project to use srcclr

Using the IDE of your choice, open up the build.gradle file in the project’s home directory.
Add the following sections of code into the build.gradle file directly below the buildscript block.
```gradle
plugins {
    id "com.srcclr.gradle" version "2.0.2"
}
```

Next add the srcclr plugin to the end of the plugins list. Append the following line:
```gradle 
apply plugin: 'srcclr'
```

That portion of the build.gradle file should now look like this
```gradle
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'spring-boot'
apply plugin: 'srcclr'
```

Finally, add the api token for this application to the bottom of the build.gradle file.
```gradle
srcclr{
  apiToken = "<YOUR AUTH TOKEN HERE>"
}
```

Save the gradle.build file and you are ready to try srcclr out.


#Step 3) Run the project with srcclr and see the vulnerability report
Now that we have our dependancy vulnerabality scanning tool hooked up into our application, lets use it to generate a report. 
Make sure that you have saved the build.gradle file and run the following command
```bash
./gradlew clean build srcclr
```

In the output of the build you will see a block of text with the srcclr results.
It should look something like this:
```bash
Libraries:
62 libraries, 38 lines of code
3 direct and 59 transitive
3 vulnerable libraries
9 different licenses are used
1 library is GPL-licensed

Security Issues:
0 high [H] risk vulnerabilities
2 medium [M] risk vulnerabilities affecting 2 libraries
1 low [L] risk vulnerability affecting 1 library

- M SID-2375  SpEL Injection Attacks [Spring Boot AutoConfigure 1.2.7.RELEASE]
- M SID-2403  Leakage of sensitive properties [Spring Boot Actuator 1.2.7.RELEASE]
- L SID-1600  Information Disclosure of Previous Requests [Jetty :: Http Utility 9.2.13.v20150730]

Report:
https://app.sourceclear.com/teams/N22UNW/--a custom link for you. It wil look different for each person. 
:srcclr
```

The version of Spring Boot has two medium vulnerabilities and the version of the Jetty Http Utility library has one low risk vulnerabilities.

#Step 4) Fix the vulnerability 

Now that we know where our problems are, we can fix them. We will do this by upgrading Spring Boot from 1.2.7 to 1.4.0 in our build.gradle file.
Update the Spring Boot library by modifying this line of code:
```gradle
classpath("org.springframework.boot:spring-boot-gradle-plugin:1.2.7.RELEASE")
```
To:
```gradle
classpath("org.springframework.boot:spring-boot-gradle-plugin:1.4.0.RELEASE")
```
Save the build.gradle file.

#Step 5) Run srcclr again to see the results of updating the dependency
Run:
```bash
./gradlew clean build srcclr
```

The report in the output should look like this:
```bash

Libraries:
53 libraries, 38 lines of code
3 direct and 50 transitive
0 vulnerable libraries
8 different licenses are used
1 library is GPL-licensed

Security Issues:
0 high [H] risk vulnerabilities
0 medium [M] risk vulnerabilities
0 low [L] risk vulnerabilities


Report:
https://app.sourceclear.com/teams/N22UNW/--a custom link for you. It wil look different for each person.
:srcclr

```

All the dependency vulnerabilities are gone, not just the Spring Boot ones!

The Jetty Http Utility library was a dependency for Spring Boot 1.2.7, when you updated Spring Boot the Jetty lib was also updated. When hunting down dependency vulnerabilities, it’s recommended that replace re-run srcclr after updating each library.

After updating the library we need to run the application to make sure that it still works.
Run: 
```bash
./gradlew build && java -jar build/libs/insecure-web-app-0.1.0.jar
```
When the application starts, open a browser and go to http://localhost:8080/
You should see a success message that says "Greetings from srcclr!"

**Congratulations! You have learned how to setup srcclr to scan your application for dependency vulnerabilities.**  

> More information on how to use srcclr can be found here: https://srcclr.help/docs

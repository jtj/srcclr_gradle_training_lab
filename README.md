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
1. Git and Gradle running on your laptop. 
2. A srcclr account (signup on https://www.sourceclear.com)
3. An Auth Token (NEED LINK TO INSTRUCTIONS HERE.)


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

# srcclr_gradle_training_lab
Learn to use SourceClear (srcclr) to analyze your code for dependency vulnerabilities.

One of the most common ways teams introduce security issues in their applications is by using components with known vulnerabilities.  The reason this happens is that it is virtually impossible for a developer to manually keep track of each library that the teams uses and which versions of the library have known vulnerabilities.  

srcclr ( https://www.sourceclear.com/ ) provides an excellent tool for identifying known vulnerabalities in your project's dependencies. 

This 30 minute lab will teach you how to setup srcclr in a project to automatically scan for vulnerabilities.

##Lab structure:
1. Set up an existing web application that was intentionally created with a known vulnerability.  You will pull down the application from Github, run it, and then start it to show it works.   
2. Add srcclr to the project’s build script.
3. Run the project with srcclr and see the vulnerability report
4. Fix the vulnerability 
5. Rerun srcclr to show the vulnerability has been fixed and the application works.

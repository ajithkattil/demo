# Hello Node
This is a very basic Hello World application written with Node.

It includes a `Dockerfile` for building a Docker image with the application, and a `Jenkinsfile` that defines a build pipeline for it.

A) Problem statement:

Setup a coded pipeline using Groovy script on Jenkins to demonstrate the following:
Connect to any public GitHub repository that you can build locally.
Your job should have a build stage and deploy stage.
Demonstrate the use of multi-branch pipeline plugin
Build and deploy the application on a Docker container and show it functioning on the container
Show conditional promotion in the pipeline from build stage to deploy stage i.e. if the build fails, it should just send a notification to a slack channel about the error.

Jenkins URL :http://192.168.1.141:8080
Multibranch Pipeline Job : http://192.168.1.141:8080/job/demo/
Source code : https://github.com/ajithkattil/demo
Slack notifications sent to slack URL: demogroup-global.slack.com  and channel #jenkins-notifications

Install and configure  Jenkins + plugins , Docker, Groovy , Slack in Mac, Open a github account 
Created a freestyle job " http://localhost:8080/job/freestyledockerisation-1/” . This checks out code , package and create docker image, then create a container .   


Issues:
1. Docker build and run command working from Mac laptop, but not in jenkins. 
Solution:Added full path to docker command in exec shell
2. Jenkins user does not have permissions to run Docker
chmod -R g=rwx Group Containers
chmod -R g=rwx "Group Containers”
ln -f -s /Applications/Docker.app/Contents/Resources/bin/* /usr/local/bin
3. Jenkins user got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: 
No other way but to change Jenkins default user from “Jenkins ‘ to “root”
sudo vi /Library/LaunchDaemons/org.jenkins-ci.plist
sudo vi /Library/LaunchDaemons/org.jenkins-ci.plis
sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist
Changed the username from jenkins to root, Docker is good from jenkins now


 Docker run , builds were failing need to delete previous container
needs to name the container during run, and as a prestep make sure this container does not exist

Created a mutibranch job for a maven project for testing.http://localhost:8080/job/multibranchpipeline-2/
Integrated slack and jenkins. Some issues with plugin. Reverted back to ver 1.7 , then back to latest version
Created the demo job with multibranch plugin  and tested . Except slack notification , everything works now. “Slacksend” method is not working 

After configuring jenkins CI in slack , go to Jenkins manage ->Global Slack Notifier  Settings   Base URL should be left blank. Now test is a success
Created a job for testing slack: http://localhost:8080/job/slacknotification-3. Made this work after adding slack in postbuild ,along with the postbuild checks configured earlier ( to be configured twice in the job)
Slack notification works now !!


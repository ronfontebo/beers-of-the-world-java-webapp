# beers-of-the-world
A java web app for listing various kinds of beer.

Steps for deploying this project:
=================================
1. Fork this github repository into your github account 
2. Create a github webhook in the project repository that will be used by the jenkins server to pull the source code
3. Create a slack channel for the project and setup slack incoming webhook to recieve notifications on the progress of your project
4. Setup a jenkins ci/cd pipeline with the following components:
   - A Jenkins master server configured to use the github webhook of your project repository to pull the source code from the git repository
   - A maven slave agent to compile, test and build the artifact 
   - A sonarqube slave agent to run code quality analysis 
   - Notify slack channel on build status
   - A nexus server to upload the build artifact
   - A tomcat server to deploy the build artifact
   - Notify slack channel on deployment status
5. Setup logging and monitoring of the application server using prometheus and grafana
6. Setup regular notifications to project slack channel on the application server's health 


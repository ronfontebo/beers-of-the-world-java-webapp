# beers-of-the-world
A java web app for listing all kinds of beer

Steps for deploying this project:
=================================
1. Fork this github repository into your github account 
2. Create a github webhook in the project repository 
3. Create a slack channel for project and setup slack incoming webhook to recieve notications on deployment progress
4. Setup a jenkins ci/cd pipeline with the following components:
   - A Jenkins master server configured to use the github webhook to pull the source code from the git repository
   - A maven slave agent to compile, test and build the artifact 
   - A sonarqube slave agent to run code quality analysis 
   - A nexus server to upload the build artifact
   - A tomcat server to deploy the build artifact
   - Notify slack channel on build status
5. Setup logging and monitoring of the application server using prometheus and grafana


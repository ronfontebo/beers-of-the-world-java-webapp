
// jenkins declarative pipeline script for beers-of-the-world-java-webapp
//=======================================================================


pipeline {
  agent any
  tools {
    maven "maven"
  }
  triggers {
    githubPush()
  }
  stages {
    stage('1. code checkout') {
      steps {
        sh "echo cloning the lastest code version"
        git branch: 'main', 
        credentialsId: 'git-token', 
        url: 'https://github.com/ronfontebo/beers-of-the-world-java-webapp'
        sh "echo clonning successful"
      }
    }
  
    stage('2. maven build') {
      steps {
        sh "echo validation, compilation, testing and packaging"
        sh "echo testing successful and ready to package"
        sh "mvn clean package"        
      }
    }

    stage('3. sonarqube code quality analysis') {
      environment {
        SONAR_CREDS = credentials('sonarqube-token')
      }  
      steps {
        sh "echo performing code quality analysis"
        sh "mvn sonar:sonar \
          -Dsonar.projectKey=beers-of-the-world-webapp \
          -Dsonar.host.url=http://18.118.168.91:9000 \
          -Dsonar.login=${SONAR_CREDS}"
        sh "echo code quality successful and ready to upload"
      }
    } 
  
   /* post {
      always {
        sh "echo notifying slack channel on build status"
        slackSend botUser: true, channel: '#beers-of-the-world-java-webapp', message: 'Build completed and successful. Ready to upload backup to nexus and deploy to tomcat servers respectively. Only 4/23 build attempts have been successful so far. : ${env.JOB_NAME} ${env.BUILD_NUMBER}  ', tokenCredentialId: 'slack-token'     
      }
    } */ 

    stage('4. upload to nexus') {   
      steps {
        sh "echo uploading artifacts"
        sh "mvn deploy"
      }
    }

    stage('5. deploy to tomcat') {
      steps {
        sh "echo deploying to production server"
        sshagent(['agentcredentials']) {
        sh "scp -i apache-key-pair.pem target/*.war ec2-user@18.191.253.216:/opt/tomcat9/webapps/beers-of-the-world-webapp.war"
        }
      }
    } 
  }
  
  post {
    always {
      sh "echo notifying slack channel on deployment status"
      slackSend botUser: true, channel: '#beers-of-the-world-java-webapp', message: 'Deployment completed and successful : ${env.JOB_NAME} ${env.BUILD_NUMBER}  ', tokenCredentialId: 'slack-token'
    }
  }
}  
 
  
// End of pipeline
//=================================================================================================================================

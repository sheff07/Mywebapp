pipeline {
  agent any
 
  tools {
    maven 'Maven3'
  }
  
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean install -f MyWebApp/pom.xml'
      }
    }
    // stage ('Code Quality') {
    //   steps {
    //     withSonarQubeEnv('SonarQube') {
    //       sh 'mvn -f MyWebApp/pom.xml sonar:sonar'
    //     }
    //   }
    // }
    stage ('JaCoCo') {
      steps {
        sh 'mvn jacoco:prepare-agent test jacoco:report'
      }
    }
    stage ('DEV Tomcat Deploy') {
      steps {
        echo "deploying to DEV Env "
        deploy adapters: [tomcat9(credentialsId: '17570684-9bc3-4f63-b0c4-b73b74703882', url: 'http://ec2-44-211-170-192.compute-1.amazonaws.com:8080/', path: '/MyWebApp')], war: '**/*.war'
      }
    }
    // stage ('Slack Notification') {
    //   steps {
    //     echo "deployed to DEV Env successfully"
    //     slackSend(channel:'your slack channel_name', message: "Job is successful, here is the info - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    //   }
    // }
    // stage ('DEV Approve') {
    //   steps {
    //     echo "Taking approval from DEV Manager for QA Deployment"
    //     timeout(time: 7, unit: 'DAYS') {
    //       input message: 'Do you want to deploy?', submitter: 'admin'
    //     }
    //   }
    // }
  }
  
  post {
    always {
      // Clean up workspace
      cleanWs()
    }
    success {
      // Notify success (you can add email or Slack notifications here)
      echo "Build and deployment successful."
    }
    failure {
      // Notify failure
      echo "Build or deployment failed."
    }
  }
}

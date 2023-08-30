def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
    'UNSTABLE': 'danger'
]
pipeline {
  agent {
    label 'Gradle-Build-Env' // Use the Gradle slave node for this pipeline
  }
  stages {
    stage('Validate Project') {
        steps {
            sh 'gradle check'
        }
    }
    stage('Gradle Project Tasks'){
        steps {
            sh 'gradle tasks'
        }
    }
    stage('Unit & Integration Test'){
        steps {
            sh 'gradle test'
        }
    }
    stage('Package Application'){
        steps {
            sh 'gradle build'
        }
    }
    stage ('Checkstyle Code Analysis'){
        steps {
            sh 'gradle checkstyleTest'
        }
    }
    stage('SonarQube Inspection') {
        steps {
            sh 'gradle sonarqube'
        }
    }
    stage("Upload Artifact To Nexus"){
        steps{
            sh 'gradle publish'
        }
        post {
            success {
                echo 'Successfully Uploaded Artifact to Nexus Artifactory'
        }
      }
    }
  }
  post {
    always {
        echo 'Slack Notifications.'
        slackSend channel: '#oromidayo-jenkins-master-client-alerts', //update and provide your channel name
        color: COLOR_MAP[currentBuild.currentResult],
        message: "*${currentBuild.currentResult}:* Job Name '${env.JOB_NAME}' build ${env.BUILD_NUMBER} \n Build Timestamp: ${env.BUILD_TIMESTAMP} \n Project Workspace: ${env.WORKSPACE} \n More info at: ${env.BUILD_URL}"
    }
  }
}


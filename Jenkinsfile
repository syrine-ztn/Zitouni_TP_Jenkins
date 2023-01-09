pipeline {
  agent any
  stages {
    stage('build') {
      post {
        failure {
          script {
            mail= " Build termine avec échec "
          }

        }

        success {
          script {
            mail=" Build termine avec succes "
          }

        }

      }
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'

        junit(testResults: 'build/test-results/test/*.xml', skipPublishingChecks: true, allowEmptyResults: true)
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'TP Jenkins notification', body: mail, cc: 'js_zitouni@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat(script: 'gradle sonarqube', returnStatus: true)
            }

            waitForQualityGate true
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber 'reports/*json'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        bat 'gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services', channel: '#my_channel', token: 'TSGEF9MGR/B04EKU8G31A/1s4jywo6tECegVgcgnVM5qIz', message: 'notification for JenkinsOGL has been successfully sent to slack')
      }
    }

  }

}

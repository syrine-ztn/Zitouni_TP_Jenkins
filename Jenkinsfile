pipeline {
  agent any
  stages {
    
    stage('build') {
      post {
        failure {
          script {
            mail= "Pipeline termine avec échec "
          }
        }

      }
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'

        junit(testResults: 'build/test-results/test/*.xml', skipPublishingChecks: true, allowEmptyResults: true)
        mail(subject: 'Pipeline Notifications', body: mail, cc: 'js_zitouni@esi.dz')
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'notification', body: 'mail', cc: 'js_zitouni@esi.dz')
        mail(subject: 'Pipeline Notifications', body: mail, cc: 'js_zitouni@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat(script: 'gradle sonarqube', returnStatus: true)
              mail(subject: 'Pipeline Notifications', body: mail, cc: 'js_zitouni@esi.dz')
            }

            waitForQualityGate true
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber 'reports/*json'
            mail(subject: 'Pipeline Notifications', body: mail, cc: 'js_zitouni@esi.dz')
          }
        }

      }
    }

    stage('Deploy') {
       
      steps {
        bat 'gradle publish'
        notifyEvents message:'Déploiement avec succès', token: 'HOEOr4tfxF2jQnspzWrTo5hk1rLy1yVZ'
        mail(subject: 'Pipeline Notifications', body: mail, cc: 'js_zitouni@esi.dz')
        mail(subject: 'Déploiement notifications', body: 'Déploiement avec succès', cc: 'js_zitouni@esi.dz')
      }
    }

    stage('Notification') {
     
      steps {
        mail(subject: 'Pipeline Notifications', body: mail, cc: 'js_zitouni@esi.dz')
      }
    }
             
  }
environment {
    mail = ''
  }
}

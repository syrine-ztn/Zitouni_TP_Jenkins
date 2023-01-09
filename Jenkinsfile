pipeline {
  agent any
  stages {
    
    stage('build') 
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
       post {
        success {
          script {
            mail="Déploiement avec succès"
            notifyEvents message:'Déploiement avec succès', token: 'HOEOr4tfxF2jQnspzWrTo5hk1rLy1yVZ'
          }
        }
      }
      steps {
        bat 'gradle publish'
      }
    }

    stage('Notification') {
      

      steps {
        mail(subject: 'Mail Notifications', body: 'mail', cc: 'js_zitouni@esi.dz', to: 'js_zitouni@esi.dz'
      }
    }
             
  }

}

pipeline {
  agent any
  stages {
    
    stage('test') {
      post {
        failure {
          script {
            mail= "Pipeline termine avec échec "
          }

        }

        success {
          script {
            mail="Pipeline termine avec succes "
          }

        }

      }
      steps {
        bat 'gradle test'
        junit(testResults: 'build/test-results/test/*.xml', skipPublishingChecks: true, allowEmptyResults: true)
        cucumber 'target/*.json'
      }
    }

    stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat(script: 'gradle sonarqube', returnStatus: true)
            }
        }
    }
    
    stage('Code Quality') {
          steps {
            withSonarQubeEnv('sonar') {
              waitForQualityGate true
            }
        }
    }
    
    stage('build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
      }
    }


    stage('Deploy') {
       
      steps {
        bat 'gradle publish'
        notifyEvents message:'Déploiement avec succès', token: 'HOEOr4tfxF2jQnspzWrTo5hk1rLy1yVZ'
        mail(subject: 'Déploiement notifications', body: 'Déploiement avec succès', cc: 'js_zitouni@esi.dz',from:'js_zitouni@esi.dz',to:'js_zitouni@esi.dz')
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

pipeline {
  agent any
  stages {
    stage('CheckOut SCM') {
      steps {
        echo 'Building Mule Project with Maven POM file'
      }
    }
    stage('CodeAnalysis') {
      steps {
         echo 'SonarQube-Static Code Analysis'
         bat(script: 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.sources=. -Dsonar.projectKey=mulecicdsamplekey:master', returnStatus: true)         
         waitForQualityGate()
      }
    }
    stage('Build') {
        steps {
          echo 'Build code with Maven'
       }
    }
    stage('Run Tests') {
        parallel {
          stage('Service') {
            steps {
              echo 'Run Tests'
            }
          }
          stage('Unit') {
            steps {
              echo 'Munit Unit Testing'
              bat 'mvn test'
            }
          }
          stage('UI') {
            steps {
              echo 'Running UI Tests'
            }
          }
        }
      }
      stage('Archive Artifacts') {
        steps {
          echo 'Archiving the Artifacts into JFrog Artifactory'
        }
      }
      stage('Package&Deploy') {
        steps {
          echo 'Deploy Mule app to CloudHub'
        }
      }
    }
  }
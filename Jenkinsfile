pipeline {
  agent any
  stages {
    stage('CheckOut SCM') {
      steps {
        echo 'Building Mule Project with Maven POM file'
      }
    }
    stage('Build') {
      steps {
        echo 'Build code with Maven'
        bat 'mvn compile'
        echo 'echo ${POM_ARTIFACTID} ${env.JOB_NAME} ${env.POM_GROUPID}'
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
    stage('CodeAnalysis') {
      steps {
        echo 'SonarQube-Static Code Analysis'
        bat(script: 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=admin -Dsonar.password=admin -Dsonar.sources=. -Dsonar.projectKey=mulecicdsamplekey:master', returnStatus: true)
      }
    }
    stage('Package') {
      steps {
        echo 'Package the application'
        bat 'mvn package'
      }
    }
    stage('Archive Artifacts') {
      steps {
        archiveArtifacts 'target/*.zip'
      }
    }
    stage('Deploy') {
      steps {
        bat 'mvn package mule:deploy'
        echo 'Deploy Mule app'
      }
    }
  }
}
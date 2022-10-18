pipeline {
  agent any

  tools {
    maven "maven"
  }
  stages {
    stage('checkout') {
      steps {

        git branch: 'main', url: 'https://github.com/ShraddhaGithub123/demo.git'
        git pull origin main

      }
    }
    stage('Execute Maven') {
      steps {

        sh 'mvn package'
      }
    }

    stage('SonarQube analysis') {
      //    def scannerHome = tool 'SonarScanner 4.0';
      steps {
        withSonarQubeEnv('sonarqube-8.9.9') {
          // If you have configured more than one global server connection, you can specify its name
          // sh "${scannerHome}/bin/sonar-scanner"
          sh "mvn sonar:sonar"
        }
      }
    }

    stage('Docker Build and Tag') {
      steps {

        sh 'docker build -t samplewebapp:latest .'
        sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:latest'
        //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'

      }
    }

    stage('Publish image to Docker Hub') {

      steps {
        withDockerRegistry([credentialsId: "dockerHub", url: ""]) {
          sh 'docker push nikhilnidhi/samplewebapp:latest'
          //  sh  'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER' 
        }

      }
    }

    stage('Run Docker container on Jenkins Agent') {

      steps {
        sh "docker run -d -p 8003:8080 nikhilnidhi/samplewebapp"

      }
    }
    stage('Run Docker container on remote hosts') {

      steps {
        sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 nikhilnidhi/samplewebapp"

      }
    }
  }
}

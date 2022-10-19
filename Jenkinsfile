pipeline {
  agent any

  tools {
    maven "maven"
  }
  stages {
    stage('checkout') {
      steps {

        git branch: 'master', url: 'https://github.com/ShraddhaGithub123/demo.git'

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
        withSonarQubeEnv('sonarqube') {
          // If you have configured more than one global server connection, you can specify its name
          // sh "${scannerHome}/bin/sonar-scanner"
          sh "mvn sonar:sonar"
        }
      }
    }

    stage('Docker Build and Tag') {
      steps {

        sh 'docker build -t demo:latest .'
        sh 'docker tag demo shraddhagaikwad52/demo:latest'
        //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'

      }
    }

    stage('Publish image to Docker Hub') {

      steps {
        withDockerRegistry([credentialsId: "dockerHub", url: ""]) {
          sh 'docker push shraddhagaikwad52/demo:latest'
          //  sh  'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER' 
        }

      }
    }

    stage('Run Docker container on Jenkins Agent') {

      steps {
        sh "docker run -d -p 8003:8080 shraddhagaikwad52/demo"

      }
    }
    stage('Run Docker container on remote hosts') {

      steps {
        sh "docker -H ssh://ec2-user@ec2-18-181-206-78.ap-northeast-1.compute.amazonaws.com run -d -p 8003:8080 shraddhagaikwad52/demo"

      }
    }
  }
}

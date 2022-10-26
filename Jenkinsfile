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

        sh 'mvn deploy'
      }
    }

    /*stage('SonarQube analysis') {
      //    def scannerHome = tool 'SonarScanner 4.0';
      steps {
        withSonarQubeEnv('sonar') {
          // If you have configured more than one global server connection, you can specify its name
          // sh "${scannerHome}/bin/sonar-scanner"
          sh "mvn sonar:sonar"
        }
      }
    }*/
    /*stage("Upload Artifactory to Nexus") {
      steps {
        script {
          def mavenPom = readMavenPom file: 'pom.xml'
          nexusArtifactUploader(
            nexusVersion: 'nexus3',
            protocol: "http",
            nexusUrl: "18.183.190.99:8081",
            groupId: "com.devops4solutions",
            version: "1",
            repository: "demo",
            credentialsId: "admin", //nexus3 //shubhnexus
            artifacts: [
              [artifactId: 'LoginWebApp',
                classifier: '',
                file: "target/LoginWebApp-1.war",
                type: "war"
              ]
            ]
          )
        }
      }
  }*/
    stage('upload artifact ') {
      steps {
        // nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: 'jar', file: 'target/my-app-1.0.0.jar', type: '']], credentialsId: 'nexuscred', groupId: 'com.mycompany.app', nexusUrl: '18.183.254.123:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'demoapp', version: '1.0.0'
        nexusArtifactUploader artifacts: [
          [artifactId: 'LoginWebApp', classifier: '', file: 'target/LoginWebApp-1.war', type: 'war']
        ], credentialsId: 'admin', groupId: 'com.devops4solutions', nexusUrl: '18.183.190.99:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'demo', version: '1'
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

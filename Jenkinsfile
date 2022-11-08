pipeline {
  agent any

  tools {
    maven "maven"
  }
  stages {
    stage('checkout') {
      steps {

        git branch: 'master', url: 'https://github.com/ShraddhaGithub123/demo2.git'

      }
    }
    stage('Execute Maven') {
      steps {

        sh 'mvn clean install'
      }
    }

    /*stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('sonar') {
          sh "mvn sonar:sonar"
          //sh "org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
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
            nexusUrl: "13.234.37.215:8081",
            groupId: "com.devops4solutions",
            version: "1",
            repository: "demo",
            credentialsId: "nexus3", //nexus3 //shubhnexus
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

    /*stage('Ansible Tomcat Deployment') {
      agent {
        label 'ans'
      }
      steps {
        ansiblePlaybook credentialsId: 'ubuntu',
          disableHostKeyChecking: true,
          installation: 'ansible',
          inventory: 'hosts',
          playbook: 'main.yaml'
      }
    }*/
    /*stage('upload artifact ') {
      steps {
        // nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: 'jar', file: 'target/my-app-1.0.0.jar', type: '']], credentialsId: 'nexuscred', groupId: 'com.mycompany.app', nexusUrl: '18.183.254.123:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'demoapp', version: '1.0.0'
        nexusArtifactUploader artifacts: [
          [artifactId: 'LoginWebApp', classifier: '', file: 'target/LoginWebApp-1.war', type: 'war']
        ], credentialsId: 'admin', groupId: 'com.devops4solutions', nexusUrl: '18.183.190.99:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'demo', version: '1'
      }
    }*/
    stage('Docker Build and Tag') {
      steps {

        sh 'docker build -t demo:temp .'
        sh 'docker tag demo shraddhagaikwad52/demo2:temp'

      }
    }

    stage('Publish image to Docker Hub') {

      steps {
        withDockerRegistry([credentialsId: "dockerHub", url: ""]) {
          sh 'docker push shraddhagaikwad52/demo2:temp'
        }

      }
    }

    stage('Run Docker container on Jenkins Agent') {

      steps {
        sh "docker run -d -p 8003:8080 shraddhagaikwad52/demo2"

      }
    }
  }
}

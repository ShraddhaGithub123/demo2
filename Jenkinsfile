pipeline {
   environment {
        def ARTID = 'LoginWebApp'
        def ARTTYPE = 'war'
        def ARTGRPID = 'com.devops4solutionse'
        def ARTURL = '13.234.37.215:8081'  //change ip
        def ARTREPO = 'demo'
        def ARTVER = '1'
        def ARTPROTO = 'http'
        DOCKER_TAG = getVersion()
        //def ARTDOWPATH = '$ARTPROTO://$ARTURL/repository/$ARTREPO' //http://3.108
   }
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
            protocol: "${env.ARTPROTO}",
            nexusUrl: "${env.ARTURL}",
            groupId: "${mavenPom.groupId}",
            version: "${mavenPom.version}",
            repository: "${env.ARTREPO}",
            credentialsId: "nexus3", //nexus3 //shubhnexus
            artifacts: [
              [artifactId: 'LoginWebApp',
                classifier: '',
                file: "target/LoginWebApp-${mavenPom.version}.${mavenPom.packaging}",
                type: "${mavenPom.packaging}"
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

        //sh 'docker build -t demo:temp .'
        //sh 'docker tag demo shraddhagaikwad52/demo2:temp'
        sh "docker build . -t shraddhagaikwad52/demo2:${DOCKER_TAG} "
  
      }
    }

    stage('Publish image to Docker Hub') {

      steps {
        withDockerRegistry([credentialsId: "dockerHub", url: ""]) {
          sh 'docker push shraddhagaikwad52/demo2:${DOCKER_TAG}'
        }

      }
    }

    stage('Run Docker container on Jenkins Agent') {

      steps {
        sh "docker run -d -p 8003:8080 shraddhagaikwad52/demo2:${DOCKER_TAG}"

      }
    }
  }
}
def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}

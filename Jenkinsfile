pipeline {
    environment {
    registry = "arnabnath96/python-jenkins"
    registryCredential = 'dockerhub'
  }
    agent any
    tools{
        dockerTool "docker"
    }
    stages {
        
        stage('VCS') {
            steps {
                git 'https://github.com/arnabnath96/hello-world-webapp'
            }
        }
         stage('Deploy') {
            steps {
                script {
                     withDockerRegistry(credentialsId: 'jenkinstest', toolName: 'docker', url: 'https://hub.docker.com') {
                        
                     def echoServerImage = docker.build("arnabnath96/jenkins-webserver:latest");
                     echoServerImage.push();
                    }
                }
            }
        }    
    }
    
    post {
        success {
            echo "Success"
        }
        failure {
            echo "Failure"
        }
    }
}

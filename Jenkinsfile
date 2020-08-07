pipeline {
    
    agent any
    tools{
        dockerTool "docker1"
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
                     withDockerRegistry(credentialsId: 'jenkinstest', toolName: 'docker1'){
                        
                     def echoServerImage = docker.build("arnabnath96/jenkins-webserver:latest");
                     echoServerImage.push();
                    }
                }
            }
        }    
    }
    
    post {
        success {
            script { 
                def payload =  """{"text": "Project Name: hello-world-webapp\nBuild Commit: ${GIT_COMMIT} \nBuild Status: Success"}"""
                httpRequest url: "https://api.flock.com/hooks/sendMessage/ee50a7ee-8c2f-44c8-a4d2-47d4572df7b7",  httpMode: 'POST', requestBody: payload
       
            }
        }
        failure {
            script {    
                def payload =  """{"text": "Project Name: hello-world-webapp\nBuild Commit: ${GIT_COMMIT} \nBuild Status: Failure"}"""
                httpRequest url: "https://api.flock.com/hooks/sendMessage/ee50a7ee-8c2f-44c8-a4d2-47d4572df7b7",  httpMode: 'POST', requestBody: payload
        
            }
        }
    }
}

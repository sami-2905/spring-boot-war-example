pipeline {
    agent any
     tools {
        maven 'maven' #name you have given to maven in global too configuration 
        }
    stages {
        stage("Test"){
            steps{
                // mvn test
                slackSend channel: 'jenkins-pipeline', message: 'Job Started'
                sh "mvn test"
                
                
            }
            
        }
        stage("Build"){
            steps{
                sh "mvn package"
                
            }
            
        }
        stage("Deploy on Test"){
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'test-server', path: '', url: 'http://172.31.4.63:8080')], contextPath: '/app', war: '**/*.war'
              
            }
            
        }
        stage("Deploy on Prod"){
             input {
                message "Should we continue?"
                ok "Yes we Should"
            }
            
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'prod-server', path: '', url: 'http://172.31.4.149:8080')], contextPath: '/app', war: '**/*.war'

            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'jenkins-pipeline', message: 'Success'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'jenkins-pipeline', message: 'Job Failed'
        }
    }
}

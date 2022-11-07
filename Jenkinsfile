def JOB_NAME = env.JOB_NAME
def BUILD_NUMBER= env.BUILD_NUMBER

pipeline {
    agent any 
  

    stages {
        stage('Build') {
            steps {
                script{
                    try{
                        slackSend channel: '#builds-jenkins', color: 'good', message: "Start job: ${JOB_NAME} ${BUILD_NUMBER}"
                        echo 'TODO: build'
                        bat 'mvnw clean compile -e'
                        }
                    catch(all){
                        slackSend channel: '#builds-jenkins', color: 'danger', message: "Fail job: ${JOB_NAME} ${BUILD_NUMBER}"
                        }
                    }
            }
        }
        stage('Test') {
            steps {
                echo 'TODO: test'
                bat 'mvnw clean test -e'
            }
        }
        stage('Package') {
            steps {
                echo 'TODO: package'
                bat 'mvnw clean package -e'           
            }
        }
        stage('Run') {
            steps {
                echo 'TODO: run'
                bat 'mvnw spring-boot:run'                      
            }           
        }
        stage('Clean Workspace') {
            steps {     
                cleanWs()
                slackSend channel: '#builds-jenkins', color: 'good', message: "Finish job: ${JOB_NAME} ${BUILD_NUMBER}"
            }           
        }
    }
}

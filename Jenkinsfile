def custom_msg()
{
  def JENKINS_URL= "https://2b7c-2800-150-122-43c-1d26-368f-4267-36a9.sa.ngrok.io/job/ms-iclab/job/feature-estado-pais/"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText"
  return JENKINS_LOG
}

pipeline {
    agent any 
      stages {
        stage('Build') {
            steps {
                slackSend channel: '#builds-jenkins', color: 'good', message: 'Start job'
                echo 'TODO: build'
                bat 'mvnw clean compile -e'
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
                slackSend channel: '#builds-jenkins', color: 'good', message: 'Finish job'
            }           
        }
    }
}

pipeline {
    agent any 
  

    stages {
        stage('Build') {
            steps {
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
                bat 'nohup bash mvnw spring-boot:run &'                      
            }           
        }
        stage('Clean Workspace') {
            steps {     
                cleanWs()
            }           
        }
    }
}

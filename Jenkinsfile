pipeline {
    agent any
    tools {
    	maven 'Maven-3.8.6'
    }
    stages {
		
		stage('Build') {
			steps {
				script {
					try {
						sh 'mvn clean install'
						slackSend channel: '#grupo2', color: 'good', message: "Start job: ${BRANCH_NAME} ${JOB_NAME} ${BUILD_NUMBER} SUCCESS"
					}
					catch(all) {
						slackSend channel: '#grupo2', color: 'danger', message: "Fail Job: ${BRANCH_NAME}/${JOB_NAME}/${BUILD_NUMBER} Step: Build FAIL"
					}
				}	
			}
		}
		stage('Sonar') {
			steps {
				script {      
					try {
						withSonarQubeEnv('SonarTest') {
							sh 'mvn clean package sonar:sonar -Dsonar.projectKey=lab-04 -Dsonar.java.binaries=build'
						}
					}
					catch(all) {
						slackSend channel: '#grupo2', color: 'danger', message: "Fail Job: ${BRANCH_NAME}/${JOB_NAME}/${BUILD_NUMBER} Step: Sonar FAIL"
					}
				}
			}
		}
		stage("Publish to Nexus Repository Manager") {
			when {
				expression{
					"${BRANCH_NAME}"=='main'
				}
			}
			steps {
					script {
						try {
							nexusPublisher nexusInstanceId: 'Nexus-Repository', nexusRepositoryId: 'devops-usach-nexus', 
								packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: "${WORKSPACE}/build/DevOpsUsach2020-0.0.1.jar"]],
								mavenCoordinate: [artifactId: 'DevOpsUsach2020', groupId: 'com.devopsusach2020', packaging: 'jar', version: "1.1.${BUILD_NUMBER}"]]]
						}
						catch(all) {
							slackSend channel: '#grupo2', color: 'danger', message: "Fail Job: ${BRANCH_NAME}/${JOB_NAME}/${BUILD_NUMBER} Step: Publish to Nexus FAIL"
						}
					}
				}
		}  
		stage('Pull the file off Nexus'){
			when {
				expression{
					"${BRANCH_NAME}"=='main'
				}
			}
			steps{
				script {
					try {
						withCredentials([usernameColonPassword(credentialsId: 'NexusKey', variable: 'NEXUS_CREDENTIALS')]){
							sh script: 'curl -u ${NEXUS_CREDENTIALS} -o DevOpsUsach2020-0.0.1.jar "http://nexus:8081/repository/devops-usach-nexus/com/devopsusach2020/DevOpsUsach2020/0.0.1/DevOpsUsach2020-0.0.1.jar"'
						}
					}
					catch(all) {
						slackSend channel: '#grupo2', color: 'danger', message: "Fail Job: ${BRANCH_NAME}/${JOB_NAME}/${BUILD_NUMBER} Step: Pull off nexus FAIL"
					}
				}
			}
		}
		
	    	stage('git push') {
		    	when {
				expression{
					"${BRANCH_NAME}"=='develop'
				}
			}
			steps {
			withCredentials([
			    gitUsernamePassword(credentialsId: 'gitHubKey', gitToolName: 'Default')
			]) {
			   	sh 'git config --global user.email "opmalzahn@gmail.com"'
  				sh 'git config --global user.name "Oriana Pardo"'
				sh 'git tag -a "Release1.0.${BUILD_NUMBER}" -m "V1.0.${BUILD_NUMBER}"'
				
				//sh 'git checkout develop'
				//sh 'git pull'
				sh 'git remote update'
				sh 'git fetch'
				sh 'git checkout --track origin/main'
				sh 'git merge develop main'
				sh 'git commit -am "Merged develop branch to main - ${BUILD_NUMBER}"'
				sh "git push origin main --tag"
			}
		    }
		}
		      
		stage('Clean Workspace') {
			steps {
				cleanWs()
			}
		}
    }
}

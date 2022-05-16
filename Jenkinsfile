pipeline {
    agent any

    stages {
		stage('Clone'){
		
		
		}
		
        stage('Build') {
            steps {
				echo 'Building..'
                sh 'docker build -t builder:latest . -f /var/jenkins_home/workspace/PiplineForDevOps/Docker-build'
				
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
				sh 'docker build -t tester:latest . -f /var/jenkins_home/workspace/PiplineForDevOps/Docker-test'
				
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
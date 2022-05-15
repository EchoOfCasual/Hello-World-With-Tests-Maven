pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
				echo 'Building..'
                sh 'docker-compose build -t builder:latest . -f /Docker-build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
				sh 'docker-compose build -t tester:latest . -f /Docker-test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
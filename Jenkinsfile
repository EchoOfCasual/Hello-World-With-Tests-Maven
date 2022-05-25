pipeline {
    agent any
	
	parameters{
		string(name:'version',defaultValue:'1.0.0', description:'The version of artifact')
		booleanParam(name:'promote',defaultValue: false, description:'Publish new version')
	}

    stages {
		stage('Clone'){
			steps {
				echo 'Cloning.. And setting up voulumes..'
				
				sh 'docker system prune --all --volumes -f'
				
				sh 'docker volume create vol-in'
				sh 'docker volume create vol-out'
				
                sh 'docker build -t cloner:latest . -f /var/jenkins_home/workspace/PiplineForDevOps/Docker-clone'
				sh 'docker run --mount source=vol-in,destination=/inputVol cloner:latest'
			}
		
		}
        stage('Build') {
            steps {
				echo 'Building..'
                sh 'docker build -t builder:latest . -f /var/jenkins_home/workspace/PiplineForDevOps/Docker-build'
				sh 'docker run --mount source=vol-in,destination=/inputVol --mount source=vol-out,destination=/outputVol builder:latest'
				
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
				sh 'docker build -t tester:latest . -f /var/jenkins_home/workspace/PiplineForDevOps/Docker-test'
				sh 'docker run --mount source=vol-in,destination=/inputVol --mount source=vol-out,destination=/outputVol tester:latest'
				
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying..'
				
				sh 'docker build -t deployer:latest . -f /var/jenkins_home/workspace/PiplineForDevOps/Docker-deploy'
				sh 'docker run --name deployC --mount source=vol-out,destination=/outputVol deployer:latest'
				
				sh 'rm -rf artifacts'
				sh 'mkdir artifacts'
				sh 'docker cp deployC:outputVol/SimpleApp.jar ./artifacts'
            }
        }
		
		stage('Publish') {
            steps {
                echo 'Publishing..'
					script{
						if(params.promote){
						 sh 'mv ./artifacts/SimpleApp.jar ./artifacts/SimpleApp-${version}.jar'
                         archiveArtifacts artifacts: 'artifacts/SimpleApp-${version}.jar'
						}
						else{
						 echo 'Pipeline finished work sucessfully but new version wasn\'t published.'
						}
					
					
					}
	
            }
        }
    }
}
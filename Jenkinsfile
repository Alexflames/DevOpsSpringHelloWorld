pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Create image') {
            agent {
                docker {
                    image 'docker:latest'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
            	sh 'ls'
            	script {
		        	sh 'ls'
		        	def customimage = docker.build("SpringHelloWorld")
		        	sh 'ls'
            	}
            }
        }
    }
}

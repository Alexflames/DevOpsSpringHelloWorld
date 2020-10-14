pipeline {
    agent any
    environment {
    	registry = "alexflames/mavenjenkins_ssu"
    	registryCredential = "alexflames"
    }
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /root/.m2:/root/.m2' 
                }
            }
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
            	script {	
			def customimage = docker.build("${registry}:${env.BUILD_ID}")	
            	}
            }
        }
        stage('Deploy image') {
        	steps {
        		script {
        			docker.withRegistry('', registryCredential) {
        				customimage.push()
        			}
        		}
        	}
        }
        stage('Cleaning up') {
        	steps {
        		sh "docker rmi $registry:$BUILD_NUMBER"
        	}
        }
    }
}

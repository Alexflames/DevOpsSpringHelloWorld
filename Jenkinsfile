pipeline {
    agent any
    environment {
    	registry = "alexflames/mavenjenkins_ssu"
    	registryCredential = "alexflames"
    	dockerImage = ''
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
					dockerImage = docker.build("${registry}:${env.BUILD_ID}")	
            	}
            }
        }
        
        stage('Clear and run container') {
        	steps {
		    	script {
	        		try {
        				sh 'docker rmi -f testproject'
        			}
        			catch(all) {
        		
        			} 
        			sh 'docker run -d -p 8181:8080 -n testproject ${registry}:${env.BUILD_ID} 
		    	}
        	}
        }
        stage('Test container') {
        	steps {
        		
        	}
        }
        stage('Deploy image') {
        	steps {
        		script {
        			docker.withRegistry('', registryCredential) {
        				dockerImage.push()
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

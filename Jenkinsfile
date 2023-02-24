pipeline{
    agent any
    environment{
        DOCKERHUB_USERNAME= "avatareleniyan1"
        APP_NAME="gitop"
        IMAGE_TAG="${BUILD_NUMBER}"
        IMAGE_NAME="${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS="dockerhubID"
    }
    stages{
        stage('Clean workspace area'){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage ('Check-Git-Secrets') {
		    steps {
                script{
                    sh 'rm trufflesecurity/trufflehog:latest || true'
                    sh 'docker run --rm -t -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --repo https://github.com/avatareleniyan1/gitop-ci.git --debug > trufflehog'
		            sh 'cat trufflehog'

               }
	         
	       }
        }
        stage('Git Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'gitop',
                    url: 'https://github.com/avatareleniyan1/gitop-ci.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage ('Source-Composition-Analysis (SCA)') {
		    steps {
		     sh 'rm owasp-* || true'
		     sh 'wget https://raw.githubusercontent.com/devopssecure/webapp/master/owasp-dependency-check.sh'	
		     sh 'chmod +x owasp-dependency-check.sh'
		     sh 'bash owasp-dependency-check.sh'
		     sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
		   }
	   }
       stage ('Network Mapper (Port Scan)') {
		    steps {
			sh 'rm nmap* || true'
			sh 'docker run --rm -v "$(pwd)":/data uzyexe/nmap -sS -sV -oX nmap http://10.0.0.98:8080/'
			sh 'cat nmap'
		    }
	    }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUIlD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker Images'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }

    }  
}
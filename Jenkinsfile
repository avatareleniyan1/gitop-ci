pipeline{
    agent any
    environment{
        DOCKERHUB_USERNAME= "avatareleniyan1"
        APP_NAME="gitop-ci"
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
                    sh 'trufflesecurity/trufflehog:latest || true'
		            sh 'docker pull trufflesecurity/trufflehog:latest'
                    sh 'docker run -t -v "$PWD:/pwd" ghcr.io/trufflesecurity/trufflehog:latest github --repo  https://github.com/avatareleniyan1/gitop-ci.git --debug > trufflehog'
		            sh 'cat trufflehog'

                }
	         
	       }
        }
        stage('Git Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'githubtoken',
                    url: 'https://github.com/avatareleniyan1/gitops-ci.git',
                    branch: 'main'
                }
            }
        }
    }  
}
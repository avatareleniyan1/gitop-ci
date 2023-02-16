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
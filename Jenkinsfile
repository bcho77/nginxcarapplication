pipeline
{
    agent any

    environment{
        DOCKER_USERNAME = "vaninoel"
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
        RELEASE = "1.0.0"
        APP_NAME = "carvilla"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKER_USERNAME}" + "/" + "${APP_NAME}"
        DOCKER_PASS = 'DockerCredential' 

        
    }
    stages
    { 
            
         stage('chekout from scm')
        {
            steps
            {
                git branch: 'main', 
                url: 'https://github.com/bcho77/nginxcarapplication.git'
            }
        }
        stage('Build docker image'){
            steps{
                sh 'docker build -t ${APP_NAME}:${BUILD_NUMBER} .'
                sh 'docker tag ${APP_NAME} ${IMAGE_NAME}:latest'
                
                withCredentials([string(credentialsId: 'DOCKER_CREDENT', variable: 'dockerhub')]) {
                sh 'docker login -u vaninoel -p $dockerhub'
            }
            }
            
        }
        stage('push'){
            steps{
                sh 'docker push ${IMAGE_NAME}:latest'
            }
        }
       
    }
        
}   

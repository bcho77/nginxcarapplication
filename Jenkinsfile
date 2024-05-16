pipeline
{
    agent {
        label 'maven'
    }
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
                git branch: 'main', credentialsId: 'GITHUB_CREDENT', 
                url: 'https://github.com/bcho77/nginxcarapplication'
            }
        }
         stage('Build ')
        {
           steps{
            script {
            withCredentials([usernamePassword(credentialsId: 'DockerCredential', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'PASSWORD')]) {
                docker.withRegistry('', "${DOCKER_USERNAME}:${PASSWORD}") {
                    docker.build("${IMAGE_NAME}")
                    docker.image("${IMAGE_NAME}").push("${IMAGE_TAG}")
                    docker.image("${IMAGE_NAME}").push('latest')
                }
            }

          
            }
        }
        
             }   }
}
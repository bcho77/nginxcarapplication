pipeline
{
    agent {
        label 'maven'
    }
    environment{
        DOCKER_USERNAME = "vaninoel"
       // BUILD_NUMBER = "${env.BUILD_NUMBER}"
        RELEASE = "1.0.0"
        APP_NAME = "carvilla"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKER_USERNAME}" + "/" + "${APP_NAME}"
        DOCKER_PASS = 'DOCKER_CREDENT'
        
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
         stage('Build and push')
        {
            steps
            {
                script{
                    docker.withRegistry('',DOCKER_PASS){
                        docker.build"${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }

                }
            }
        }
        
    }
}
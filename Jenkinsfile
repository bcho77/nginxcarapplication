pipeline
{
    agent any

    environment{
        DOCKER_USERNAME = "vaninoel"
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
        RELEASE = "1.0.0"
        APP_NAME = "carvilla"
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
                sh 'docker build -t $APP_NAME:$BUILD_NUMBER .'
                sh 'docker tag $APP_NAME:$BUILD_NUMBER $IMAGE_NAME:$BUILD_NUMBER'
                  
                withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDENT', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASS')]) {
                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASS'
            }
            }
            
        }
        stage('push'){
            steps{
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                
            }
        }
        stage('Trigger ManifestUpdate'){
            steps{
                echo "triggering udpdatemanifestjob"
                build job: 'cdcarwebb' , parameters: [string (name:BUILD_NUMBER, value: env.BUILD_NUMBER )]
            }
        }
       
    }

        
}   

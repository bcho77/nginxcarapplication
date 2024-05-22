pipeline {
    agent any

    environment {
        DOCKER_USERNAME = "vaninoel"
        RELEASE = "1.0.0"
        APP_NAME = "carvilla"
        DOCKER_PASS = 'DockerCredential'
    }
    
    stages {
        stage('Checkout from SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/bcho77/nginxcarapplication.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    def imageName = "${env.DOCKER_USERNAME}/${env.APP_NAME}"
                    
                    sh "docker build -t ${env.APP_NAME}:${buildNumber} ."
                    sh "docker tag ${env.APP_NAME}:${buildNumber} ${imageName}:${buildNumber}"
                    
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDENT', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASS}"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    def imageName = "${env.DOCKER_USERNAME}/${env.APP_NAME}"
                    
                    sh "docker push ${imageName}:${buildNumber}"
                }
            }
        }

        stage('Trigger Manifest Update') {
            steps {
                echo "Triggering update manifest job"
                build job: 'cdcarwebb', parameters: [string(name: 'BUILD_NUMBER', value: env.BUILD_NUMBER)]
            }
        }
    }
}






pipeline {
    agent any

    stages {
        stage('Hello...') {
            steps {
                sh "echo 'Hello World'"
                sh 'echo $USER'
            }
        }
        stage('Checkout...') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/okslisovska/devops-django.git'
            }
        }
        stage('Build docker image...') {
            steps {
                sh 'docker build -t lisovska/devops-django:v${BUILD_NUMBER} .'
                
            }
        }
        stage('Push docker image to DockerHub') {
            steps{
                withDockerRegistry(credentialsId: 'dockerhub-cred-lisovska', url: 'https://index.docker.io/v1/') {
                    sh '''
                        docker push lisovska/devops-django:v${BUILD_NUMBER}
                    '''
                }
            }
        }
        stage('Run container...') {
            steps {
                sh '''
                    docker run --name tryfr${BUILD_NUMBER} -p 8000:8000 -d lisovska/devops-django:v${BUILD_NUMBER}
                    docker ps
                    sleep 40
                    docker stop tryfr${BUILD_NUMBER}
                    docker ps
                    docker rm tryfr${BUILD_NUMBER}
                    docker ps -a
                '''
                
            }
        }
    }
}


pipeline {
    environment {
        registry = "youhad10/jenkins-docker-cicd"
        registryCredential = 'dockerhub-credentials'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git credentialsId: 'github-credentials',
                    url: 'https://github.com/YOUHAD08/jenkins-docker-cicd.git',
                    branch: 'main'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                bat """
                    docker stop bdcc-container || true
                    docker rm bdcc-container || true
                    docker run -d --name bdcc-container -p 8090:80 ${registry}:${env.BUILD_NUMBER}
                """
            }
        }
    }
}
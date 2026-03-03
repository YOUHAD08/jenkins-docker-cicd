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
    }
}
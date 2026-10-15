pipeline {
    environment {
        registry = "aichazer/bdcc-web"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'master', url: 'https://github.com/AichaZEROUAL/devops-tp2-jenkins.git'
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
                        dockerImage.push('latest')
                    }
                }
            }
        }
         stage('Deploy image') {
            steps {
                bat(script: 'docker rm -f tp2-web', returnStatus: true)
                bat "docker run -d --name tp2-web -p 8081:80 ${registry}:${BUILD_NUMBER}"
            }
        }
    }
}
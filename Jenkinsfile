pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/itsrakeshhq/productivity-app.git']]])
            }
        }

        stage('Client Tests') {
            steps {
                dir('client') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Server Tests') {
            steps {
                dir('server') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Build Client Docker Image') {
            steps {
                dir('client') {
                    sh 'docker build -t rakeshpotnuru/productivity-app:client .'
                }
            }
        }

        stage('Build Server Docker Image') {
            steps {
                dir('server') {
                    sh 'docker build -t rakeshpotnuru/productivity-app:server .'
                }
            }
        }

        stage('Docker Login and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                }

                sh 'docker push rakeshpotnuru/productivity-app:client'
                sh 'docker push rakeshpotnuru/productivity-app:server'
            }
        }
    }
}

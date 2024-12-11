pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'avish031/dvna'
        MYSQL_IMAGE = 'mysql:5.7' 
        MYSQL_USER='dvna'
        MYSQL_DATABASE='dvna'
        MYSQL_PASSWORD='passw0rd'
        MYSQL_RANDOM_ROOT_PASSWORD='yes'

        // KUBECONFIG = credentials('kubeconfig') // Add kubeconfig as Jenkins credential
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the repository
                git 'https://github.com/031-Avish/jenkins-client-server'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                """
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                
                withDockerRegistry([credentialsId: 'dockerhubcred', url: '']) {
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }
    }
}
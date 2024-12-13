pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'avish031/avishrepo'
        MYSQL_IMAGE = 'mysql:5.7' 
        MYSQL_USER='dvna'
        MYSQL_DATABASE='dvna'
        MYSQL_PASSWORD='passw0rd'
        MYSQL_RANDOM_ROOT_PASSWORD='yes'
        DOCKERHUB_CREDENTIALS= credentials('dockerhubcred')
        KUBECONFIG = credentials('kubeconfig')     
}
        // KUBECONFIG = credentials('kubeconfig') // Add kubeconfig as Jenkins credential
    
    tools{
        dockerTool 'docker'
    }
    stages {
        // stage('Checkout Code') {
        //     steps {
        //         // Checkout the code from the repository
        //         git 'https://github.com/031-Avish/jenkins-client-server'
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                sh """
                
                docker build -t avish031/avishrepo:${BUILD_NUMBER} .
                """
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                 
	            echo 'Login Completed' 

                sh 'sudo docker push avish031/avishrepo:$BUILD_NUMBER'          
                echo 'Push Image Completed' 
                }
        }
        
        stage('check agent') {
            agent { label 'jenkins-slave' }  // Specify the agent label for this stage
            steps {
                
                    echo "working"
                    echo "Running on agent: ${env.NODE_NAME}"
                    sh 'echo hello world'
                    sh 'mkdir hello'
                    sh """
                    curl -LO https://dl.k8s.io/release/v1.26.0/bin/linux/amd64/kubectl
                        chmod +x kubectl
                        mkdir -p \$HOME/bin
                        mv kubectl \$HOME/bin/
                    """
                    sh """
                    export PATH=\$HOME/bin:\$PATH
                    sed -i 's|image: avish031/avishrepo:.|image: avish031/avishrepo:${BUILD_NUMBER}|' k8s/deployment.yaml
                    kubectl apply -f k8s/deployment.yaml
                    """
                    
                // kubectl delete secret app-secrets
                //     kubectl create secret generic app-secrets \\
                //         --from-literal=MYSQL_USER=${MYSQL_USER} \\
                //         --from-literal=MYSQL_PASSWORD=${MYSQL_PASSWORD} \\
                //         --from-literal=MYSQL_DATABASE=${MYSQL_DATABASE} \\
                //         --from-literal=BUILD_NUMBER=${BUILD_NUMBER}
            }
        }

    }
}
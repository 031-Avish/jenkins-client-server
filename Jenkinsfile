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
        KUBECONFIG = credentials(kubeconfig)     
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

        // stage('Build Docker Image') {
        //     steps {
        //         sh """
                
        //         docker build -t avish031/avishrepo:${BUILD_NUMBER} .
        //         """
        //     }
        // }

        // stage('Push Docker Image to Docker Hub') {
        //     steps {
        //         sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                 
	    //         echo 'Login Completed' 

        //         sh 'sudo docker push avish031/avishrepo:$BUILD_NUMBER'          
        //         echo 'Push Image Completed' 
        //         }
        // }
        
        stage('check agent') {
            agent { label 'jenkins-slave' }  // Specify the agent label for this stage
            steps {
                container('kubectl') {  
                    echo "working"
                    echo "Running on agent: ${env.NODE_NAME}"
                    sh 'echo "$KUBECONFIG" > ~/.kube/config'
                    sh 'kubectl get pod'
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }

    }
}
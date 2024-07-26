pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "your_docker_registry/${env.JOB_NAME}:${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the registry
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Update the deployment.yaml file with the new image tag
                    sh "sed -i 's|image: .*$|image: ${DOCKER_IMAGE}|' k8s/deployment.yaml"
                    
                    // Apply the updated deployment.yaml to the Kubernetes cluster
                    sh "kubectl apply -f k8s/deployment.yaml --namespace your-namespace"
                }
            }
        }
    }
}

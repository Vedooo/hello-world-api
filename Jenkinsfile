pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "vedooo/${env.JOB_NAME}:dev-1"
        KUBECONFIG = '/var/jenkins_home/.kube/config'
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git 'https://github.com/Vedooo/hello-world-api/tree/main'
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
                    sh "sed -i 's|image: .*\$|image: ${DOCKER_IMAGE}|' k8s/deployment.yaml"
                    
                    // Apply the updated deployment.yaml to the Kubernetes cluster
                    sh "kubectl apply -f k8s/deployment.yaml --namespace dev"
                }
            }
        }
    }
}

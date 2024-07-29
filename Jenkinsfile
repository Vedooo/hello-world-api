pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "vedooo/${env.JOB_NAME}:dev-1"
        KUBECONFIG = '/var/jenkins_home/.kube/config'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Vedooo/hello-world-api.git'
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
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "sed -i 's|image: .*\$|image: ${DOCKER_IMAGE}|' k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/deployment.yaml --namespace dev"
                }
            }
        }
    }
}

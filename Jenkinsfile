pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                // Checkout the source code from the GitHub repository
                git branch: 'main', url: 'https://github.com/n0f4ce/microservices-demo.git', credentialsId: 'github'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes manifests
                script {
                    bat 'kubectl apply ./release/kubernetes-manifests.yaml'
                }
            }
        }

        stage('Get Pods Status') {
            steps {
                // Get pods status
                script {
                    bat 'kubectl get pods'
                }
            }
        }
    }
}

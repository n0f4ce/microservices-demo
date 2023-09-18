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
                    bar 'kubectl config get-contexts'
                    bat 'kubectl apply -f ./release/kubernetes-manifests.yaml --context docker-desktop'
                }
            }
        }

        stage('Get Pods Status') {
            steps {
                // Get pods status
                script {
                    bat 'kubectl get pods --context docker-desktop'
                }
            }
        }
    }
}

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
                    sh '/usr/local/bin/kubectl config get-contexts'
                    sh '/usr/local/bin/kubectl apply -f ./release/kubernetes-manifests.yaml -n jenkins'
                }
            }
        }

        stage('Get Pods Status') {
            steps {
                // Get pods status
                script {
                    sh '/usr/local/bin/kubectl get pods -n jenkins'
                }
            }
        }
    }
}

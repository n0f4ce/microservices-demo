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
                    sh 'ls -l /tmp'
                    sh '/tmp/kubectl config get-contexts'
                    sh '/tmp/kubectl apply -f ./release/kubernetes-manifests.yaml -n jenkins'
                }
            }
        }

        stage('Get Pods Status') {
            steps {
                // Get pods status
                script {
                    sh '/tmp/kubectl get pods -n jenkins'
                }
            }
        }
    }
}

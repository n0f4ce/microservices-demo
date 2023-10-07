podTemplate(cloud: 'minikube1') {
  node(POD_LABEL) {
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
                    // Run the 'kubectl apply' command one by one, waiting for each command to finish before running the next one
                    sh '/tmp/kubectl apply -f ./release/kubernetes-manifests.yaml -n jenkins --wait=true'
                }
            }
        }

        stage('Get Pods Status') {
            steps {
                // Get pods status
                script {
                    // Set 'wait: false' to ensure that the Jenkins pipeline does not wait for this stage to finish before moving on to the next stage
                    sh '/tmp/kubectl get pods -n jenkins --wait=false'
                }
            }
        }
    }
  }
}

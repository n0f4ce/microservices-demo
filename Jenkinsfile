pipeline {
    agent {
        kubernetes {
            cloud 'minikube1'
            label 'jenkins-agent-1'
            defaultContainer 'jnlp'
            containerCap: 10
        }
    }

    stages {
        stage('Set Number of Executors') {
            steps {
                // Set the number of executors on the Jenkins agent
                sh """
                def agentName = 'jenkins-agent-1'
                def numExecutors = 2

                try {
                    def node = Jenkins.instance.getNode(agentName)
                    if (node != null) {
                        node.setNumExecutors(numExecutors)
                    }
                } catch (Exception e) {
                    echo "setNumExecutor failed"
                }
                """
            }
        }
      
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

pipeline {
  agent {
    kubernetes {
      cloud 'minikube1'
      label 'jenkins-agent-1'
    }
  }

  environment {
    GIT_EXECUTABLE='/usr/bin/git'
  }

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
                    sh 'ls -l /home/jenkins/agent/workspace/demo-pipeline'
                    // Run the 'kubectl apply' command one by one, waiting for each command to finish before running the next one
                    sh 'kubectl apply -f ./release/kubernetes-manifests.yaml -n jenkins'
                }
            }
        }

        stage('Get Pods Status') {
            steps {
                // Get pods status
                script {
                    // Set 'wait: false' to ensure that the Jenkins pipeline does not wait for this stage to finish before moving on to the next stage
                    sh 'kubectl get pods -n jenkins'
                }
            }
        }
    }
}

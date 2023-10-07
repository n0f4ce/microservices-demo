pipeline {
    agent {
        kubernetes {
            cloud 'minikube1'  // Specify the Kubernetes cloud by name
            label 'jenkins-agent-1'  // Specify the agent label for the build
            defaultContainer 'jnlp'
            yaml """
            apiVersion: v1
            kind: Pod
            metadata:
              labels:
                jenkins: agent
                jenkins/label: jenkins-agent-1
              namespace: jenkins  // Specify the namespace
            spec:
              containers:
              - name: jnlp
                image: jenkins/inbound-agent:latest
                imagePullPolicy: IfNotPresent
                workingDir: "/home/jenkins/agent"
                resources:
                  requests:
                    cpu: "512m"
                    memory: "512Mi"
                securityContext:
                  runAsUser: 1000
                  runAsGroup: 1000
                  fsGroup: 1000
              serviceAccountName: jenkins  // Specify the service account
            """
            containerCap: 10  // Specify the maximum number of executors
        }
    }

    stages {

        stage('Set Number of Executors') {
            agent {
                label 'jenkins-agent-1'
            }
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
            agent {
                label 'jenkins-agent-1'
            }
            steps {
                // Checkout the source code from the GitHub repository
                git branch: 'main', url: 'https://github.com/n0f4ce/microservices-demo.git', credentialsId: 'github'
            }
        }

        stage('Deploy to Kubernetes') {
            agent {
                label 'jenkins-agent-1'
            }
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
            agent {
                label 'jenkins-agent-1'
            }
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

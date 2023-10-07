pipeline {

  stages {
    stage('Checkout Source') {
      steps {
        node 'master' {
          git branch: 'main', url: 'https://github.com/n0f4ce/microservices-demo.git', credentialsId: 'github'
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        podTemplate(cloud: 'minikube') {
          container('builder') {
            image 'docker/kubectl:v1.25.3'
            command 'kubectl apply -f ${WORKSPACE}/release/kubernetes-manifests.yaml -n jenkins --wait=true'
          }
        }
      }
    }
  }
}

podTemplate(cloud: 'minikube1') {
  node(POD_LABEL) {
    stage('Checkout Source') {
      git branch: 'main', url: 'https://github.com/n0f4ce/microservices-demo.git', credentialsId: 'github'
    }
  }
}

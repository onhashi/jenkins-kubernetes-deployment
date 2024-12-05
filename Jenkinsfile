p peline {
  environment {
    dockerimagename = "santanarulez/react-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/onhashi/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {

          withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'local-kind', contextName: 'kind-kind', credentialsId: 'kind-credentials', namespace: 'default', serverUrl: 'https://kind-control-plane:6443/']]) {
            sh 'kubectl get nodes'
            sh 'kubectl cluster-info'
          }
          
         // kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }
  }
}

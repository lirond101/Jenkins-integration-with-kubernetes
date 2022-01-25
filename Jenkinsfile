pipeline {

  environment {
    dockerimagename = "lirondadon/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/lirond101/nodeapp_test.git'
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
              registryCredential = 'dockerhublogin'
          }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        withCredentials([
            string(credentialsId: 'minikube', variable: 'api_token')
            ]) {
            sh './kubectl --token $api_token --server https://192.168.99.103:8443 --insecure-skip-tls-verify=true apply -f deploymentservice.yml '
              }
        }
    }

  }
}

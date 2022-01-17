pipeline {

  environment {
    dockerimagename = "paksalman/nodejs-example"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Git Checkout') {
      steps {
        git branch:'main', url:'https://github.com/paksalman/nodejs-example.git'
      }
    }

    stage('Build Docker Image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Push Image') {
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

    stage('Deploy to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "nodejs-deploy.yml", kubeconfigId: "kubernetes1")
        }
      }
    }

  }

}

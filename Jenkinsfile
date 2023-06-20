pipeline {

  environment {
    dockerimagename = "aweit/react-app"
    dockerImage = ""
  }

  agent{
    node{
      label 'slave-pipeline'
    }
  }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/aweit-zhu/jenkins-kubernetes-deployment.git'
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
        container('docker') {
          script {
            kubernetesDeploy(configs: "deployment.yaml"."service.yaml")
          }
        }
      }
    }

  }

}
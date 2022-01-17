pipeline {

  environment {
    dockerimagename = "javatechie/springboot-k8s-demo"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/basanta-spring-boot/k8s-cicd.git'
      }
    }


    stage('Build image') {
      steps{
        script {
          dockerImage = sudo docker.build dockerimagename
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
        script {
          kubernetesDeploy(configs: "k8s-deployment.yaml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
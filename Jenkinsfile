pipeline {

  environment {
    registry = "192.168.43.26:5000/sandiptest/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        sh 'echo "$BUILD_NUMBER"'
        git 'https://github.com/sandipchatterjee540/play-jenkins-k8s.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "KUBECONFIG")
        }
      }
    }

  }

}

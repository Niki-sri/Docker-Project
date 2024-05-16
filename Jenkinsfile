pipeline {

  environment {
    registry = "docker.io/nikila407/flask"
    registry_mysql = "docker.io/nikila407/mysql"
    dockerImage = ""
    registryCredential = "dockerhub" 
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/Niki-sri/Docker-Project'
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
          docker.withRegistry( "", registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       script {
               dockerImage = docker.build registry_mysql + ":$BUILD_NUMBER"  '"$WORKSPACE"/mysql'
         docker.withRegistry( "", registryCredential) {
            dockerImage.push()
         }       
        }
      }
   }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}

pipeline {  
 environment {
    registry = "alexshlam/express-web"
    registryCredential = "dockerhub"
  }  
  agent any  
  stages {
    stage("Cloning Git") {
      steps {
        git "https://github.com/alexshlam/express-web.git"
      }
    }
    stage("Building image") {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                     dockerImage.push()  
                    }
                }
            }
    } 
  }
}
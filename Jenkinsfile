pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          docker.build(DOCKER_IMAGE)
        }

      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            dockerImage = docker.image(DOCKER_IMAGE)
            dockerImage.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            dockerImage.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
          }
        }

      }
    }

  }
  environment {
    DOCKER_IMAGE = 't0dorov/kiii_jenkins'
  }
}
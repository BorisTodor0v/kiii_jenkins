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
          def app = docker.build("t0dorov/kiii_jenkins")
          // Set 'app' as an environment variable for later use
          env.APP_IMAGE = app
        }

      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            // Retrieve 'app' from environment variable
            def app = env.APP_IMAGE
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
          }
        }

      }
    }

  }
}
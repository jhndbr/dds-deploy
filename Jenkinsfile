pipeline {
  agent any
  environment {
    // Be sure to replace "felipelujan" with your own Docker Hub username
    DOCKER_IMAGE_NAME = "jhndbr/deploys"
    KUBECONFIG = credentials('kubeconfig')
  }
  stages {
    stage('Build Docker Image') {
      when {
        branch 'main'
      }
      steps {
        node {
          checkout scm
          script {
            docker.withServer('tcp://swarm.example.com:2376', 'swarm-certs') {
              app = docker.build(DOCKER_IMAGE_NAME)
            }
          }
        }
      }
    }
  }
